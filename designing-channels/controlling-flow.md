# Controlling message flow

When designing channels, on of the most important things is to consider how messages will flow between handlers. Especially when considering that all handlers can look at all messages in the channel by default, include their own output as well as the output of other handlers, it becomes clear that you require a good level of control over the message flow inside the channel. 

There are multiple ways to control the message flow though.

## Using subjects

The easiest way to control the flow is by using so called subjects. Subjects are created by linking the input of one handler to the output of another, this will limit the message flow inbound for the second handler to just those messages that are outputted by the first.

![Subjects](/documentation/images/architecture-channel.png)

## Using filters