#Create and Publish a Package

There are a few ways to create a NuGet package. This tutorial walks you through creating a NuGet package from a project using the nuget CLI and publishing it to nuget.org.

##Pre-requisites
1. Visual Studio - If you don't have Visual Studio already, you can download [Visual Studio Community 2015](https://developer.microsoft.com/en-us/windows/downloads) for free.
2. NuGet CLI - Download the latest version of nuget.exe from [nuget.org/downloads](https://nuget.org/downloads), move it to a common location and add this path to the PATH Environment Variable. For more details, take a look at [The NuGet Install guide](/ndocs/guides/install-nuget#nuget-cli)

##Create a new Class library project

In Visual Studio, choose **File**, **New**, **Project**. In the **New Project** dialog, expand the **Visual C#** node and choose the **Windows** node, and then choose **Class Library**. Change the name to AppLogger. 

![Create new Project](/images/CreatePublishNugetSample/01.PNG)

Feel free to add your code to this library to perform the operations that you desire.  

On the **Build** menu, choose **Build Solution**. Make sure the solution builds successfully.


##Create the .nuspec file

Bring up the command prompt and navigate to the folder containing the `.csproj` file for the project that you just created. This path will look something like this
	`C:\Users\username\Documents\Visual Studio 2015\Projects\AppLogger\AppLogger`

Then run the <code>spec</code> command

<code class="bash hljs">
	nuget spec
</code>

This will generate a new file `AppLogger.nuspec` in the same directory as the `.csproj` file for the project.

Open this file with notepad or your favorite text editor. It will look something like this

	<?xml version="1.0"?>
	<package >
	  <metadata>
		<id>$id$</id>
		<version>$version$</version>
		<title>$title$</title>
		<authors>$author$</authors>
		<owners>$author$</owners>
		<licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
		<projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
		<iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
		<requireLicenseAcceptance>false</requireLicenseAcceptance>
		<description>$description$</description>
		<releaseNotes>Summary of changes made in this release of the package.</releaseNotes>
		<copyright>Copyright 2016</copyright>
		<tags>Tag1 Tag2</tags>
	  </metadata>
	</package>

This file includes tokens that are meant to be replaced at pack time, based on the project metadata stored in AssemblyInfo.cs (This can be found by expanding the properties node in the solution explorer).
To know more about how tokens are handled, read [Creating a nuspec file](/ndocs/create-packages/create%20a%20package#user-content-create-a--nuspec-file)


<div class="block-callout-warning">
	You must update the author and description or you will get an error in the next step.
</div>

Here is how the updated nuspec file looks:

	<?xml version="1.0"?>
	<package >
	  <metadata>
		<id>$id$</id>
		<version>$version$</version>
		<title>$title$</title>
		<authors>karann</authors>
		<owners>karann</owners>
		<requireLicenseAcceptance>false</requireLicenseAcceptance>
		<description>Awesome application logging utility</description>
		<releaseNotes>First release</releaseNotes>
		<copyright>Copyright 2016</copyright>
		<tags>application app logger logging logs</tags>
	  </metadata>
	</package>


It is a good practice to update the metadata tags to make it easier for others to find the package and understand what it does, and how to use it.

Having finalized the nuspec file, we are now ready to create the nuget package.

##Pack
On the **Build** menu, choose **Build Solution**.

Now run the <code>pack</code> command on the project

	nuget pack AppLogger.csproj

You will get warnings if you haven't updated the release notes and tags from the default value.

When the command has run successfully, it will generate a new file `AppLogger.1.0.0.0.nupkg`. This is your nuget package.


##Publish
Go to [nuget.org](https://www.nuget.org/) and register for an account or login if you already have one.

Click on <b>My Account</b> to see the API Key that was generated for you.

![api key](/images/CreatePublishNugetSample/03.PNG)

<div class="block-callout-warning">
	Always keep your API key a secret! If your key is accidentally revealed, you can always regenerate it at any time. You can also remove the API key if necessary. 
</div>

Open your command prompt and run the following command. Replace the key below with the key that was generated for you.

<code class="bash hljs">
	nuget push AppLogger.1.0.0.0.nupkg 47be3377-c434-4c29-8576-af7f6993a54b -Source https://www.nuget.org/api/v2/package
</code>

You should see something like this when the command has successfully executed.

	Pushing AppLogger.1.0.0.0.nupkg to 'https://www.nuget.org/api/v2/package'...
	  PUT https://www.nuget.org/api/v2/package/
	  Created https://www.nuget.org/api/v2/package/ 6829ms
	Your package was pushed.

You can go to your account on nuget.org and <b>Manage my packages</b> to see the package that you just published. You should also receive an email notifying you that the package was just published.

It might take a while for your package to be indexed and appear in search results. While that happens, you will see the following message on your package page.

![api key](/images/CreatePublishNugetSample/04.PNG)

That's it! You have just created and published your first NuGet package.

##Related Reading
* [Create a Package](/ndocs/create-packages/create-a-package)
* [Publish a Package](/ndocs/create-packages/publish-a-package)
* [Support multiple target frameworks](/ndocs/create-packages/supporting-mulitple-target-frameworks)
* [Dependency versions](/ndocs/create-packages/dependency-versions)
* [Creating localized packages](/ndocs/create-packages/creating-localized-packages)



