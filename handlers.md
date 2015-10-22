# Handlers

Handlers are the processing units that make up your business logic. Handlers look at each message that flows through a channel, determine if they need to respond to it and then take the appropriate action if needed. 

Roughly speaking, a handler consists of 2 pieces of code:

* A standing query that continuously evaluates the message stream, flowing through the channel, in search of messages that it needs to respond to.
* An action that triggers for each message that fulfils the criteria imposed by the query.

In order to create handlers, you will need specific development skills. If you're not into software development, check out the gallery for existing handlers

* [Learn how to develop handlers](/documentation/developing-handlers)
* [Check out the gallery for existing handlers](/gallery/handlers)
