## Authentication and authorization

This article describes how to authenticate against access control service and then authorize against the gateway authorization endpoint.

### OAuth 2.0, 2 Legged

Authentication and authorization for MessageHandler happens using the OAuth 2.0 flow, more specifically the 2-legged variant. This is a variant of the traditional OAuth 2.0 flow where the user of the application is not involved.

This flow consists of 2 steps

- First you go to the serviceprovider to obtain a request token, typically by providing some kind of secret data in exchange of a token
- Secondly you to to the serviceprovider using a request token to obtain an access token. This access token can than be used to access resources on the service provider until the access token expires

![Oauth2](http://www.websequencediagrams.com/cgi-bin/cdraw?lz=cGFydGljaXBhbnQgVXNlcgoABQxDb25zdW0ABg8iU2VydmljZSBQcm92aWRlciIKCm5vdGUgb3ZlcgApCiAgNi4xIE9idGFpbmluZyBhbiBVbmF1dGhvcml6ZWQgUmVxdWVzdCBUb2tlbgplbmQgbm90ZQoKAGwILT4AVxI6ICI2LjEuMS4AgREJAFoHcyBhAEMOIgphY3RpdmF0ZQCBHBQAgTESLT4AgWQIAF0HMi4gAIFYECBJc3N1ZXMgYSBwcmUtAIEzGCIKZGUAYBwAgQ8KAIJVCQCCJxgzAIIxDkFjY2VzcwCBfzIzAIIkDQCCcQdzAEIQIgoAgS0MAIESCgCBLx0AgighMwCCNBVHcmFuAGIgAIRUEw&s=vs2010)

This flow differs from the traditional username/password flow, by only passing your secret info across the wire once, and afterwards you only exchange tokens.

### Windows Azure Active Directory Access Control Service

As doing security right is tough, we have taken the design decision, to offload step 1 in the flow to an external service, the Windows Azure Active Directory Access Control Service.

Once you have obtained a request code from the ACS, you can use this code to request and access token from the `/Authorize` endpoint on the gateway Uri.

### Windows Azure Acs Oauth2 Client

To consume this flow in an easy way, you can make use of the excellent [Windows Azure Acs Oauth2 Client library](http://www.nuget.org/packages/WindowsAzure.Acs.Oauth2.client) which is an open source library written by one of our founders (before we decided to start this venture).

All you need to do is fill in the secret values and the library will do the authentication and authorization dance for you.

	private void Authorize()
    {
		const string ClientId = "XXXXXXXXXXXX"; 
        const string ClientSecret = "XXXXXXXXXXXX"; 
        const string RedirectUri = "http://" + ClientId;
        const string Scope = "http://api.messagehandler.net/";
        const string AuthenticationServer = "https://messagehandler-acs-eu-west-prod.accesscontrol.windows.net/v2/OAuth2-13/";
        const string AuthorizationServer = "http://uritomessagehandlergateway:8080/authorize";

        _client = new SimpleOAuth2Client(new Uri(AuthorizationServer), 
    							        new Uri(AuthenticationServer), 
                						ClientId, 
               							ClientSecret, 
                						Scope, 
                						new Uri(RedirectUri), 
                						ClientMode.TwoLegged);

       _client.Authorize();
	   
    }

Every time you need to access a resource on the gateway, you need to pass it an accesstoken, these accesstokens will not remain valid over time and you need to request a new one, but the oauth client will take care of the details.

	var accesstoken = _client.GetAccessToken()