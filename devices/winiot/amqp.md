# Connecting Windows 10 IoT Core using the AMQP protocol

In this article you'll learn how you can use the AMQP protocol to connect to MessageHandler and send or receive messages to/from your channels.

## Preparation

Before you get started, make sure you assign [an endpoint](http://www.messagehandler.net/documentation/connectivity/endpoints-assign) with the AMQP protocol enabled for your device. This step is required to obtain the necessary authentication information and to authorize you device to send or receive messages from or to your channels.

The information you need to gather from your endpoint configuration is the following:

* EndpointId
* ChannelId (optional)
* Inbound Shared Access Signature
* Outbound Shared Access Signature

Our gateway exposes Azure ServiceBus entities (Eventhub Publisher or Queue) in specific Azure ServiceBus namespaces in order to fulfil the AMQP protocol requirement. Note that the namespace for sending and receiving are different! 

From the Shared Access Signature you can derive the names of these entities and namespaces:

* The namespace name can be extracted from the `sr` variable, it probably looks something like 

	`messagehandler-gw-eu-west-X.servicebus.windows.net`

* The entity name can also be extracted from the `sr` variable, for sending it looks similar to 

	`mh-gw-eu-in-X\Publishers\EndpointId`
	
* The entity for receive it's equal to

	`EndpointId`

## Using AMQP Net Lite

In order to do all the heavy lifting for the AMQP protocol, you can make use of the AMQPNetLite library. You can find it on nuget with a target for Universal Windows Applications, which is what windows 10 IoT apps are.

Execute the following command to install it.

	Install-Package AMQPNetLite -Version 1.1.0

### Authentication

Before you can send or receive messages, you need to authenticate on the AMQP resource exposed by our gateway. 

Authenticating is done by sending an `put-token` message to a special queue called `$cbs` in the same namespace as the entity that you want to use. This operation creates a dynamic reply queue on the fly where your device can read it's authentication result from. 

The message that you must send needs to have the shared access signature passed in in the body part, as well as multiple specific properties and application properties set in order to do the validation. These properties include metadata that describes how the body should be interpreted (type sastoken) as well as the entity you would like to authenticate to.

Once the validation has occurred, you will be able to receive the authentication result on the dynamically created reply queue. You can check the `status-code` application property to know whether or not the authentication succeeded. However, behind the scenes some magic is going on as well. The authentication result will contain a claimset, which is automatically tracked by the connection and added to future messages, sent through that connection, automatically as well.

The code required to authenticate looks like this:

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

In order to send a message you need to authenticate on the inbound entity first. To do that you need to establish a connection with the inbound namespace using AMQPS, specifying that the connection should get it's SaslProfile externally (this enables the code that will automagically pass the claimset around).

	public void Send()
	{
		var address = new Address(inboundNamespace, 5671); //5671 = amqps, 5672 = amqp
		var connection = new Connection(address, Amqp.Sasl.SaslProfile.External, null, null);

		if (Authenticate(connection, inboundSignature, inboundEntity, inboundNamespace))
		{
			SendMessage(connection, inboundEntity);
		}
		connection.Close();
	}

Once authenticated you can send messages to the inbound entity. The content should be embedded in the body section. The example code below sends a byte array, but the content type can be anything (depending on what your handlers can de-serialize).

The entity that we use to allow devices to ingest messages into the system are so called EventHubs. These entities can process up to a million events per second, but they do so in a partitioned way. If you want your messages to arrive in the same order as sent, then you must set the `partition-key` to the same value for all messages coming from this device. You can use the `EndpointId` or some other device identifier for this.

In order to re-authenticate your message when the MessageHandler gateway reads it from the eventhub, you need to pass in the inbound shared access signature again through the `Authorization` application header.

Optionally you can also pass in additional headers that control how the routing logic operates. If you don't pass in this metadata, the gateway will forward your message to all authorized channels. But you can limit this if needed by passing in the `x-mh-channel` and/or `x-mh-environment` headers containing the identifiers of the respective channels & environments that you want to sent to (if authorized).

The code to send a message looks like this:

	private void SendMessage(Connection connection, string node)
	{
		Session session = new Session(connection);
		SenderLink sender = new SenderLink(session, endpointId, node);
		Message message = new Message()
		{
			BodySection = new Data()
			{
				Binary = Encoding.UTF8.GetBytes("content")
			}
		};

		message.Properties = new Properties() { };
		message.MessageAnnotations = new MessageAnnotations();
		message.MessageAnnotations[new Symbol("x-opt-partition-key")] = endpointId;

		message.ApplicationProperties = new ApplicationProperties();
		message.ApplicationProperties["Authorization"] = inboundSignature;
		message.ApplicationProperties["x-mh-channel"] = channelId;	

		sender.Send(message);

		sender.Close();
		session.Close();
	}

### Receiving

In order to receive a message you need to authenticate on the outbound entity first. To do that you need to establish a connection with the outbound namespace using AMQPS, again specifying that the connection should get it's SaslProfile externally.

	public void Receive()
	{
		var address = new Address(outboundNamespace, 5671); //5671 = amqps, 5672 = amqp
		var connection = new Connection(address, Amqp.Sasl.SaslProfile.External, null, null);

		if (Authenticate(connection, outboundSignature, outboundEntity, outboundNamespace))
		{
			ReceiveMessage(connection, outboundEntity);
		}
		connection.Close();
	}
	
Once authenticated you can open a receive link to start receiving from the queue that we provisioned for your endpoint and consume the message content. Note that receiving a message means that it becomes invisible on the queue, if you fail to process it in a certain time frame (30 seconds) then the message will reappear on the queue.

If you can correctly process the message you must signal that you `Accept` the message, this will effectively remove the message from the queue. If your code intentionally wants to fail the processing of the message so that it reappears on the queue you can call `Reject`.