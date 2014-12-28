# Connectivity

Messages are pieces of information flowing from one point to another. In many cases the source and destination of such a flow will be outside our system, in the physical world, somewhere online, or inside your business. We call these points endpoints and they are a crucial item in our connectivity features. 

## Endpoints

Endpoints can be the virtual representation of just about anything, a mobile app, a sensor in a factory, a website, line of business system. But in essence they are composed of the following virtual components

 * Access Control Information, like Shared Access Signatures, for granting access to our gateway.
 * Enable standard protocols on our gateway. These protocols allow you to send and receive messages in a way that your IT systems can understand and reduce the need to change existing systems.
 * Define routing rules, which our gateway will use to decide to which channel an endpoint is allowed to send, or receive from.
 
If you want to learn more about defining endpoints, check out our [Endpoints](/documentation/connectivity/endpoints) documentation

## Using the protocols

Once you have enabled a protocol, on an endpoint, you need to write some code on your end in order to send messages to our gateway using said protocol. The following documents will show you all the details on how to do that

 * [Signalr](/documentation/connectivity/signalr)
 * [HTTP](/documentation/connectivity/http)
 * [AMQP](/documentation/connectivity/amqp)