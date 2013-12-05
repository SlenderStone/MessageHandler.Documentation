## HTTP API

This article describes how to use the HTTP API to publish messages to a channel.

### Before you connect

Before you connect, make sure you are authorized using the OAuth 2.0, 2 legged authorization flow.

	private static SimpleOAuth2Client _client;


### Publishing messages

To publish a message to your channel using the HTTP endpoint, you perform a, HTTP `POST` operation to the `Publish` resource with the message to be sent as the body content, and pass in the `Channel`, the `Environment` via the URI as described in the following example.


	private static void Send()
    {
       var apiRoot = "http://uritomessagehandlergateway:8080/api/channel/publish/" + Channel + "/" + Environment + "/";

       var req = WebRequest.CreateHttp(apiRoot);

       _client.AppendAccessTokenTo(req);

       req.Method = "post";
       req.ContentType = "application/json";

       var json = Newtonsoft.Json.JsonConvert.SerializeObject(new Temperature { Amount = 37 });
       byte[] bytes = Encoding.UTF8.GetBytes(json);

       req.ContentLength = bytes.Length;

       var s = req.GetRequestStream();
       s.Write(bytes, 0, bytes.Length);

       var response = req.GetResponse();

       using (var stream = response.GetResponseStream())
       {
           using (var reader = new StreamReader(stream))
           {
              var text = reader.ReadToEnd();
              Console.WriteLine(text);
           }
       }
    }