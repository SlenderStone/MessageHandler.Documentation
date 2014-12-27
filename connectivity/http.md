## HTTP

Before you connect, make sure you are authorized using the OAuth 2.0, 2 legged authorization flow and have a valid authorization header.

### Sending messages

To send a message to your channel using the HTTP protocol, you perform an HTTP `POST` operation to the host & path of the received http endpoint, as illustrated in the following example.

	private void Send()
    {
		var client = new RestClient(endpoint.host);

		request = new RestRequest(endpoint.path, Method.POST);
		request.AddHeader(HttpRequestHeader.Authorization.ToString(), header);

		request.AddBody("Hello world"); // body can be anything
           
		response = client.Execute(request);
    }