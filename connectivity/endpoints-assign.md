# Assigning endpoints

In order to assign new endpoints, you can navigate to your endpoint list and select the action `Assign Endpoints`
	
On the form presented to you, perform the following actions

* Enter the amount of endpoints you want to generate.
* Specify a name, or naming convention for the endpoint(s) that you want to create. When you use the `{0}` placeholder in the name, then the endpoint generator will automatically replace this place holder with a unique value for each generated endpoint.
* Select the protocols that you want to allow your endpoint to use for receiving messages from your channels.
* Select the protocols that you want to allow your endpoint to use to send messages to your channels.
* For each of the environments that you manage, select the channels that this endpoint is allowed to send to/receive from.
* Specify a date that access for the endpoint expires.

When all the information is supplied, hit the `Generate` button, this will start the provisioning process for your endpoints.

Please note that provisioning may take a while, for each endpoint defined the system needs to set up Azure ServiceBus resources, credentials, authorization and routing rules.