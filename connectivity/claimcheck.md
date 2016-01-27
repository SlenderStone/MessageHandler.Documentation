# Claimcheck Pattern

In order to request a claimcheck by code you can send an HTTP POST request to

`https://gateway.messagehandler.net/claimchecks`

with following headers

<!-- start of code block -->
 
	Accept: "application/json"
	Content-Type: "application/json
	Authorization: {devicesharedaccesssignature}
    
<!-- end of code block -->

The body content must be empty

The response of the claimcheck request will provide you shared access signature that can be used to upload file content to a file in azure blob storage. The response body format looks like this:

<!-- start of code block -->
 
	{
		"key":"value",
		"expires_at":"datetime-value"
	}
    
<!-- end of code block -->

## Notes

* **Device Shared Access Signature:** You can find the device's shared access signature by navigation to your list of endpoints, and open the endpoint to which this the device belongs. Then click 'manage devices' and select the device in question. You can find the shared access signature in the Device Secret field. Alternatively, you can also issue a device registration request to the API in order to obtain one.