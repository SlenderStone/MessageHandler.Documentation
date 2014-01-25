
To obtain an access token from our api, you need to issue an http request to the `authorize` endpoint and pass in your client_id and client_secret information. Here is a code sample using RestSharp and JSon.Net

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