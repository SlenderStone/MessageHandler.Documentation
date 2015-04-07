# Connecting the dots

The internet of things, business process automation, service orientation, microservices, mobile, cloud computing, and many other buzz words from both the past and present, are all variations on the same theme: Distributed software architectures.

From a very high level, one can look at distributed software architectures as 'dots' of independent software components, that communicate with each other using various messaging patterns, very similar to the way humans communicate amongst themselves. This is obviously an extreme simplification, that hides all the complexity involved in distributed computing, but it's a great starting point nevertheless.

![MesssageHandler](/documentation/images/architecture.png)

## Core concepts

MessageHandler is not trying to be the one size fits all messaging solution, we are solely focussed on getting those components right that logically belong in the cloud. Typically this involves 3 kinds of components:

* **Handlers**: Perform processing logic on messages
* **Endpoints**: Secured gates to which external dots can push messages
* **Stream**: Pulls messages from endpoints of other systems, and pushes them into our system

![MesssageHandler](/documentation/images/architecture-concepts.png)

Next to 3 different types of hosted software components, we also have a few grouping mechanisms

* **Channels**: Groups handlers and streams, that collectively solve a specific issue, as a unit, at design time.
* **Account**: Groups everything that you and your team owns, at design time.
* **Environments**: Groups handlers and streams by the location they are hosted in, at runtime. Can be private, limited to an account.

MessageHandler has been specifically designed to bridge the technical gap between corporate internet of things implementations and the rest of the business ecosystem. So let's have a look at how our core concepts fit into these 2 scenarios.

## Internet of things

Typical corporate internet of things scenario's involve hundreds, thousands or even more sensors and actuators deployed across geographically dispersed areas. These sensors collect a lot of information about their environment. This information is extremely valuable for both the owning organization as it's partner ecosystem. 

For security reasons these devices are often not directly attached to the rest of the world, but make use of local field gateways to aggregate and secure the information stream before sending it over to the cloud for further processing. So on our end, we typically provision an endpoint per local field gateway instance. 

This approach however leads to data of many sensors, being concentrated on a relatively low amount of connections. But you don't have to worry about that, in contrast to many of our competitors we can easily handle very large volumes of messages per second. *(A little anecdote: even 'at rest' our system processes over 10.000 messages per second on a single node)*

The endpoint directs the stream of messages to one or more channels, potentially hosted in different environments, for processing. Processing is done by handlers that typically perform one of the following tasks

* Reduce the message stream so that only relevant information for the use case at hand needs to be taken into account.
* Store the information in the message stream, for later historical analyses using big data technology.
* Perform aggregations on the message stream as humans can't interpret millions of data points per second.
* Detect anomalies and boundary checks at real time.
* Notify the relevant people or partner organisations in case some of these boundaries are exceeded.

![MesssageHandler](/documentation/images/architecture-iot.png)

## Business processes

![MesssageHandler](/documentation/images/architecture-business.png)