## SignalR

This article describes how to use the Signalr ChannelHub to publish messages to a channel and subscribe to messages coming from a channel which has a signalr handler deployed

### Before you connect

Before you connect, make sure you are authorized using the OAuth 2.0, 2 legged authorization flow.

	private static SimpleOAuth2Client _client;

### Connecting to the ChannelHub

All communication using the SignalR protocol happens with a well known endpoint on the messagehandler endpoint, called the `ChannelHub`. To communicate with this `ChannelHub` you probably need a client that understands the SignalR protocol. There are various SignalR clients available on nuget.org. For the sake of simplicity, we'll use the .net client, available at `http://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/`.

	private static IHubProxy _channelHubProxy;
    
In order to connect you need to create a proxy to the channel hub, and you need to provide it your access token so that it can pass it along on every request it makes. And finally we need to open the connection (in this case using the `LongPollingTransport`)

	private void Connect()
    {
		 var hubConnection = new HubConnection("http://uritomessagehandlergateway:8080/signalr/");

         hubConnection.Headers.Add(HttpRequestHeader.Authorization.ToString(), _client.GetAccessToken());
     
         _channelHubProxy = hubConnection.CreateHubProxy("ChannelHub");
           
         hubConnection.Start(new LongPollingTransport()).Wait();
    }

### Publishing messages

To publish a message to your channel, you invoke the `Publish` resource and pass in the `Channel`, the `Environment` and the message you want to send. 

	_channelHubProxy.Invoke("Publish", Channel, Environment, new Measurement
    {
        Type = "TemperatureMeasurement",
        Temperature = 37
    }).Wait();

### Subscribing for messages

If you want to listen for messages in the channel (assuming you have a signalr handler deployed in it), you can do so by registering a callback on the `ReceiveMessage` operation and than call the `Subscribe` operation to let the gateway know which channels and in which environment you are interested in.

	 _channelHubProxy.On("ReceiveMessage", alert =>
        {
            dynamic msg = JObject.Parse(alert.Message.Value);
            Console.WriteLine("Received alert: {0}, measured temperature {1}", alert.Trigger, msg.Temperature);
        });
       
     hubConnection.Start(new LongPollingTransport()).Wait();

	_channelHubProxy.Invoke("Subscribe", Channel, Environment).Wait();


 **Notes:** 

You should hook up your callback to the `ReceiveMessage` operation before opening the connection, and invoke `Subscribe` after opening it.

Furthermore: You can only subscribe to your own channels.