# Connectivity

Messages are pieces of information flowing from one point to another. In many cases the source and destination of such a flow will be outside our system, in the physical world, somewhere online, or inside your business. We call these points endpoints and they are a crucial item of our connectivity features (called our gateway). 

## Endpoints

Endpoints can be the virtual representation of just about anything, a mobile app, a sensor in a factory, a website, a line of business system, you name it. But in essence they are all composed of the following virtual components

 * Access Control Information, like Shared Access Signatures, for granting access to our gateway.
 * Enabled standard protocols on our gateway. These protocols allow you to send and receive messages in a way that your IT systems can understand and reduce the need to change existing systems with protocols that you haven't used before.
 * Routing rules, which our gateway will use to decide to which channel an endpoint is allowed to send, or receive from.
 
If you want to learn more about defining endpoints, check out our documentation on the topic: 

 * [Endpoints](/documentation/connectivity/endpoints)

## Using the protocols

Once you have enabled a protocol, you probably need to write some code on your end in order to send messages to our gateway using said protocol. The following documents will show you all the details on how to do that:

 * [Signalr](/documentation/connectivity/signalr)
 * [HTTP](/documentation/connectivity/http)
 * [AMQP](/documentation/connectivity/amqp)