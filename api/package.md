# Uploading a new handler package 

In order to let your buildserver, or other tooling, upload a new package version of your handler into our system, it needs to perform an authorized POST request on the following uri:

`https://api.messagehandler.net/descriptions/package/{descriptionid}/{version}`

with following headers

<!-- start of code block -->
 
	Accept: "application/json"
	Content-Type: "application/zip"
	Authorization: Bearer Base64Encode(access_token)
    
<!-- end of code block -->

The body content must contain a zip file, which contains the binaries of your handler's build output (content of bin folder)

## Notes

* **Finding the description id:** You can find the description id by navigation to your list of custom handlers, and open the handler for which you want to push a new package. You can find the descriptionid (guid) in the uri of the page uri.
* **Versioning:** Each time you push a new package, the version number needs to be increased. The system will not allow you to overwrite existing versions.
* **Review required:** Package content will require a review of one of our trusted reviewers before being accepted into the system, so a newly uploaded version may not immediately be available for usage.



