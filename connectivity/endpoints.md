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
 
## Overcoming size limits

The protocols supported above are limited to messages of up to 256Kb in size, if you want to send larger messages you can overcome these limits by leveraging the claim check pattern

 * [Claimcheck pattern](/documentation/connectivity/claimcheck) which provides you a valet key to a location where you can upload large message content in parallel blocks. This option is ideal for huge files like videos.
 * [File upload](/documentation/connectivity/files) where you upload large message content to as http body content and our infrastructure will handle the claimcheck pattern behind the scenes. Ideal for normal files (MB's).
 
Note that in order to use the claimcheck pattern you need to accompany it with specific types of handlers that can [process files](/handlers/fileprocessors), that can optionally [stream the file content as messages](/handlers/streams). 