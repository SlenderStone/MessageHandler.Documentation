# Uploading a new handler package 

In order to let your buildserver, or other tooling, upload a new package version of your handler into our system, it needs to perform an authorized POST request on the following uri:

`https://api.messagehandler.net/handlers/definitions/{definitionid}/{version}/package`

with following headers

<!-- start of code block -->
 
	Accept: "application/json"
	Content-Type: "application/zip"
	Authorization: "Bearer access_token"
    
<!-- end of code block -->

### Parameters

 * definitionid: The identifier of your handler definition, e.g. "98b3162d-41a2-41e7-91b5-050c3f15d667".
 * version: A string representing a version number, e.g. "1.0.0.0"
 
### Body

The body content must contain a zip file, which contains the binaries of your handler's build output (content of bin folder)

### Response

<!-- start of code block -->

	Status: 200 OK

<!-- end of code block -->

### Notes

* **Versioning:** Each time you push a new package, the version number needs to be increased. The system will not allow you to overwrite existing versions.
* **Review required:** Package content will require a review of one of our trusted reviewers before being accepted into the system, so a newly uploaded version may not immediately be available for usage.



