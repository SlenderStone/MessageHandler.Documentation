# SignalR

According to the ASP.NET website: *"ASP.NET SignalR is a library for ASP.NET developers that simplifies the process of adding real-time web functionality to applications. Real-time web functionality is the ability to have server code push content to connected clients instantly as it becomes available, rather than having the server wait for a client to request new data."*

In other words SignalR is a messaging protocol/framework specifically created for web applications!

If you want to learn more about SignalR itself, please refer to the [Introduction to SignalR on the ASP.NET website](http://www.asp.net/signalr/overview/getting-started/introduction-to-signalr)

## Sending messages

Before you try send messages using SignalR, make sure you have created endpoint that has the appropriate authorizations! 

To send a message to your channels using the signalr protocol, all you need to do is establish a hub connection with the correctly specified endpoint values. Then use the `push` method on the server side hub object (which is a part of our gateway).

	$(function () {    
		$.connection.hub.url = 'paste your **inbound** signalr uri here';
		var sas = 'paste your **inbound** shared access signature here';
		
		var hub = $.connection.messages;

		$.connection.hub.qs = { "Authorization": sas };

		$.connection.hub.start(); 
		
		var message = { ... }; // message as JSon structured object 
		
		hub.server.push(message) // sends the message
			.done(function () {
                console.log('Invocation of push succeeded');				
            }).fail(function (error) {
                console.log('Invocation of push failed. Error: ' + error);
            });
	}

## Routing messages

By default our gateway routes your messages to all channels and environments that have been specified in the endpoint definition. If you wish to restrict this to a single channel and/or environment, you can do so by adding the correct header values for `x-mh-channel` and `x-mh-environment` to your requests. In the SignalR client framework the headers are added to the hub.qs object and share by all subsequent requests.

	$.connection.hub.qs = { "Authorization": sas, "x-mh-channel": 'paste channel id here' , "x-mh-environment": 'paste environment id here'   };
	
## Receiving messages

Before you try receiving messages using SignalR, make sure you have created endpoint that has the appropriate authorizations!

To receive messages from your channels using the signalr protocol, all you need to do is specify a `receive` callback on the client side hub object, then establish a hub connection with the correctly specified endpoint values and finally invoke the `subscribe` method on the server side hub object to signal that you're ready to receive messages.

	$(function () {    
		$.connection.hub.url = 'paste your **outbound** signalr uri here';
		var sas = 'paste your **outbound** shared access signature here';
		
		var hub = $.connection.messages;
		
		$.connection.hub.qs = { "Authorization": sas };
		
		hub.client.receive = function(message){
			// this method will be invoked for every message you receive
			// you can access the message properties immediately
			// just like this: message.MyProperty
		};

		$.connection.hub.start().done(function () {
			hub.server.subscribe(); // tells the server that you're ready to receive messages
		});		
	}

## Conditional subscriptions

Web applications are often multi-tenant, multi-user or at least multi-task environments. Depending on who is logged in, or where they are inside the application, you'll want to show users only a subset of all messages that are directed towards your application as a whole. To support this, you can call `subscribeconditional` and specify a message property name and value. This will instruct the server to only invoke your receive method when the property name on the received messages exists and contains the specified value.

	$.connection.hub.start().done(function () {
		hub.server.subscribeconditional("UserId", "myuserid"); // tells the server that you're ready to receive messages filtered by UserId with value myuserid
	});	
