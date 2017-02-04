# API Reference : Handler Definitions

In order to perform queries related to your handler definitions, send an authorized GET request on the following base uri:

`https://api.messagehandler.net/handlers/definitions`

with following headers

<!-- start of code block -->
 
	Accept: "application/json"
	Authorization: "Bearer access_token"
    
<!-- end of code block -->

## List all your handler definitions

`https://api.messagehandler.net/handlers/definitions`

### Response

<!-- start of code block -->

	Status: 200 OK

	[{
		"identifier": "98b3162d-41a2-41e7-91b5-050c3f15d667",
		"name": "Average In Period",
		"description": "Raises aggregated measurements by means of performing an average during a defined time period",
		"version": "1.0.0.0",
		"runtime": "MessageHandler",
		"runtimeVersion": "2.0.0.0",
		"owner": "messagehandler",
		"package": "98b3162d-41a2-41e7-91b5-050c3f15d667.zip",
		"status": "Accepted",
		"configuration": [
		  {
			"version": "1.0.0.0",
			"name": "MeasurementType",
			"fieldType": "textbox",
			"isOutputSubject": false
		  },
		  {
			"version": "1.0.0.0",
			"name": "DurationInSeconds",
			"fieldType": "textbox",
			"isOutputSubject": false
		  }
		]
	},
	{
		"identifier": "98b3162d-41a2-41e7-91b5-050c3f15d667",
		"name": "Average In Period",
		"description": "Raises aggregated measurements by means of performing an average during a defined time period",
		"version": "1.0.0.1",
		"runtime": "MessageHandler",
		"runtimeVersion": "2.0.0.0",
		"owner": "messagehandler",
		"package": "98b3162d-41a2-41e7-91b5-050c3f15d667.zip",
		"status": "Accepted",
		"configuration": [
		  {
			"version": "1.0.0.1",
			"name": "MeasurementType",
			"fieldType": "textbox",
			"isOutputSubject": false
		  },
		  {
			"version": "1.0.0.1",
			"name": "DurationInSeconds",
			"fieldType": "textbox",
			"isOutputSubject": false
		  }
		]
	}]

<!-- end of code block -->

## Get a single handler definition

`https://api.messagehandler.net/handlers/definitions/{definitionid}/{version}`

### Parameters

 * definitionid: The identifier of your handler definition, e.g. "98b3162d-41a2-41e7-91b5-050c3f15d667".
 * version: A string representing a version number, e.g. "1.0.0.0"

### Response

<!-- start of code block -->

	Status: 200 OK

	{
		"identifier": "98b3162d-41a2-41e7-91b5-050c3f15d667",
		"name": "Average In Period",
		"description": "Raises aggregated measurements by means of performing an average during a defined time period",
		"version": "1.0.0.0",
		"runtime": "MessageHandler",
		"runtimeVersion": "2.0.0.0",
		"owner": "messagehandler",
		"package": "98b3162d-41a2-41e7-91b5-050c3f15d667.zip",
		"status": "Accepted",
		"configuration": [
		  {
			"version": "1.0.0.0",
			"name": "MeasurementType",
			"fieldType": "textbox",
			"isOutputSubject": false
		  },
		  {
			"version": "1.0.0.0",
			"name": "DurationInSeconds",
			"fieldType": "textbox",
			"isOutputSubject": false
		  }
		]
	}

<!-- end of code block -->
  
## Related Tasks

* [Uploading a handler package](/documentation/api/definitions/package)