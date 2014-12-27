## SignalR

Before you connect, make sure you are authorized using the OAuth 2.0, 2 legged authorization flow and that you have obtained an endpoint for the signalr protocol.

### Sending messages

To send a message to your channel using the signalr protocol, you invoke the provided method on the signalr hub at the given address, and pass in the `Channel`, the `Environment` and the message you want to send. 

	private void Send()
    {
		var hubConnection = new HubConnection(endpoint.address);
            
        hubConnection.EnsureReconnecting();
        hubConnection.Headers.Add(HttpRequestHeader.Authorization.ToString(), header);

        var channelHubProxy = hubConnection.CreateHubProxy(endpoint.hub);

        hubConnection.Start(new LongPollingTransport())
           .Wait(TimeSpan.FromSeconds(30));

        channelHubProxy.Invoke(endpoint.method,
            "YourChannel",
            "YourEnvironment",
            new { Message = "Hello world" } // must be json
            ).Wait(TimeSpan.FromSeconds(30));
	}
