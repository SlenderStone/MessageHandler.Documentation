## Endpoints

Messages are pieces of data that travel from an origin to a destination. In many cases the origin and destination are somewhere in the physical world, not in the cloud where our system is. So these points need to be connected securely to the cloud. We call these connection points endpoints and they are a crucial aspect of our connectivity and security system (called our gateway). 

Endpoints are the virtual representation of just about anything in the real world, a mobile app, a sensor in a factory, a website, a line of business system, you name it. But in essence they are all composed of the following virtual components

 * **Access Control Information**, for granting access to our gateway.
 * **Standard protocols** enabled on our gateway. These protocols allow you to send and receive messages in a way that your IT systems can understand and reduce the need to change existing systems with protocols that you haven't used before.
 * **Routing rules**, which our gateway will use to decide to which channel an endpoint is allowed to send to, or receive from.
 
If you want to learn more about defining endpoints, check out our documentation on the topic: 

 * [Managing Endpoints](/documentation/connectivity/manage-endpoints)

## Using the protocols

Once you have enabled a protocol, you probably need to write some code on your end in order to send messages to our gateway using said protocol. The following documents will show you all the details on how to do that:

 * [Connect with Signalr](/documentation/connectivity/signalr)
 * [Connect with HTTP](/documentation/connectivity/http)
 * [Connect with AMQP](/documentation/connectivity/amqp)