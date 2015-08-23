# Setting up a build

In this article we learn how to set up an automated build, which will turn our source code into a handler package.

## What will we do in this article

 * Set up a build
 * Run & validate the build
 * Use a webhook to automate the build
 * Validation & activation of your handler 
 
## Setting up a build

 * Go to **Handlers** and open the handler definition for which you want to define a build
 * On the handler definition detail page, in the actions menu, select `Manage build`
 * Click `Define build` in the actions menu
 * Fill out the details required to connect to your source control repository
 * Add additional package sources if needed
 * Hit `Save`

## Run & validate the build

 * On the recent builds page, to which you should be redirected after creation, hit `Start Build` and wait for the build to complete
 * Once the build completes, you can click on the entry in the build list to get a log of the build. Verify the build output to see if everything went as expected.
 
## Automate the build using a webhook

While you're waiting for the build to complete, it's a great time to automate the build on each check-in.

 * Copy the uri value next to `Hook (HTTP POST)` to your clipboard, using the copy button.
 * Go over to your source control provider, and add the uri value as a webhook into their system.
 * This will make sure that the build runs each time a check in occurs on your source code repository.
 
## Validation & activation

When a build succeeds, you will **not** be able to use your new handler version immediately. It will first be submitted for review to our review team. We understand that this is a minor inconvenience, but it is required to guarantee both the quality and security of the content on our platform.

After validation your new handler version will become available for use, unless it concerns the very first time that you want to use your handler. In that case you need to activate a specific version first. Activation basically implies assigning the default version that people, including yourself, will use when adding it to their channel. After adding, they can upgrade or downgrade to any desired accepted version.

 * Activation is done on the detail page of a handler definition by selecting a version and clicking the `activate` button.