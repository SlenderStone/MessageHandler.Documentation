# HTTP

According to wikipedia: *"The Hypertext Transfer Protocol (HTTP) is an application protocol for distributed, collaborative, hypermedia information systems. HTTP is the foundation of data communication for the World Wide Web. HTTP functions as a request-response protocol in the client-server computing model. The client submits an HTTP request message to the server. The server, which provides resources such as HTML files and other content, or performs other functions on behalf of the client, returns a response message to the client. The response contains completion status information about the request and may also contain requested content in its message body."*

Basically it's the messaging protocol that the internet has been built on. Support for this protocol is so ubiquitous, that almost any system can use it. If you want to learn more about Http itself, please refer to [the wiikipedia document on Http](en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)

## <a name="sending"></a>Sending messages

Before you try send messages using Http, make sure you have created an endpoint that has the appropriate authorizations! See [Endpoints](/documentation/connectivity/endpoints) for more details.

To send messages to your channels using the http protocol, all you need to do is to perform a POST request on the uri provided by the endpoint, while including the correct Shared Access Signature in the authorization header and the message content in the body.

	private async Task Send()
    {
		var uri = "paste your inbound http uri here";
		var sas = "paste your inbound shared access signature here";
		
		var client = new HttpClient();
        client.DefaultRequestHeaders.Add(HttpRequestHeader.Authorization.ToString(), sas);
		
		var message = "{ ... }"; // any format will do, doesn't have to be JSon or a string for that matter

        var response = await client.PostAsync(uri, new StringContent(message));

        var result = await response.Content.ReadAsStringAsync();
		
		Console.WriteLine(result);
    }
	
## Routing messages

By default our gateway routes your messages to all channels and environments that have been specified in the endpoint definition. If you wish to restrict this to a single channel and/or environment, you can do so by adding the correct header values for `x-mh-channel` and `x-mh-environment` to your requests. In the Http protocol the headers must be added to each individual request, or if your client supports it a default header collection which will be added to each subsequent request.

	client.DefaultRequestHeaders.Add("x-mh-channel", "paste channel id here");
	client.DefaultRequestHeaders.Add("x-mh-environment", "paste environment id here");
	
## <a name="receiving"></a>Receiving messages

Before you try receiving messages using Http, make sure you have created an endpoint that has the appropriate authorizations! See [Endpoints](/documentation/connectivity/endpoints) for more details.

To receive messages from your channels using the http protocol, all you need to do is to perform a GET request on the uri provided by the endpoint, while including the correct Shared Access Signature in the authorization header. The response will contain zero or more messages.

	private async Task Receive()
    {
		var uri = "paste your outbound http uri here";
		var sas = "paste your outbound shared access signature here";
		
		var client = new HttpClient();
        client.DefaultRequestHeaders.Add(HttpRequestHeader.Authorization.ToString(), sas);
		
        var response = await client.GetAsync(uri);

		var result = await response.Content.ReadAsStringAsync(); 
    }