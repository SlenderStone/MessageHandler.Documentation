# Connecting the dots

The internet of things, business process automation, service orientation, microservices, mobile, cloud computing, and many other buzz words from both the past and present, are all variations on the same theme: Distributed software architectures.

From a very high level, one can look at distributed software architectures as 'dots' of independent software components, that communicate with each other using various messaging patterns, very similar to the way humans communicate amongst themselves. This is obviously an extreme simplification, that hides all the complexity involved in distributed computing, but it's a great starting point for this conversation nevertheless.

![MesssageHandler](/documentation/images/architecture.png)

## Core concepts

All of these software components perform a very specific role in the overall system, but because they are independent of each other the system can continue to operate even if some of the components are temporarily unavailable. Some typical component types that you will find in a distributed messaging system are:

* Software that initiates a message exchange, like sensors, websites, or mobile interfaces. In our nomenclature we call these **origins**
* Components that perform processing logic (think encryption, filtering, calculations etc...) on the data while it is passing by, we call these components **handlers**.
* Secured gates to which external components can push their data, or receive information from in a secure way, over a distance, we call these [**endpoints**](/documentation/architecture/endpoints).
* Sometimes logic needs to pull information in from endpoints in other systems so that you can process it in your logic, we call these **streams**.
* And ultimately the result of all this processing logic needs to end up somewhere, at a certain **destination**, this destination could be a database, an external system, a human or even a robot.

![MesssageHandler](/documentation/images/architecture-concepts.png)

Even though the software components in such systems are typically independent of each other, in order to fulfill specific business use cases they need to be tied together. You need to be able to control how the data flows between them, which component is interested in the output of which other component, and you need to be able to apply configuration settings to everything so that the components, and the whole, start behaving in a well defined way. We call this collective a **channel**.

![MesssageHandler](/documentation/images/architecture-channel.png)

Once a channel has been composed, it needs to be put into operation. In order to do so, we deploy all of it's parts to a given **environment**. An environment is a bunch of machines hosted somewhere on the planet. All of our environments are powered by Windows Azure, which means you can put your logic on any continent, close to your customers. We will take care of managing those environments and keeping them up to date.

## On the internet of things

MessageHandler provides you all the tools to build, sell, run and operate all kinds of message processing logic. But what makes it special is that it has been specifically designed for the 'internet of things' as well. It is in fact the ideal place to bridge the gap between corporate internet of things implementations and the rest of a business ecosystem. 

What is so special about 'internet of things' scenarios, is the huge amount of information that is being generated. Typical corporate 'internet of things' scenarios involve thousands and more sensors and actuators deployed across geographically dispersed areas. Each sensor collects some data points about it's environment, individually these datapoints are of limited value, but collectively and when interpreted into information, this information is extremely valuable for both the owning organization as it's partner ecosystem. 

The more real time this information is the more valuable it becomes, often resulting in the desire to process millions of messages per second. When you can get more accurate information faster then your competition, you have a better shot at winning the race.

Real time information is hard though. But you don't have to worry about that, we'll handle it. In contrast to many of our competitors (who are usually taking a pure request-reply based approach by stacking API calls) we can easily scale out to very large amounts of messages processed per second. *A little anecdote: even 'at rest' our system processes over 10.000 messages per second, just because we can!*

**Now lets have a look at what such a scenario often translates into in terms of our concepts.**

![MesssageHandler](/documentation/images/architecture-iot.png)

For security reasons sensors are rarely directly attached to the internet, but make use of local field gateways to aggregate and secure the information stream before sending it over to the cloud for further processing. The reason for this is that most of those devices have very limited memory and cpu resources, as they want to run on batteries, and this prevents them from securing or encrypting their data, which requires quite a lot of power. It is the role of the field gateway to make the local system secure. So on our end, we typically provision a secured **endpoint** per local field gateway instance, and not per sensor.

The endpoint directs the stream of messages coming from the field gateway to one or more **channels**, potentially hosted in different **environments**, for processing. Processing is done by **handlers** that typically perform one of the following tasks:

* Reduce the message stream so that only relevant information for the use case at hand needs to be taken into account.
* Store the information in the message stream, for later historical analyses using big data technology.
* Perform aggregations on the message stream as humans can't interpret millions of data points per second.
* Boundary checks and anomaly detection in real time, as that is what we humans are mostly interested in.
* Notify the relevant people or partner organisations in case some of these boundaries are exceeded.
* And in some cases it is also desirable to automate the scenario completely, by sending commands back to the machine so that it can take preventive measures automatically.

The general structure of this scenario is almost always the same, ingest as much information as possible, store it for later usage, but at the same time detect patterns and extract the interesting information & insights in real time, then forward those insights to people or machines that need to know about it.

## Under the hood

Now that you have a solid understanding of the core functional concepts, lets take a look at how this translates into technical components under the hood.

![MesssageHandler](/documentation/images/architecture-technical-overview.png)

The core runtime, an environment, consists of 3 major subsystems:

* **The fabric** is an azure cloudservice which is responsible for hosting, monitoring, managing, scaling and load balancing the handlers hosted on it.
* [**The gateway**](/documentation/architecture/endpoints) is another azure cloudservice, plus a set of azure servicebus namespaces, which takes care of the endpoints assigned to it: among others it provisions protocol specific resources, it deals with authentication and authorization of devices, it takes care of routing of message streams towards the correct transport infrastructure and much more.
* **The transport** wires everything together. It consists of a set of azure servicebus namespaces that have been configured based on your channel definitions.

On top of the core runtime, you can find other subsystems that are all being operated by the core runtime

![MesssageHandler](/documentation/images/architecture-dogfooding.png)

Yes, you have read that right, we are dogfooding our own runtime to the extreme. Everything you can use inside our system has been built using our runtime. Every action you take will become a **message**, routed to a **channel** via an **endpoint** and then processed by a set of **handlers** to result into the desired action, everything in real time.

## Want to learn more?

I hope I've been able to explain both our functional and technical architecture at a very high level. We are working hard to get more detailed information out about the individual aspects described in this article. In the mean time, if you want to learn more about certain aspects, do not hesitate to reach out to us through the discussion panel below. Or just stay in touch through twitter, our newsletter or other medium so that you can stay informed of updates.