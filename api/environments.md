# API Reference : Environments

In order to perform queries related to your environments, send an authorized GET request on the following base uri:

`https://api.messagehandler.net/environments`

with following headers

<!-- start of code block -->
 
	Accept: "application/json"
	Authorization: "Bearer access_token"
    
<!-- end of code block -->

## List all environment that you can access

`https://api.messagehandler.net/environments`

### Response

<!-- start of code block -->

	Status: 200 OK

	[
	  {
		"identifier": "4af4b433-4d63-4185-90a3-5012070534ab",
		"name": "MessageHandler"
	  },
	  {
		"identifier": "6b7b6e2c-c229-49b0-a2b5-a367d7eff068",
		"name": "Western Europe"
	  },
	  {
		"identifier": "762dd94c-9f60-4cb5-b460-a3fa902e6fe8",
		"name": "SF Testing"
	  },
	  {
		"identifier": "ffad468b-656f-4f64-b697-27deedfbccbc",
		"name": "MessageHandler.BuildServices"
	  }
	]

<!-- end of code block -->

## Get a single environment

`https://api.messagehandler.net/environments/{environmentid}`

### Parameters

 * environmentid: The identifier of your environment, e.g. "6b7b6e2c-c229-49b0-a2b5-a367d7eff068".

### Response

<!-- start of code block -->

	Status: 200 OK

	{
		"identifier": "6b7b6e2c-c229-49b0-a2b5-a367d7eff068",
		"name": "Western Europe"
	}

<!-- end of code block -->
  
## Related Tasks

* [Get configurations of handlers deployed to an environment](/documentation/api/environments/handlers)
