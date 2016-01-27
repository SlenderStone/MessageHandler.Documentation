# Registering A Device 

In order to register a device by code you can send an HTTP POST request to

`https://api.messagehandler.net/devices/register/{endpointid}/{deviceid}`

with following headers

<!-- start of code block -->
 
	Accept: "application/json"
	Content-Type: "application/json
    
<!-- end of code block -->

The body content must be empty

The response of the registration request will provide you an access_token which is valid for a limited amount of time (but can be refreshed). The response body format looks like this:

<!-- start of code block -->
 
	{
		"access_token":"access-token-value",
		"expires_at":"datetime-value"
	}
    
<!-- end of code block -->

## Notes

* **Endpoint id:** You can find the endpoint id by navigation to your list of endpoints, and open the endpoint in question. You can find the endpoint id (guid) in the identifier field.
* **Device id:** Your device can register itself



