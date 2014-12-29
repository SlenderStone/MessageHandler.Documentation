# Use cases

At it's core, messaging is a very simple concept, pieces of digital information that are travelling from one point to another. And because it's such a basic concept, it can be applied in many different ways. This document tries to list some of the use cases where messaging is often applied and shows you how to put it all together.

## Remote Control

The first use case, in which messaging plays a key role are Remote Control systems. Typically these systems consist of a central control application (ranging in size of smartphone app to a full blown control room) which sends commands to one or more devices out there.

## Monitoring & alerting

Often, the remote control systems are accompanied by Monitoring and Alerting systems. These systems monitor things out there for vital discrete events and respond to those events by informing the correct people about what is going on. Typically using alerts or notifications depending on the business severity of these discrete events. It's then up to those people to choose how to respond to the alerts.

## Real-time analytics

Often it is the case that interesting events are not readily available from the outside world, but they need to be distilled from more raw streams of relatively meaningless data. In this use case, the messages to be processed cannot be looked at individually, like in the previous use cases, but should be looked at in a time window relative to each other, typically in search for anomalies in the stream. Real-time analytics algorithms are applied to the stream of data points in order to determine the discrete events which can then be fed into the Monitoring and alerting systems.

## Workflow automation

When people are alerted, they may choose to act upon it. If this reaction is well defined, with a well known process to be followed, then messaging can be used to streamline or even fully automate it. Messaging can be leveraged to either set up the communication flow between different people in an automated way and even involve sending commands to machines in order to automate the repetitive parts of the process.

## Integration

And finally, messaging is also used to integrate different technical systems with each other. Asynchronous messaging architectures have proven to be the best strategy for enterprise and application integration because they allow for a loosely coupled solution that overcomes the limitations of remote communication, such as latency and unreliability.

## Auditing and compliance management

When all systems and people in an organisation, or ecosystem, are communicating with each other using messaging, then auditing and compliance management, which are often a serious organizational challenge, can also be fully automated. Just keep a copy of all messages being passed around and you will get a complete audit of what is going on inside your organisation. Ideal for auditing and compliance management.

## Event Sourcing

If your audit log is complete, which should be the case for compliance reasons anyway, it even becomes possible to restore the state of your organisation as it was at any point in time in the past, by simply replaying all the messages in the audit log up until the point of interest. This is the main idea behind the concept of Event Sourcing, which uses the audit log to rebuild the state of a technical entity, such as application state or the database state. In fact a database is in these types of solutions only viewed as a view on the audit log, and not as the single point of truth... allowing more efficient use of multiple types of data stores in the same application, where each data store is used for a specific aspect of the application.

##  Building systems

If you put all of the above together, you can see that it is perfectly possible to build complete systems purely on the concepts of messaging. In fact, MessageHandler is living proof of this idea, as it is completely built on top of itself.



