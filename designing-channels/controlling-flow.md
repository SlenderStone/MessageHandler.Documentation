# Controlling message flow

When designing channels, one of the most important things to consider is how messages will flow between handlers. Especially when keeping in mind that all handlers can look at all messages in the channel by default, include their own output as well as the output of other handlers, it becomes clear that you require a good level of control over the message flow inside the channel. 

There are multiple ways to control the message flow though.

## Using subjects

The easiest way to control the flow is by using so called subjects. Subjects are created by linking the input of one handler to the output of another, this will limit the message flow inbound for the second handler to just those messages that are outputted by the first.

In the graph editor, you can do this by dragging the square on the right of a handler (output) to the left square on another (input).

![Subjects](/documentation/images/architecture-channel.png)

You can also do this in a non graphical manner
 * Open the detail page of the handler for which you want to set the input
 * Click the `Edit Input Subjects` action
 * Select the output subject of the handler(s) that you want to subscribe to and hit `Save`

## Using filters

Many handlers are equipped with dynamic filtering capabilities, often using a configuration property called `Trigger`. You can specify complex filters, using our templating engine, based on the mesage properties, to determine which messages will be taken into account by the handlers and which ones not. These expressions should evaluate as valid c# syntax, it's no rocket science though. Here is an example of such a filter: `message.AggregationMode == "Average" && (message.Amount < 18 || message.Amount > 27)`

## Using queries

Handlers that don't offer filtering capabilities probably control the inflow of messages them selves, hard coded inside the handler using RX queries. This is the third way to control the flow, but it requires development skills and comes a little closer to rocket science as well. See [Implementing a basic handler](/documentation/developing-handlers/implementing-basic-handler) for more details on how to do this.