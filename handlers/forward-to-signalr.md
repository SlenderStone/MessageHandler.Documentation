## Forward to signalr

Forwards a message to a signalr hub, called ChannelHub, hosted in the MessageHandler gateway. You can subscribe to this hub to receive messages that are traveling through the channel and satisfy certain condition.

[Learn more about using the ChannelHub](http://#todo)

### Handler properties

**Trigger:** A condition that will trigger the handler to forward a message to the ChannelHub, the trigger has to be created using Javascript syntax and must evaluate to a bool. You can access the message under consideration using the `msg` parameter. For example:

	msg.Temperature > 50

### Message requirements

**Serializer:** JSON

**Required Fields:** None
