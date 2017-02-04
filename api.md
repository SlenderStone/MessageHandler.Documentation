# API Reference

This API reference describes all the resources and operations available throught the MessageHandler API. You can find these at the following base address:

`https://api.messagehandler.net`

## Overview Resources

* [Handler Definitions](/documentation/api/definitions)
* [Environments](/documentation/api/environments)
* [Variables](/documentation/api/variables)
* [Endpoints](/documentation/api/endpoints)

## Getting an access token

Many of the resources and operations on our API require authorization before they can be used. In order to authorize, you need to send a POST request to

`https://api.messagehandler.net/authorize`

With the following header values

<!-- start of code block -->
 
	Accept: "application/json"
	Content-Type: "application/json; charset=utf-8"
    
<!-- end of code block -->

In the body of the request you have to provide following content. You can find your client id and client secret on your account's settings page. Open the dropdown underneath your profile picture and select Settings to reach this page.

<!-- start of code block -->
 
	{
		client_id: "Your client id here",
		client_Secret: "Your client secret here",
		scope: "http://api.messagehandler.net/",
		grant_type: 'client_credentials'
	}
    
<!-- end of code block -->

The response of the authorization request will provide you an access_token which is valid for a limited amount of time (but can be refreshed). The response body format looks like this:

<!-- start of code block -->
 
	{
		"token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
		"access_token":"access-token-value",
		"scope":"http://api.messagehandler.net/",
		"expires_in":3600,
		"refresh_token":"refresh-token-value"
	}
    
<!-- end of code block -->

## Passing in the acces token

Once you have an access token, you need to pass that token into each subsequent request that requires authorization using the authorization header like this:

`Authorization: "Bearer access_token"`

