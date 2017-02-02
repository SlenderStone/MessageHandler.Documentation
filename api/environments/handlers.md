# API Reference : Environments

In order to perform queries for the handler configurations that are deployed in a given environment, send an authorized GET request on the following base uri:

`https://api.messagehandler.net/environments/{environmentid}/handlers`

with following headers

<!-- start of code block -->
 
	Accept: "application/json"
	Authorization: "Bearer Base64Encode(access_token)"
    
<!-- end of code block -->

### Parameters

 * environmentid: The identifier of your environment, e.g. "6b7b6e2c-c229-49b0-a2b5-a367d7eff068".

## List all handler configurations deployed to the environment

`https://api.messagehandler.net/environments/{environmentid}/handlers`

### Response

<!-- start of code block -->

	Status: 200 OK

	[
	  {
		"account": "messagehandler",
		"accountId": "messagehandler",
		"averages": "1b206e08-91d0-47cc-80fe-bf2f416beada_Averages",
		"channel": "1eea763a-4a16-421e-9e16-0b24d9f50b78",
		"channelId": "1eea763a-4a16-421e-9e16-0b24d9f50b78",
		"description": "Raises aggregated measurements by means of performing an average during a defined time period",
		"durationInSeconds": "60",
		"handlerConfigurationDescriptionIdentifier": "98b3162d-41a2-41e7-91b5-050c3f15d667",
		"handlerConfigurationId": "1b206e08-91d0-47cc-80fe-bf2f416beada",
		"handlerConfigurationIdentifier": "1b206e08-91d0-47cc-80fe-bf2f416beada",
		"handlerDescriptionId": "98b3162d-41a2-41e7-91b5-050c3f15d667",
		"handlerDescriptionIdentifier": "98b3162d-41a2-41e7-91b5-050c3f15d667",
		"handlerIdentifier": "1b206e08-91d0-47cc-80fe-bf2f416beada",
		"hostVersion": "2.2.0.0",
		"identifier": "1b206e08-91d0-47cc-80fe-bf2f416beada",
		"inputSubjects": null,
		"name": "Average In Period",
		"outputSubjectRoutes": {},
		"projectId": "51deb092-0ffa-bc4d-395b-5ff42e6f03ed",
		"purpose": "Production",
		"state": "Operational",
		"version": "1.0.0.40"
	  },
	  {
		"account": "messagehandler",
		"accountId": "messagehandler",
		"body": "The average temperature is outside the ideal boundaries.",
		"channel": "1eea763a-4a16-421e-9e16-0b24d9f50b78",
		"channelId": "1eea763a-4a16-421e-9e16-0b24d9f50b78",
		"description": "Sends emails through SMTP",
		"from": "yves@messagehandler.net",
		"handlerConfigurationDescriptionIdentifier": "99297ee4-c6e5-4068-bb55-40924343e5d4",
		"handlerConfigurationId": "1640a34a-2929-4a2f-98c2-7d6851839ef5",
		"handlerConfigurationIdentifier": "1640a34a-2929-4a2f-98c2-7d6851839ef5",
		"handlerDescriptionId": "99297ee4-c6e5-4068-bb55-40924343e5d4",
		"handlerDescriptionIdentifier": "99297ee4-c6e5-4068-bb55-40924343e5d4",
		"hostVersion": "2.2.0.0",
		"identifier": "1640a34a-2929-4a2f-98c2-7d6851839ef5",
		"inputSubjects": null,
		"name": "Send Email",
		"outputSubjectRoutes": {},
		"password": "xxxxxxxxxxxxxx",
		"port": "587",
		"projectId": "51deb092-0ffa-bc4d-395b-5ff42e6f03ed",
		"purpose": "Production",
		"server": "smtp.sendgrid.net",
		"state": "Operational",
		"subject": "Alert!!!!",
		"to": "yves@goeleven.com",
		"trigger": "message.AggregationMode == \"Average\" && (message.Amount < 18 || message.Amount > 27)",
		"username": "MessageHandler",
		"version": "1.0.0.1"
	  }
	]

<!-- end of code block -->

## Get a single environment

`https://api.messagehandler.net/environments/{environmentid}/handlers/{handlerconfigurationid}`

### Parameters

 * handlerconfigurationid: The identifier of your handler configuration, e.g. "1b206e08-91d0-47cc-80fe-bf2f416beada".

### Response

<!-- start of code block -->

	Status: 200 OK

	{
		"account": "messagehandler",
		"accountId": "messagehandler",
		"averages": "1b206e08-91d0-47cc-80fe-bf2f416beada_Averages",
		"channel": "1eea763a-4a16-421e-9e16-0b24d9f50b78",
		"channelId": "1eea763a-4a16-421e-9e16-0b24d9f50b78",
		"description": "Raises aggregated measurements by means of performing an average during a defined time period",
		"durationInSeconds": "60",
		"handlerConfigurationDescriptionIdentifier": "98b3162d-41a2-41e7-91b5-050c3f15d667",
		"handlerConfigurationId": "1b206e08-91d0-47cc-80fe-bf2f416beada",
		"handlerConfigurationIdentifier": "1b206e08-91d0-47cc-80fe-bf2f416beada",
		"handlerDescriptionId": "98b3162d-41a2-41e7-91b5-050c3f15d667",
		"handlerDescriptionIdentifier": "98b3162d-41a2-41e7-91b5-050c3f15d667",
		"handlerIdentifier": "1b206e08-91d0-47cc-80fe-bf2f416beada",
		"hostVersion": "2.2.0.0",
		"identifier": "1b206e08-91d0-47cc-80fe-bf2f416beada",
		"inputSubjects": null,
		"name": "Average In Period",
		"outputSubjectRoutes": {},
		"projectId": "51deb092-0ffa-bc4d-395b-5ff42e6f03ed",
		"purpose": "Production",
		"state": "Operational",
		"version": "1.0.0.40"
	  }

<!-- end of code block -->
 