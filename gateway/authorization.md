## Oauth 2.0 authentication

Authentication and authorization is implemented using the OAuth 2.0 flow. [OAuth](http://tools.ietf.org/html/rfc6749) is an open protocol that allows secure authorization in a simple and standard method from web, mobile and desktop applications. Basically, the OAuth 2.0 authorization framework enables a third-party application to obtain limited access to an HTTP service.

There are multiple variants of this flow, and depending on certain conditions you need to select one of them. But in general all these variants can be identified as being one of 2 groups, the 3-legged variants and the 2-legged variants.

* The 3-legged variants are used to authenticate application access on behalf of people, by using an external provider and with the user's consent. After consent has been given, the application can access the user's information. This flow is being used by our web application, to allow you to log in using an external account (like github, bitbucket, google, facebook, etc...)

* The 2-legged variants are used to authenticate applications, without user consent. That means that the app typically can only access it's own information and not the user's. 

If you want to learn more about these different variants, IBM has done a great job documenting all these [OAuth 2.0 variants](http://publib.boulder.ibm.com/infocenter/tivihelp/v2r1/index.jsp?topic=%2Fcom.ibm.tivoli.fim.doc_6226%2Fconfig%2Fconcept%2FOAuth20Workflow.html). 
 
We believe that for the majority of applications, MessageHandler will be used as back-end service for the application itself, so without the user knowing about it. Therefore our api defaults to the 2 legged variant with client credentials flow.

## Obtaining an access token

To obtain an access token from our api, you need to issue an http request to the `authorize` endpoint and pass in your client_id and client_secret information  (Ps: You can find these values on the connection details page in your account). It will return you an access_token and a refresh_token (which you will need later).

Here is a code sample using RestSharp and JSon.Net

	private string GetAuthorizationHeader()
    {
		const string ClientId = "XXXXXXXXXXXX"; 
        const string ClientSecret = "XXXXXXXXXXXX"; 
        const string Scope = "http://api.messagehandler.net/";
        const string BaseUri = "http://secretapi.messagehandler.net/";

         var client = new RestClient(BaseUri);

        var request = new RestRequest("authorize", Method.POST);
        request.AddParameter("client_id", ClientId);
        request.AddParameter("client_secret", ClientSecret);
        request.AddParameter("scope", Scope);
        request.AddParameter("grant_type", "client_credentials");

        var response = client.Execute(request);

		var info = JsonConvert.DeserializeObject<dynamic>(response.Content);
        string token = info.access_token;
		string refresh_token = info.refresh_token;

        return "Bearer " + Convert.ToBase64String(Encoding.UTF8.GetBytes(token));
	   
    }

You will have to convert the token value into an authorization header that can be passed along subsequent web requests, by base 64 encoding the token and append it as a `Bearer ` token.

### Passing the header to the api

Every time you request information from or perform an operation on our API you need to pass in the token as an authorization header and bearer token as an Http header.

	private string RequestEndpoint()
    {
		var header = GetAuthorizationHeader();
		
		var client = new RestClient(BaseUri);

        var request = new RestRequest("endpoints", Method.POST);
        request.AddHeader(HttpRequestHeader.Authorization.ToString(), header);

        request.AddParameter("protocol", "http");
        request.AddParameter("channel", Channel);
        request.AddParameter("environment", Environment);

        var response = client.Execute(request);

		var endpoint = JsonConvert.DeserializeObject<dynamic>(response.Content);
        return endpoint.address;
	}

### Refreshing the access token

Access tokens only have a limited lifetime, when the token expires, you will need to obtain a new one. This is done by authorizing again, this time using the refresh token that you got originally.

	private string GetAuthorizationHeader()
    {
        var refreshRequest = new RestRequest("authorize", Method.POST);
        refreshRequest.AddParameter("client_id", Settings.ClientId);
        refreshRequest.AddParameter("client_secret", Settings.ClientSecret);
        refreshRequest.AddParameter("scope", Scope);
        refreshRequest.AddParameter("grant_type", "refresh_token");
        refreshRequest.AddParameter("refresh_token", refresh_token);

        response = client.Execute(refreshRequest);

        string access_token = JsonConvert.DeserializeObject<dynamic>(response.Content).access_token;
        return "Bearer " + Convert.ToBase64String(Encoding.UTF8.GetBytes(access_token));
    }

