To send a message to your channel using the HTTP protocol, you perform an HTTP `POST` operation to the host & path of the received http endpoint, as illustrated in the following example.

	private void Send()
    {
		var client = new RestClient(endpoint.host);

		request = new RestRequest(endpoint.path, Method.POST);
		request.AddHeader(HttpRequestHeader.Authorization.ToString(), header);

		request.AddBody("Hello world"); // body can be anything
           
		response = client.Execute(request);
    }