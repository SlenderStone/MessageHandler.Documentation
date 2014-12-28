## AMQP

According to wikipedia: *"The Advanced Message Queuing Protocol (AMQP) is an open standard application layer protocol for message-oriented middleware. The defining features of AMQP are message orientation, queuing, routing (including point-to-point and publish-and-subscribe), reliability and security. AMQP is a binary, application layer protocol, designed to efficiently support a wide variety of messaging applications and communication patterns. It provides flow controlled, message-oriented communication with message-delivery guarantees such as at-most-once (where each message is delivered once or never), at-least-once (where each message is certain to be delivered, but may do so multiple times) and exactly-once (where the message will always certainly arrive and do so only once), and authentication and/or encryption based on SASL and/or TLS. It assumes an underlying reliable transport layer protocol such as Transmission Control Protocol (TCP)."*

AMQP is our internal protocol of choice as it is extremely suitable for server to server messaging and has high reliability and security requirements. To learn more about AMQP itself, please refer to [the wikipedia document on Amqp](http://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol)

### Sending messages

Before you try send messages using Amqp, make sure you have created an endpoint that has the appropriate authorizations! See [Endpoints](/documentation/connectivity/endpoints) for more details.

To send a message using the AMQP protocol, you probably need to use an external client library that supports the AMQP 1.0 protocol. We used the Azure ServiceBus SDK in our example code, but there are others out there as well.

For sending messages to us using AMQP, we have chosen to leverage Azure ServiceBus eventhubs. These constructs allow sending up to a million messages per second to our gateway per eventhub (and we have multiple of these, so no need for you to worry about scale). 

But the ASB SDK will require the information embedded in the AMQP inbound URI at different parts in it's API, so first you will need to disect it. If your uri would look like this `amqps://messagehandler-gw-eu-west-1.servicebus.windows.net/mh-gw-eh-in-1`, then the `messagehandler-gw-eu-west-1` is called the namespace and `mh-gw-eh-in-1` is the name of the hub.

Once you have determined the correct values, you can use a MessagingFactory to establish an AMQP connection to the Azure ServiceBus broker and create and eventhub client from it. You'll have to wrap your message in the AMQP type format, using the EventData object, set the authorization header on it and use the eventhub client to send the data over.

	private async Task Send()
    {
		var ns = "paste your endpoints namespace here";
		var hubname = "paste your hubname here";
		var endpointid = "paste your endpoint id here";
		
		var factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ns, ""), new MessagingFactorySettings
        {
            TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(sas),
            TransportType = TransportType.Amqp
        });

        var client = factory.CreateEventHubClient(String.Format("{0}/publishers/{1}", hubname, endpointid));

        var message = "{ ... }";
		
		var data = new EventData(Encoding.UTF8.GetBytes(message)) { PartitionKey = endpointid };
        data.Properties["Authorization"] = sas;

        await client.SendAsync(data);

    }
	
## Routing messages

By default our gateway routes your messages to all channels and environments that have been specified in the endpoint definition. If you wish to restrict this to a single channel and/or environment, you can do so by adding the correct header values for `x-mh-channel` and `x-mh-environment` to your requests. In the AMQP protocol the headers must be added to each individual message as part of the Properties collection.

	data.Properties["x-mh-channel"] = "paste channel id here";
	data.Properties["x-mh-environment"] = "paste environment id here";
	
## Receiving messages

Before you try receiving messages using Amqp, make sure you have created an endpoint that has the appropriate authorizations! See [Endpoints](/documentation/connectivity/endpoints) for more details.

To receive messages from your channels using the Amqp protocol, you will also need to establish a connection with the Azure ServiceBus broker, but this time to a dedicated queue for your endpoint. Again, you'll need to distill this information from the outbound amqp uri. If it would look like `amqps://messagehandler-gw-eu-west-1.servicebus.windows.net/0eb1beb8-572f-48fd-b9d7-3e6063f0bfd2`, the `messagehandler-gw-eu-west-1` is called the namespace and `0eb1beb8-572f-48fd-b9d7-3e6063f0bfd2` is the name of your queue (which should be the same as your endpoint id.

Once you have determined the correct values, you can again use a MessagingFactory to establish an AMQP connection to the Azure ServiceBus broker and create an queue client from it. Then use that queue client to receive a batch of BrokeredMessage objects from the queue. You need to keep track of the locktokens on the brokered messages. It's up to you to complete those messages that can successfully be processed, and those that can not. Once you have the lock token, you can extract your messages as a byte[] from the brokered messages and start processing. When processing is successful, you will need to signal this back to the Azure ServiceBus broker by calling complete with the lock tokens of those messages that have been processed. 

	private async Task Receive()
    {
		var ns = "paste your endpoints namespace here";
		var queuename = "paste your queue name here";
		var endpointid = "paste your endpoint id here";
		
		var factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ns, ""), new MessagingFactorySettings
        {
            TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(sas),
            TransportType = TransportType.Amqp
        });
		
		var client = factory.CreateQueueClient(endpointid);
		
		var batch = await client.ReceiveBatchAsync(1000, TimeSpan.FromSeconds(1));
		var tokens = batch.Select(x => x.LockToken).ToArray();
		var messages = batch.Select(x => Encoding.UTF8.GetString(x.GetBody<byte[]>())).ToArray();
		
		// processes your messages
		
		if(tokens.Count()  > 0) await client.CompleteBatchAsync(tokens);
    }
	
## Remarks

### Security Requirements

Note that AMQP has very strict security requirements, including SASL or TLS encryption. Security is good, but also very compute intensive. This makes the AMQP protocol less suitable for certain Internet Of Things scenario's, where sensors often don't have enough horsepower to perform these computations. In that case we advise to work with a more potent local field gateway, that acts as a secure intermediary between your sensor and the Azure ServiceBus broker. Alternatively you could also make use of our other protocols that do not necessarily have these requirements.