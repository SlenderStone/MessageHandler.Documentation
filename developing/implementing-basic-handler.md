## Implementing a basic handler

In this document we'll look at the basics of handler development, explain the interfaces available in our SDK and show you how to use them to implement message processing logic. In essence a handler does only one thing: respond to interesting information that is flying by. But if you look at this more carefully, you'll see that this single thing actually implies doing 2 things.

 * First the handler needs to look at every message and determine whether it is interesting or not
 * Then it has to act on those messages that have been deemed interesting
 
### What we will do in this tutorial
 
 In this document you will learn how to implement both of these requirements as well as make some of the aspects of a handler configurable by other users.

 * Detecting interesting messages
 * Taking action
 * Define configuration settings
 
### Detecting interesting messages

In order, for a handler, to determine whether certain data is worth acting upon, it needs to inspect this data first. 

In a relatively simple, non-distributed system, where all data is stored in one place, this problem is typically solved by sending the question to the place where the data is.
However, when the data is in transit, which is typical in a distributed, real time environment, this can't be done. We can't bring the query to the data as the data is  moving around and will have disappeared by the time our query would arrive.

To solve this problem, we route the data to the query whenever it is passing by. This means that handlers can only look at a message whenever it is flying by and (conceptually) only do this once for a brief amount of time. We call this concept a standing query and it is represented in our SDK by the type `IStandingQuery<Tin, TOut>`. 
So if you want to inspect the stream of messages flowing by, all you need to do is implement this interface to give us the query that you want to run. Next to this interface in our SDK, we're leveraging the power of the amazing Reactive Extensions framework, which has been specifically created to create this type of query.

Therefore `IStandingQuery<TIn, TOut>` has only one member:

* `IObservable<TIn> Compose(IObservable<TOut> messages)` : Which allows you to compose your standing query using Rx and return it to our runtime

Before diving into the details of building an Rx query, we have to consider that messages are nothing more than bits on the wire flying by. These bits could represent any kind of data, in any format, in any encoding and could be encrypted as well. However, when you build your handler, you'll probably have some assumptions in mind of what is on the wire. The first type parameter of the IStandingQuery interface (Tin) allows you to communicate those assumptions to our infrastructure. The following options are available right now:

 * **byte[]**: Assumes that the data represents a byte array in any encoding
 * **Stream**: A memory stream wrapper around this byte array
 * **string**: Assumes the data is an UTF8 encoded string value
 * **DynamicJsonObject**: Assumes the data is an UTF8 encoded string value representing a JSON serialized object. This object is addressable as a dynamic type in your handler code
 * **IMyType**: Any interface type, assumes the data is an UTF8 encoded string value representing a JSON serialized object with the same member shape as the interface. As members can be nullable, we cannot possibly validate if this assumption is true, so do not assume that the object is filled up correctly
 * **MyType**: Any concrete class type, assumes the data is an UTF8 encoded string value representing a JSON serialized object with the same member shape as the concrete type. As members can be nullable, we cannot possibly validate if this assumption is true, so do not assume that the object is filled up correctly
 
The type that comes out of the query can be different from the input type. This means that a standing query is allowed to perform any kind of transformation on the incoming data as it sees fit, no matter if it is a direct transformation, aggregation, creation or reduction... go ahead. However we do assume that any kind of transformation is performed in memory and will not involve any IO. The goal of the standing query is to reduce the actionable information as fast as possible so the rest of the execution time can be spent performing useful actions. Therefore calling this query is an unconstrained, unthrottled operation and we may invoke it at a rate of several hundreds of thousands calls per second or more. We're pretty sure no remote service is not going to like that and neither will the tcp sockets on our machines.

Implementing the IStandingQuery interface is pretty straightforward, with this example we would simply like to show off the power of the Rx framework and how it can be used to turn and onslaught of data into actionable information. 

 <!-- start of code block -->
 
	public class AverageMeasurementsQuery : IStandingQuery<DynamicJsonObject, AverageMeasurement>
    {
    	public IObservable<AverageMeasurement> Compose(IObservable<DynamicJsonObject> measurements)
    	{
    		return from dynamic e in measurements
    			   where IsConcreteMeasurement(e)
    			   group e by e.MeasurementType into c
    			   from v in c.Buffer(TimeSpan.FromSeconds(30), TimeSpan.FromSeconds(30))
    			   where v.Count > 0
    			   select new AverageMeasurement
    			   {
    				   MeasurementType = v.First().MeasurementType, 
    				   AggregationMode = "Average", 
    				   Amount = v.Average(t => t.Amount), 
    				   Unit = v.First().Unit
    			   };
    	}
    
    	private bool IsConcreteMeasurement(dynamic m)
    	{
    		return m.MeasurementType != null && m.Unit != null && m.Amount >= 0 && m.AggregationMode == null;
    	}
    }
	
	public class AverageMeasurement
    {
        public double Amount { get; set; }
        public string Unit { get; set; }
        public string MeasurementType { get; set; }
        public string AggregationMode { get; set; }
    }
    
<!-- end of code block -->

In this example, we tell the handler runtime that we expect some JSon serialized message to come in and we'll turn it into a AverageMeasurement instances. AverageMeasurement is a simple class with some value properties on it, that is actually actionable. What is more interesting is how we get from a stream of unknown json serialized messages to the actionable information using Rx.

In a first step of our standing query we turn every DynamicJsonObject instance that we'll see in the stream into a dynamically evaluated instance using the C# dynamic keyword, this allows us to inspect json values as if they were strongly typed properties for convenience.

	from dynamic e in measurements

But before we can use the properties, that we assume will be present on the messages, we need to validate whether they are actually there, that's what the call to `IsConcreteMeasurement` is all about. It inspects the presence or absence of certain properties to determine whether this is a concrete measurement message or not. 

	private bool IsConcreteMeasurement(dynamic m)
	{
		return m.MeasurementType != null && m.Unit != null && m.Amount >= 0 && m.AggregationMode == null;
	} 

Note that it also checks for absence of the AggregationMode property. Note that the shape of the data structure of the incoming messages and that of the outgoing message are exactly the same if nullable values would be allowed. JSon doesn't have a standard way for specifying types, in fact javascript doesn't have type safety at all, and therefore we chose not to rely on any of the hacks implemented by common .Net libraries (like $type property hack in json.net). We've included this 'feature', on purpose obviously, to illustrate that handlers process all the messages in a channel, even the ones they send themselves and that we chose explicitly not to tie ourselves to .net specific implementation details as the majority of our clients are not .net native at all.

After we've filtered out all the definitely uninteresting messages, it's time to turn the data in the stream into more meaningful (and less) information. Here the full power of Rx shines, we've grouped the messages by measurement type, buffered them up in a time window of 30 seconds, than aggregate all the values of the Amount property using an Average function for all messages within a group and finally transformed this information in a stream of AverageMeasurement messages (Assuming that they are all expressed in the same unit, but you get the picture).

	group e by e.MeasurementType into c
    from v in c.Buffer(TimeSpan.FromSeconds(30), TimeSpan.FromSeconds(30))
    where v.Count > 0
    select new AverageMeasurement
    {
    	MeasurementType = v.First().MeasurementType, 
    	AggregationMode = "Average", 
    	Amount = v.Average(t => t.Amount), 
    	Unit = v.First().Unit
    };
	
### Taking action

Once you have determined which messages are deemed interesting, you should also do something with those messages. This step is called the action, represented by `IAction<T>` in our SDK. An action could be anything, send the message to some external service, a datastore, a line of business system, or just push it back into the channel for further processing. In our example we're just going to do that, push it back into the channel.

`IAction<T>` has 2 members:

 * `Task Action(T msg)`: Is invoked for every messages that leaves the standing query, and the ideal place to perform any action. Usually this is an IO bound operation towards an external service, or adding the operation into a batch for those services that support batching.
 * `Task Complete()`: Signals the completion of a batch of received messages. For external services that support batching this would be the ideal moment to flush your batch.
 
 In our implementation, we'll simply push the message back into the channel. The SDK provides an IChannel implementation specifically for this purpose, and it also comes equipped with a built in IoC container, so that you can simply make use of constructor injection to get hold of an instance of IChannel.
 
<!-- code block -->

	public class PushIntoChannelAction : IAction<AverageMeasurement>
    {
        private readonly IChannel _channel;

        public PushIntoChannelAction(IChannel channel)
        {
            _channel = channel;
        }

        public async Task Action(AverageMeasurement msg)
        {
			Trace.TraceInformation("Pushing an average measurement into the channel");
		
            await _channel.Push(msg);
        }

        public async Task Complete()
        {
            await Task.FromResult(0); // empty async implementation
        }
    }

<!-- end of code block -->

Note that our SDK also supports .Net Diagnostics Trace logging, anything you write to the Trace infrastructure will be automatically sent to our logging channel, processed for interesting events and stored for you in a convenient way and made accessible in our administrative UI.

### Define configuration settings

Once you have finished your query and action, you probably want to allow some of the parameters to be configurable by the end user of your handler. We support this by leveraging the default .net configuration system. Simply define a configuration section with configuration properties and it's meta data and use the configuration section in your code as you typically would.

Whenever you upload your handler into our system, or as part of our continuous integration system, we will extract the metadata from your this configuration section and set up our internal structures in such a way that end users can manage the values that will eventually show up in your configuration section at runtime.

<!-- code block -->

	public class AverageInPeriodHandlerConfig : ConfigurationSection
    {
        [ConfigurationProperty("DurationInSeconds", IsRequired = true, DefaultValue = 30)]
        public int DurationInSeconds
        {
            get { return (int)this["DurationInSeconds"]; }
            set { this["DurationInSeconds"] = value; }
        }
    }

<!-- end of code block -->

Note that the origin of the configuration section is different depending on the runtime you're using to run your handler.
 
 * When running in the emulator, the values will come from your app.config file
 * But when running inside MessageHandler, the values will come from within our infrastructure (based on end user input). Note that your app.config file is NOT used when running inside MessageHandler

In order for us to switch the source dynamically, our SDK provides an `IConfigurationSource` abstraction, that gives you access to the config section.
 
<!-- code block -->

	private readonly AverageInPeriodHandlerConfig _config;
        
    public AverageMeasurementsQuery(IConfigurationSource configurationSource)
	{
		_config = configurationSource.GetConfiguration<AverageInPeriodHandlerConfig>();
	}

<!-- end of code block -->

Once you have your configsection instance, all you need to do now is use it's values where appropriate