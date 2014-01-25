
To obtain an access token from our api, you need to issue an http request to the `authorize` endpoint and pass in your client_id and client_secret information. Here is a code sample using RestSharp and JSon.Net

	private string GetAuthorizationHeader()
    {
        var client = new RestClient("http://api.messagehandler.net/");

        var request = new RestRequest("authorize", Method.POST);
        request.AddParameter("client_id", "XXXXXXXXXXXX");
        request.AddParameter("client_secret", "XXXXXXXXXXXX");
        request.AddParameter("scope", "http://api.messagehandler.net/");
        request.AddParameter("grant_type", "client_credentials");

        var response = client.Execute(request);

		var info = JsonConvert.DeserializeObject<dynamic>(response.Content);
        string token = info.access_token;
		string refresh_token = info.refresh_token;

        return "Bearer " + Convert.ToBase64String(Encoding.UTF8.GetBytes(token));
	   
    }