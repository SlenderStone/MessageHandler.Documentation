# Connecting Windows 10 IoT Core with AMQP protocol

In this article we'll show you how you can use the AMQP protocol to connect to MessageHandler and send or receive messages to/from your channels.

## Preparation

Before you get started, make sure you assign [an endpoint](http://www.messagehandler.net/documentation/connectivity/endpoints-assign) with the AMQP protocol enabled for your device. This step is required to obtain the necessary authentication information and to authorize you device to send or receive messages from or to your channels.

The information you need to gather from your endpoint configuration is the following:

* EndpointId
* ChannelId (optional)
* Inbound Shared Access Signature
* Outbound Shared Access Signature

Our gateway exposes Azure ServiceBus entities (Eventhub Publisher or Queue) in specific Azure ServiceBus namespaces in order to fulfil the AMQP protocol requirement. Note that the namespace for sending and receiving are different! 

From the Shared Access Signature you can derive the names of these entities and namespaces

* The namespace name can be extracted from the sr variable, it probably looks something like `messagehandler-gw-eu-west-X.servicebus.windows.net`
* The entity name can also be extracted from the sr variable, for sending it looks similar to `mh-gw-eu-in-X\Publishers\EndpointId`, for receive it's equal to `EndpointId`

## Using AMQP Net Lite

In order to do all the heavy lifting for the AMQP protocol, we can make use of the AMQPNetLite library. You can find it on nuget with a target for Universal Windows Applications, which is what windows 10 IoT apps are.

Execute the following command to install it.

	Install-Package AMQPNetLite -Version 1.1.0

### Authentication

Before we can send or receive messages, we need to authenticate on the AMQP resource exposed by our gateway. Authenticating is done by sending a message to a special queue called `$cbs` and a reply queue that is created on the fly for your device. 

The need to send an authentication request to the `$cbs` queue. This message needs to have multiple specific properties and application properties set in order to do the validation. Once the validation is done, we get a reply message on the dynamically generated queue, for us to read and interpret.

The code looks like this

	private bool Authenticate(Connection connection, string shareAccessSignature, string entity, string @namespace)
	{
		var authenticationResult = true;
		var session = new Session(connection);

		var replyAddress = "cbs-client-reply-to-" + endpointId;
		var sender = new SenderLink(session, "cbs-sender" + endpointId, "$cbs");
		var receiver = new ReceiverLink(session, replyAddress, "$cbs");

		var request = new Message(shareAccessSignature)
		{
			Properties = new Properties
			{
				MessageId = Guid.NewGuid().ToString(),
				ReplyTo = replyAddress
			},
			ApplicationProperties = new ApplicationProperties
			{
				["operation"] = "put-token",
				["type"] = "servicebus.windows.net:sastoken",
				["name"] = Fx.Format("amqp://{0}/{1}", @namespace, entity)
			}
		};
		sender.Send(request);

		var response = receiver.Receive();
		if (response == null || response.Properties == null || response.ApplicationProperties == null)
		{
			authenticationResult = false;
		}
		else
		{
			var statusCode = (int)response.ApplicationProperties["status-code"];
			if (statusCode != 202 && statusCode != 200)
			{
				authenticationResult = false;
			}
		}

		sender.Close();
		receiver.Close();
		session.Close();

		return authenticationResult;
	}



### Sending

### Receiving