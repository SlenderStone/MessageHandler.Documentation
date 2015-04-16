# Endpoints, Connecting systems

In this article, we'll discuss the internal architecture of endpoints. But as a little reminder, we'll rephrase what the role of an endpoint is first.

An endpoint is a specific type of node in a distributed software architecture, which has the purpose to provide secure connectivity between parts of the system (or several systems) that have different trust models. It acts as boundary, the end, of a subsystem hence it's name.

![MesssageHandler](/documentation/images/architecture-concepts.png)

In order to provide this capability, an endpoint needs to offer several features:

* Durable transport infrastructure for both ingress and egress 
* Support for multiple protocols into this infrastructure
* Authentication capabilities for both ingress and egress
* Authorization rules that specify how messages may flow behind the endpoint
* Additional restrictions specific to various use cases (think of origin restrictions for web clients for example)

Collectively, we refer to these features as our Gateway. Each master environment has a dedicated set of machines, an azure cloud service, that hosts an instance of this gateway. Each specific instance of our gateway is responsible for managing all resources required to hosts all endpoints of the accounts that are assigned to it.

The core set of resources managed for those endpoints are Azure ServiceBus entities, these take care of both sets of transport infrastructure, authentication and part of the protocol support. So, let's have a look at these resources first.

## Core resources

![MesssageHandler](/documentation/images/architecture-endpoint.png)

## Additional protocols