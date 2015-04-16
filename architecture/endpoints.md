# Endpoints, Connecting systems

In this article, we'll discuss the internal architecture of endpoints. But before we do so, as a little reminder, we'll rephrase what the role of an endpoint is first.

An endpoint is a specific type of node in a distributed software architecture, which has the purpose to provide secure connectivity between parts of the system (or several systems) that have different trust models. It acts as boundary, the end, of a subsystem hence it's name.

![MesssageHandler](/documentation/images/architecture-concepts.png)

In order to provide the capability of secure connectivity, an endpoint needs to offer several features:

* Durable transport infrastructure for both ingress and egress (we define ingress and egress from our perspective: so ingress is inbound towards MessageHandler, egress is outbound towards your system)
* Support for multiple protocols into this infrastructure
* Authentication capabilities for both ingress and egress
* Authorization rules that specify how messages may flow beyond the endpoint
* Additional restrictions specific to various use cases (think of origin restrictions for web clients for example)

Collectively, we refer to these features as our Gateway. Each master environment has a dedicated set of machines, an azure cloud service, that hosts an instance of this gateway. Each specific instance of this gateway is responsible for managing all resources required to ensure the operation of all endpoints of the accounts that are assigned to it.

The core set of resources managed for those endpoints are Azure ServiceBus entities, these entities take care of both sets of transport infrastructure, authentication and part of the protocol support. So, let's have a look at these resources first.

## Core resources

At the core of our gateway infrastructure you can find following components

![MesssageHandler](/documentation/images/architecture-endpoint.png)

For ingress:

* Azure EventHubs for high scale ingress: These babies can handle up to a million messages per second, each. Needless to say that most use cases need less capacity per endpoint, so we do share these entities between multiple endpoints by default.
* As an authentication mechanism for ingress we leverage Shared Access Signatures on Publisher Policies, so that each endpoint is individually secured and revocable.
* Our gateway software continuously processes these EventHubs and routes the messages towards the correct channels, based on 'route authorizations' and 'additional restrictions' that you can define in the administrative UI. Note: channel architecture is discussed in a separate article.

For egress:

* For egress we again leverage EventHubs to allow handlers deployed inside our route messages outbound. Only some of the built in handlers have access to these EventHubs, you cannot leverage them from your handlers.
* Our gateway software also processes these EventHubs continuously and routes messages towards your endpoint based on the 'route authorizations' that you have defined.
* For each endpoint that you define, we provision an individual queue for it. This queue is secured by a specific Shared Access Signature (different from the ingress one). This setup ensures that your endpoint can come fetch it's messages whenever is most suitable for it. This is very important in IoT and Mobile scenario's where devices don't want to allow inbound tcp connections and are often occasionally connection. Next to that there is also no risk of mixing messages up for different endpoints, every endpoint has it's own personal and secure 'inbox'.

Note: We didn't use ServiceBus topics with subscription filters for egress purposes as you cannot individually assign Shared Access Signatures to subscriptions, you have to assign them at the topic level and this would limit you to only 12 Shared Access Signatures.

## Additional protocols