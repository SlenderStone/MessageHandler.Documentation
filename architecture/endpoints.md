# Endpoints, Connecting systems

In this article, we'll discuss the internal architecture of endpoints. But before we do so, as a little reminder, we'll rephrase what the role of an endpoint is first.

An endpoint is a specific type of node in a distributed software architecture, which has the purpose to provide secure connectivity between parts of the system (or several systems) that have different trust models. It acts as boundary, the end, of a trusted subsystem hence it's name.

![MesssageHandler](/documentation/images/architecture-concepts.png)

In order to provide the capability of secure connectivity, an endpoint needs to offer several features:

* Durable transport infrastructure for both ingress and egress (we define ingress and egress from our perspective: so ingress is inbound towards MessageHandler, egress is outbound towards your system)
* Support for multiple protocols into this infrastructure
* Authentication capabilities for both ingress and egress separately
* Authorization rules that specify how messages may flow beyond the endpoint
* Additional restrictions specific to various use cases (think of origin restrictions for web clients for example)

Collectively, we refer to these features as our Gateway. Each master environment has a dedicated set of machines, an azure cloud service, that hosts an instance of this gateway. Each specific instance of this gateway is responsible for managing all resources required to ensure the operation of all endpoints of the accounts that are assigned to it.

The core set of resources managed for those endpoints are Azure ServiceBus entities, these entities take care of both sets of transport infrastructure, authentication and part of the protocol support. So, let's have a look at these resources first.

## Core resources

At the core of our gateway infrastructure you can find following components:

![Endpoint Architecture](/documentation/images/architecture-endpoint.png)

For ingress:

* Azure EventHubs for high scale ingress: These babies can handle up to a million messages per second, each. Needless to say that most use cases need less capacity per endpoint, so we do share these entities between multiple endpoints by default.
* As an authentication mechanism for ingress we leverage Shared Access Signatures on Publisher Policies, so that each endpoint is individually secured and revocable.
* Our gateway software continuously processes these EventHubs and routes the messages towards the correct channels, based on 'route authorizations' and 'additional restrictions' that you can define in the administrative UI. Note: channel architecture is discussed in a separate article.

For egress:

* For egress we again leverage EventHubs to allow handlers deployed inside our channels, to route messages outbound. Only some of the built-in handlers, like the endpoint router, have access to these EventHubs. You cannot leverage them from within your handlers.
* Our gateway software also processes these EventHubs continuously and routes messages towards your endpoint based on the 'route authorizations' that you have defined.
* For each endpoint that you define, we provision an individual queue. This queue is secured by a specific Shared Access Signature (different from the ingress one). This setup ensures that your endpoint can come fetch it's messages whenever is most convenient for it. This is very important in IoT and Mobile scenario's where devices don't want to allow inbound tcp connections and are often occasionally connected as well. Next to that there is also no risk of mixing messages up for different endpoints, every endpoint has it's own personal and secure 'inbox'.

Note: We didn't use ServiceBus topics with subscription filters for egress purposes as you cannot individually assign Shared Access Signatures to subscriptions, you have to assign them at the topic level and this would limit you to only 12 Shared Access Signatures.

## Additional protocols

Out of the box, all Azure ServiceBus entities allow you communicate using the AMQPS 1.0 or HTTPS protocol. These are convenient, but in many scenario's other protocols are required. So we added a pluggable adapter layer to our gateway to accommodate for these, in addition to AMQP we also support the following protocols:

* Signalr: Allows for real time communication with website frontends written in javascript.
* Http: Many IoT devices do not have enough system resources to deal with SSL encryption. For production use, we suggest you leverage a local field gateway intermediary that has the capabilities to perform SSL encryption on behalf of these devices, but for development and prototyping this approach is way to complex. Hence we allow you to connect to our gateway in a non-encrypted way for these scenario's.
* Other: Our gateway is pluggable, so we can add additional protocols to it if required. A common ask is MQTT for example, but almost anything is possible.

The architecture of this pluggable layer looks like this.

For ingress, the adapters are stacked next to the amqp one (which is EventProcessorHost based). All of them have dedicated addresses specific to the protocol, and are implemented using commonly available Microsoft frameworks. Each of these implementations uses a shared authentication layer to validate the ingress shared access signature and once validated, the messages are forwarded to a shared routing layer that routes the messages to your channels based on your authorization rules.

![Endpoint Protocols Out](/documentation/images/architecture-endpoint-protocols-in.png)

For egress the design is a bit different. In this case the adapters are layered on top of the queues as we need to store the messages temporarily until the endpoint can connect, authenticate and come fetch them. Some protocols however have pub-sub semantics while maintaining an open connection, like Signalr does, for these protocols we maintain a server side queue processor that runs while a connection is open and continuously monitors the queue and forwards messages towards the connected client.

![Endpoint Protocols In](/documentation/images/architecture-endpoint-protocols-out.png)

## Direct egress

I hope this article gives you some more insight into how our endpoint infrastructure works under the hood. It is quite a convenient way to get messages flowing into and out of your channels in a structured way. But you have to remember that it is also possible, for egress, to send messages directly to other systems from within your handlers. Many of our existing handlers do this to f.e. send push notifications to mobile devices through azure notification hubs, or emails via an smtp server, etc.

