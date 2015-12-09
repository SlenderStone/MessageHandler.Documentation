# Setting up a solution

In this article we will dive deeper into a proposed solution structure for custom handler development. More specifically we'll look only at setting up the solution structure, including source control repository and nuget dependencies. The goal of this document to gather all practices that the majority of handler developers accept as their standard, so that any new developer can get up to speed quickly. Don't feel required to follow any of it though (or better share your thoughts with us if you have better practices.)

## What will we do in this article

 * Set up a source repository for our handler
 * Create a visual studio solution
 * Add the necessary nuget dependencies
 
## Setting up a source repository

It is advised to set up a source repository per handler using a source control system of your choice. If you want this source control system to integrate with our build services, then it has to support either Git, Mercurial, Subversion or TeamFoundationServer and be publicly addressable. Note: If your source control system is behind the corporate firewall, there are other ways to push your handler packages as well.

In this article, we'll use a public repository on github: [https://github.com/MessageHandler/MessageHandler.Handlers.AverageInPeriod](https://github.com/MessageHandler/MessageHandler.Handlers.AverageInPeriod). This repository will contain a handler that computes an average value over a given period in time.

Using a repository per handler has the following benefits

 * Commit history, branching & versioning is distinct per handler
 * Easier for users to track issues per handler
 * Easier to integrate with our continuous integration system
 
Of course this approach also comes with it's own set of challenges, but these can be mitigated quite easily

 * Use organizations, teams or equivalent to handle authorization for a group of repositories
 * Use aggregated issue trackers (like [waffle.io](http://www.waffle.io) or equivalent) to organize your issues at the team level
 * Use local folders to group repositories on your disk

Once you have setup your repository, clone the repository locally 
 
 `C:\github.org\mh\handlers> git clone https://github.com/MessageHandler/MessageHandler.Handlers.AverageInPeriod.git averageinperiod`
 
If github didn't do it for you automatically, make sure to add the correct .gitattributes and .gitignore files for c# projects to your repository

## Create a visual studio solution

Now that your local repository is all set up, it is time to create the visual studio solution. Handlers are supposed to be very simple and understandable, so the less structure your solution has the better.

 * Open Visual Studio 2013
 * Navigate to File > New Project > Visual C# > Class Library
 * Make sure **'.Net Framework 4.5.1'** or newer is selected
 * Name: **AverageInPeriod**
 * Location: **'C:\github.org\mh\handlers\averageinperiod\'**
 * Unmark 'Create directory for solution'

After creating the class library project, I prefer to rename the root folder from **AverageInPeriod** to **src**.
 
## Add nuget dependencies

The final step before we can start development of our handler is to include the necessary nuget packages. For now these packages can only be found on our public MyGet feed (once all the dust has settled the stable versions will become available on nuget as well)

If you haven't done so already, you should add our MyGet feed to your list of package sources in the following way

 * Navigate to Tools > Options > NuGet Package Manager > Package Sources
 * Add
	- Name: **'MessageHandler Myget'**
	- Source: **'https://www.myget.org/F/messagehandler/'**
 * Click OK
 
Then open the NuGet package manager:

 * Right click Project > References
 * Click Manage NuGet Packages
 * Open Online > **'MessageHandler Myget'**
 * Make sure **'Include Prerelease'** is selected instead of 'Stable only'
 
Now you should see 2 packages

 * **MessageHandler**: This is the SDK you need in order to develop handlers
 * **MessageHandler.Emulator**: Optional, this package allows you to emulate a handler runtime locally and can be useful in order to test the behavior of your handler.
 
Make sure you install at least the SDK and it's dependency.

Note: It has a dependency on a specific version of the Rx framework, at present it is impossible to deviate from this version.
