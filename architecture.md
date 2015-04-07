# Connecting the dots

The internet of things, business process automation, service orientation, microservices, mobile, cloud computing, and many other buzz words from both the past and present, are all variations on the same theme: Distributed software architectures.

From a very high level, one can look at distributed software architectures as 'dots' of independent software components, that communicate with each other using various messaging patterns, very similar to the way humans communicate amongst themselves. This is obviously an extreme simplification, that hides all the complexity involved in distributed computing, but it's a great starting point.

![MesssageHandler](/documentation/images/architecture.png)

<!--
Ideally, these dots are on the one hand as independent of each other as possible: They can be built using different technologies, leverage different internal designs, hosted in different places, be managed individually, etc. But on the other hand they also share enough context so that it stays possible for these components to talk to each other, this includes explicit agreements on protocols, data formats, as well as implicit assumptions like delivery guarantees, data consistency levels, understanding of time, and more. Finding the right balance between these concerns makes distributed software development very challenging
-->

## Internet of things

![MesssageHandler](/documentation/images/architecture-iot.png)

![MesssageHandler](/documentation/images/architecture-business.png)