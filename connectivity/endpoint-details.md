# Endpoint details

On the endpoint details page you will get an overview of all the technical details related to your endpoint. Each value is associated with a copy button to provide a convenient way to get the (sometimes long cryptic) value on your computer's clipboard.

## Access Information

* Endpoint: The technical endpoint identifier, needed for client/device side code when sending or receiving messages.
* Inbound Shared Access Signature: Needed for the device or client to authenticate on resources that allow it to send messages.
* Outbound Shared Access Signature: Needed for the device or client to authenticate on resources that allow it to receive messages.

## Enabled Protocols

For each enabled protocol and for each direction, you will find a resource identifier. This resource identifier will allow you to connect to our gateway using the protocol in question, either for receiving (out) or for sending messages (in).

## Authorized Routes

This list contains all the routes that are allowed to be used by this endpoint. Each authorized route is defined by three properties:

* The channel to, or from, which the endpoint is allowed to route messages.
* The environment in which this channel is hosted.
* The direction: In = allowed to send, Out = allowed to receive.

## Restrictions

Further protocol specific restrictions may apply, for example Http or Signalr traffic can be limited to specific domains (CORS). If they do, they are listed here.

## Possible Actions

* Add restriction
* Authorize routes