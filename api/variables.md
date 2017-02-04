# API Reference : Variables

In order to perform queries on variables, send an authorized GET request on the following base uri:

`https://api.messagehandler.net/variables`

with following headers

<!-- start of code block -->
 
	Accept: "application/json"
	Authorization: "Bearer access_token"
    
<!-- end of code block -->

## List all your variables

`https://api.messagehandler.net/variables`

### Response

<!-- start of code block -->

	Status: 200 OK

	[
	  {
		"account": "messagehandler",
		"scopeIdentifier": "1eea763a-4a16-421e-9e16-0b24d9f50b78",
		"scopeType": "Channel",
		"name": "AverageSoilTemperatureTable",
		"value": "AverageSoilTemperature",
		"fullName": "1eea763a-4a16-421e-9e16-0b24d9f50b78.AverageSoilTemperatureTable"
	  },
	  {
		"account": "messagehandler",
		"scopeIdentifier": "1eea763a-4a16-421e-9e16-0b24d9f50b78",
		"scopeType": "Channel",
		"name": "DurationInSeconds",
		"value": "30",
		"fullName": "1eea763a-4a16-421e-9e16-0b24d9f50b78.DurationInSeconds"
	  }
	]

<!-- end of code block -->

## List all variables for a given account

`https://api.messagehandler.net/variables/{accountid}`

### Parameters

 * accountid: The identifier of the other account, e.g. "messagehandler".

### Response

<!-- start of code block -->

	Status: 200 OK

	[
	  {
		"account": "messagehandler",
		"scopeIdentifier": "1eea763a-4a16-421e-9e16-0b24d9f50b78",
		"scopeType": "Channel",
		"name": "AverageSoilTemperatureTable",
		"value": "AverageSoilTemperature",
		"fullName": "1eea763a-4a16-421e-9e16-0b24d9f50b78.AverageSoilTemperatureTable"
	  },
	  {
		"account": "messagehandler",
		"scopeIdentifier": "1eea763a-4a16-421e-9e16-0b24d9f50b78",
		"scopeType": "Channel",
		"name": "DurationInSeconds",
		"value": "30",
		"fullName": "1eea763a-4a16-421e-9e16-0b24d9f50b78.DurationInSeconds"
	  }
	]

<!-- end of code block -->
  
### Notes

**Authorization:** Before you can view another account's variables, it must authorize you to do so, because variables may often contain personal information. This authorization will be allocated during deployment to the owner account of the environment that you are deploying to. This allows the target environment to query your variables during the deployment phase.

