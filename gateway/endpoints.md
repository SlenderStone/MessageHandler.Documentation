## Endpoints

Before you connect, make sure you are authorized using the OAuth 2.0, 2 legged authorization flow.

### Creating endpoints

In order to obtain an endpoint with available capacity, for the chosen protocol, you need to perform an HTTP POST operation to the `endpoints` resource, specifying your `channel` and `environment` where you want to send messages to.

	private dynamic GetEndpoint()
    {	
		var client = new RestClient("http://api.messagehandler.net");

        var request = new RestRequest("endpoints", Method.POST);
        request.AddHeader(HttpRequestHeader.Authorization.ToString(), header);

        request.AddParameter("protocol", "http"); //amqp, signalr
        request.AddParameter("channel", "YourChannel");
        request.AddParameter("environment", "YourEnvironment");

        var response = client.Execute(request);

        var endpoint = JsonConvert.DeserializeObject<dynamic>(response.Content);
		return endpoint;
	}