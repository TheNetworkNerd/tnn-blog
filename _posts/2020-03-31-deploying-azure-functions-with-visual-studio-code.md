---
title: "Deploying Azure Functions with Visual Studio Code"
date: 2020-03-31
media_subpath: /assets/images
permalink: /2020/03/31/deploying-azure-functions-with-visual-studio-code/
categories: 
  - "Cloud"
  - "Code"
tags: 
  - ".NET"
  - ".NET Core"
  - "Azure"
  - "Azure Function Apps"
  - "Azure Functions"
  - "C#"
  - "Code"
  - "Deploying Azure Functions"
  - "FaaS"
  - "Function Apps"
  - "Functions As A Service"
  - "LearningtoCode"
  - "Microsoft"
  - "Microsoft Azure"
  - "Programming"
  - "Serverless"
  - "Visual Studio Code"
  - "VS Code"
image: "0_AzureFunctionsVSCode.png"
---

This is part 2 of my quest to explore Azure Functions for self-education in functions-as-a-service.  The goal of this post is to use VS Code to deploy, test, and edit a simple Azure Function.

[Part 1]({% link _posts/2020-02-29-a-first-voyage-into-azure-functions-and-function-apps.md %}) was a first step toward understanding and using Azure Functions.  We created a Function App, added a function to it using the Azure portal, and made some simple tweaks.  In this post, it's time to take the next step.  Keep in mind as a beginner I'll be documenting both successes and failures throughout this post, so you may want to read it completely before following all steps in order.  Hopefully this helps someone avoid making the same mistakes.

In an effort to make the Azure function deployment process more automated and learn some code at the same time, here we go.

Go back to the Azure portal, and locate our previously created Function App (named networknerd0).  Click the icon shown below to begin adding functions to this Function App.

![](1_AddFunction.png)

This next screen is a familiar one.  We could choose a template, but let's run through the Quickstart again.

![](2_ChooseTemplate-1024x386.png)

Once the Quickstart menu opens, choose the option for [VS Code](https://code.visualstudio.com/download) (or Visual Studio Code) as our development environment, and click Continue.

![](3_AzureFunctionsfor.NETStep1-1024x545.png)

On the next screen, let's choose Directly publish from VS Code for the time being (in the spirit of walking before we run).  Then click Continue.  We will use VS Code to build the functions, do as much as we can to verify they work as expected, and then push them up to Azure.

![](4_AzureFunctionsfor.NETStep2_DirectPush.png)


### Installing the Dependencies

On the next screen we find there's more to the process than just installing VS Code and starting to write code.

![](5_AzureFunctionsfor.NETStep3DependencyInstall-1024x555.png)

To give context, all steps mentioned here were performed on a laptop running Windows 10 Home 18363. First, installing [Visual Studio Code](https://code.visualstudio.com/download) was easy.  I downloaded Node.js version 12.16.1 from [here](https://nodejs.org/en/download/) and installed it with no issues (link should point to latest LTS version installer for Windows / Mac).  Before running the npm command above, I downloaded and installed [.NET Core 3.1](https://dotnet.microsoft.com/download) (selected option to Download .NET Core SDK since [this post](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs-code?tabs=csharp#additional-requirements-for-running-a-project-locally) indicates we need .NET Core CLI to run projects locally, which is included in the SDK per [this post](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)).

Next, run the code as mentioned in the previous screenshot.  The screenshot below shows the install as being successful.

```bash
npm install -g azure-functions-core-tools
```

![](6_AzureFunctionsCoreToolsInstall-1024x365.png)

Now, open up Visual Studio Code.  Click on the extensions icon.  Notice we've done a search for the Azure Functions extension and have selected it in the search results.  Click the Install button to install the extension.

![](7_VSCodeAzureFunctionsInstall-1024x280.png)

Once the extension is installed, we can see a new option on the left-hand toolbar in VS Code.  After clicking on it, select the option to Sign in to Azure.

![](8_VSCodeAzureFunctionsExtensionSelect.png)

After signing in to Azure successfully with an account that has access to the subscription we will use for this, the following message will show in our browser.

![](9_AzureSignInVSCodeAuth.png)

 
### Creating a Project

Back in VS Code, we can see the Function App named neworknerd0 which was previously created under our Azure subscription.  Click the option to create a project.

![](10_CreateProject.png)

At this point, specify a folder for storing all required project files and information.  In this case, I chose to create a folder named Azure Functions Project 1 (located inside Documents\\VSCode on my laptop).  The folder location and name are completely up to you.  For project language, we will select C#.

![](11_ProjectLanguage.png)

Technically, we can create the project without creating any functions, but go ahead and leverage this workflow to create a function.  Which template should we use this time?  Since we discussed HttpTrigger functions last time, let's stick with this template to get the process down.

![](12_FunctionTemplate.png)

Now we need to provide a function name.  This is our second function, so name it FN2-HTTP2-Test.  Be sure to press Enter to move to the next step.

![](13_FunctionName.png)

This next menu is new.  There was no option to provide a namespace when we did this in the Azure portal.  What should we use here?  For more information on what namespaces are in the C# language, see [this document](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/namespaces/).  It's essentially a way to help organize code and allow referencing of the namespace in other functions for code reuse.  Let's go with NetworkNerd.Functions.  If we create more functions, we can put them into this namespace as well or create a new namespace.  Be sure to press Enter to continue.

![](14_NamesapceName.png)

Now it's time to set access rights.  If you recall from last time, we left the HTTP authorization level set to Function.  That makes the URL a bit longer and harder to guess, so let's use the same level of access rights again here.

![](15_AccessRights.png)

In the bottom right corner of VS Code we will see the project's creation progress.  It happened very fast, but here are a couple of screenshots.

![](16_CreatingProject.png)

![](17_ProjectCreated.png)

Once the project has been created, a code editor window opens.  Wait a minute.  There are more lines of code here than what we saw when creating a function in the Azure portal.  In fact, we only saw the code inside the red rectangle below.  And the number of "using" statements calling other namespaces is longer than it was when we used the Azure portal.

![](18_FunctionCode_1-1024x460.png)

Remember at this point the function does not exist in Azure.  Even though it was not shown earlier, there was more to that Quickstart page from the Azure portal.

![](19_Quickstart2.png)

We've already completed the step for creating a function and can see the code.  The only thing left to do is press F5 to try see if the code will run successfully.

 

### Overcoming Errors

If only things would just work the first time we try, right?  As you probably guessed, pressing F5 to run (or build and run) our function did not work.  Once you press F5 from the code editor, the TERMINAL tab shows the build execution task.  We were doing fine until the very end of the process, which is littered with errors.

![](20_SDK_Error_Terminal-1024x263.png)

For searchability, I'll wrap the actual text in code tags.

```
> Executing task in folder VSCode: C:\Program Files\dotnet\dotnet.exe clean /property:GenerateFullPaths=true /consoleloggerparameters:NoSummary 
< Microsoft (R) Build Engine version 16.5.0+d4cbfca49 for .NET Core Copyright (C) Microsoft Corporation. All rights reserved. Build started 4/11/2020 5:48:33 PM. Terminal will be reused by tasks, press any key toclose it. 
> Executing task in folder VSCode: C:\Program Files\dotnet\dotnet.exe build /property:GenerateFullPaths=true /consoleloggerparameters:NoSummary 
< Microsoft (R) Build Engine version 16.5.0+d4cbfca49 for .NET Core Copyright (C) Microsoft Corporation. All rights reserved. C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj : warning NU1603: Microsoft.Azure.WebJobs.Extensions 3.0.5 depends on Microsoft.Azure.WebJobs.Host.Storage (>= 3.0.11) but Microsoft.Azure.WebJobs.Host.Storage 3.0.11 was not found. 
An approximate best match of Microsoft.Azure.WebJobs.Host.Storage 3.0.13 was resolved. Restore completed in 1.8 sec for C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj. 
C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj : warning NU1603: Microsoft.Azure.WebJobs.Extensions 3.0.5 depends on Microsoft.Azure.WebJobs.Host.Storage (>= 3.0.11) but Microsoft.Azure.WebJobs.Host.Storage 3.0.11 was not found. 
An approximate best match of Microsoft.Azure.WebJobs.Host.Storage 3.0.13 was resolved. Azure Functions Project 1 -> C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\bin\Debug\netcoreapp2.1\bin\Azure Functions Project 1.dll C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : It was not possible to find any compatible framework version [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj] 
C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : The framework 'Microsoft.NETCore.App', version '2.1.0' was not found. [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj] 
C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : - The following frameworks were found: [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj] 
C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : 3.1.3 at [C:\Program Files\dotnet\shared\Microsoft.NETCore.App] [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj] 
C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj]
C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : You can resolve the problem by installing the specified framework and/or SDK. [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj] 
C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj] 
C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : The specified framework can be found at: [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj] 
C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : - https://aka.ms/dotnet-core-applaunch?framework=Microsoft.NETCore.App&framework_version=2.1.0&arch=x64&rid=win10-x64 [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj] 
C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj] 
C:\Users\Nick\.nuget\packages\microsoft.net.sdk.functions\1.0.31\build\Microsoft.NET.Sdk.Functions.Build.targets(41,5): error : Metadata generation failed. [C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj] The terminal process terminated with exit code: 1
```

The actual text output shows some warnings and many, many errors.  As I mentioned earlier in this post, I installed .NET Core 3.1 earlier.  It was my understanding that this included .NET Core 2.1 (which was referenced in the Quickstart instructions).  In fact, the link on the Quickstart page takes you to the .NET Core 3.1 download page.  But in the error messages above, we are clearly missing something having to do with the .NET Core, and [this link](https://aka.ms/dotnet-core-applaunch?framework=Microsoft.NETCore.App&framework_version=2.1.0&arch=x64&rid=win10-x64) is referenced.  Once again, the link gets directed to a .NET Core 3.1 download page.  It took a little searching, but [this page](https://dotnet.microsoft.com/download/dotnet-core/2.1) is the right one to use for downloading .NET Core 2.1.  Once I downloaded and installed it on my machine, all of the errors above were cleared, but new ones popped up in their place.

![](21_TerminalErrorsAgain-1024x302.png)

Once again for searchability, here's the text output.

```
Executing task in folder VSCode: C:\Program Files\dotnet\dotnet.exe build /property:GenerateFullPaths=true /consoleloggerparameters:NoSummary 
< Microsoft (R) Build Engine version 16.5.0+d4cbfca49 for .NET Core Copyright (C) Microsoft Corporation. All rights reserved. 
C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj : warning NU1603: Microsoft.Azure.WebJobs.Extensions 3.0.5 depends on Microsoft.Azure.WebJobs.Host.Storage (>= 3.0.11) but Microsoft.Azure.WebJobs.Host.Storage 3.0.11 was not found. 
An approximate best match of Microsoft.Azure.WebJobs.Host.Storage 3.0.13 was resolved. 
Restore completed in 65.39 ms for C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj. 
C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\Azure Functions Project 1.csproj : warning NU1603: Microsoft.Azure.WebJobs.Extensions 3.0.5 depends on Microsoft.Azure.WebJobs.Host.Storage (>= 3.0.11) but Microsoft.Azure.WebJobs.Host.Storage 3.0.11 was not found. 
An approximate best match of Microsoft.Azure.WebJobs.Host.Storage 3.0.13 was resolved. 
Azure Functions Project 1 -> C:\Users\Nick\Documents\VSCode\Azure Functions Project 1\bin\Debug\netcoreapp2.1\bin\Azure Functions Project 1.dll Terminal will be reused by tasks, press any key to close it. 
> Executing task in folder VSCode: func host start < func : File C:\Users\Nick\AppData\Roaming\npm\func.ps1 cannot be loaded because running scripts is disabled on this system. 
For more information, see about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170. 
At line:1 char:1 + func host start + ~~~~ + CategoryInfo : SecurityError: (:) [], PSSecurityException + FullyQualifiedErrorId : UnauthorizedAccess The terminal process terminated with exit code: 1 
Terminal will be reused by tasks, press any key to close it.
```

This time we have one error and some warnings.  Let's start with the error first and try to clear it.  Following the [link from the error message](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7) takes us to a page on Powershell execution policies.  Part of running the function somehow involves Powershell, so we'll have to make a tweak.  Open Powershell (running as Administrator), and run set-executionpolicy remotesigned.  Type y and press Enter to say yes to the prompt about changing the execution policy.  That is the default policy for Windows Servers and will work for our purposes to (hopefully) clear the error.

![](22_PowershellExecutionPolicy.png)

Let's take a second to address the warning messages too.  These messages are similar to those found in [this post](https://github.com/Azure/azure-webjobs-sdk/issues/2386), which recommends installing a newer version of Microsoft.Azure.WebJobs.Extensions for this project.  From a terminal window inside VS Code, install the latest version of the Microsoft.Azure.Webjobs.Extensions package (3.0.6) using the code snippet on [this site](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/).  Notice the path you see here is the file path to the folder for this project (C:\\Users\\Nick\\Documents\\VSCode\\azure functions project 1 - created in an earlier step).

![](23_WebJobs.Extensions_3.0.6-1024x183.png)

That in and of itself did not clear the warning.  I had to follow a similar procedure to update to a newer version of Microsoft.Azure.Webjobs.Host.Storage (3.0.14) using the code snippet from [this site](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Host.Storage/3.0.14).

![](24_WebJobs.Host_.Storage-1024x184.png)

Running the function again locally was a success (no warnings, no errors)!  We are given a specific web address to open on the computer running VS Code that can be used to test the function.  What happens if we visit that address in a browser?

![](25_SuccessfulRun.png)

Following the link yields exactly what we would expect.  There was no name parameter passed into the query string, so we see the standard message that was part of the HttpTrigger template used.  The URL path matches the same pattern we've seen previously.  But, as you will notice below, even though we set the HTTP authorization level to Function when the function was created, there is no API key in the URL since this function is running completely local to the laptop being used.  The API key would not be present until we deploy the code to Azure.

![](26_LocalFunctionCallNoQueryString.png)

If we add the name parameter to the query string just for fun, we see the results are once again as expected.

![](27_LocalFunctionwithQueryString.png)

So long as the function continues to run in VS Code (i.e. you don't press CTRL + C to kill it in the terminal window), the function can be run and tested any number of times.  Here's what we see in the terminal window during a specific function execution (a single visit to the HTTP address).  A status of 200 means OK and that the request has succeeded.

![](28_VSCodeTerminalWindow-1024x323.png)

 
### Deploying to Azure

As stated earlier, the function we created exists only on the laptop used to create it and has not yet been pushed up to Azure.  Let's find out how that part works.  Back inside VS Code, go back to the Azure Functions extension area (step 1 in screenshot below).  Expand our Azure subscription as well as the local project we previously created that contains the new function.

![](29_VSCode_AzureFunctionsExpandAll.png)

After expanding the tree, we can see our previously created function (FN1-HTTP1-Test) inside the Function App networknerd0 and the function we created in VS Code (FN2\_HTTP2\_Test) shown under Local Project.  Click on FN2\_HTTP2\_Test to highlight it in the tree, and then click on the Deploy to Function App button.

![](30_DeploytoAzureButton.png)

At this point we are presented with the option to create a new Function App to deploy our function to or to select an existing Function App.  We will choose the option to deploy to the networknerd0 Function App.

![](31_SelectFunctionApp.png)

Soon after clicking networknerd0, the following dialog box came up indicating a difference in remote version and local version.  Click the Learn more option to get additional information.

![](32_RemoteVersionError_1.png)

We are directed to this [GitHub page](https://github.com/Microsoft/vscode-azurefunctions/wiki/Project-Runtime).  The local project runtime is set to 2.x somehow, and if you recall from [the last post]({% link _posts/2020-02-29-a-first-voyage-into-azure-functions-and-function-apps.md %}), the Function App was set to .NET Core 3.1 by default.  Take a look at the contents of function.json in the code editor when we select the function to be deployed in the left-hand tree.  The GitHub page mentioned we're looking for a settings.json file inside .vscode at the root of our repo, so click on VSCode.  That is a file folder on my laptop (created by me much earlier) one level above the folder where all code files for this project (Azure Function Project 1) live.

![](33_functioncode-1024x339.png)

Notice we can use the intelligent menus to navigate to the .vscode\\settings.json file previously mentioned.

![](34_NavigateToSettings.png)

Looking at the code in this file, we can easily see that azureFunctions.projectRuntime is indeed set to ~2.

![](35_ProjectRuntime.png)

Let's change that field to ~3 and save the changes.  That should change the runtime for any functions created within this project (both existing and new).  I'm not 100% certain if creating a new project would require this change as well, but it seems like it would.

![](36_ProjectRuntime2.png)

To be thorough, I recommend going back to the function code (stored in FN-HTTP2-Test.cs) and re-running (as we showed earlier in this post) it to confirm everything still works as expected.  As part of the run, the function will be rebuilt locally.  In my tests, everything still worked fine, which means we should be a go for deployment to Azure.

If we then go back to the left-hand tree, select our function (FN2\_HTTP2\_Test), click the Deploy to Function App button, and select the networknerd0 Function App, what happens?  This time we're met with a message asking if we are sure we want to deploy to networknerd0 and that all previous deployments will be overwritten.  Let's be bold and click Deploy.

![](37_AreYouSure.png)

As the process kicked off, here's what things looked like in VS Code (notifications in bottom right corner).  As the pre-deploy task ran, we could see the function getting rebuilt (output inside the TERMINAL window in VS Code) before it was deployed.

![](37_PreDeployTask.png)

![](38_DeployingToNetworkNerd0.png)

![](39_DeploymentComplete.png)

Once the deployment finished, here's what was shown in the Output window in VS Code: 
```
1:57:41 PM networknerd0: Creating zip package... 
1:57:45 PM networknerd0: Starting deployment... 
1:57:49 PM networknerd0: Fetching changes. 
1:57:50 PM networknerd0: Cleaning up temp folders from previous zip deployments and extracting pushed zip file D:\local\Temp\zipdeploy\jdoalsxz.zip (3.16 MB) to D:\local\Temp\zipdeploy\extracted 
1:57:53 PM networknerd0: Updating submodules. 
1:57:53 PM networknerd0: Preparing deployment for commit id '6b5f0e1fd0'. 
1:57:55 PM networknerd0: Generating deployment script. 
1:57:55 PM networknerd0: Using the following command to generate deployment script: 'azure site deploymentscript -y --no-dot-deployment -r "D:\local\Temp\zipdeploy\extracted" -o "D:\home\site\deployments\tools" --basic --sitePath "D:\local\Temp\zipdeploy\extracted"'. 
1:58:01 PM networknerd0: Generating deployment script for Web Site 
1:58:01 PM networknerd0: Generated deployment script files 1:58:01 PM networknerd0: Running deployment command... 
1:58:01 PM networknerd0: Command: "D:\home\site\deployments\tools\deploy.cmd" 
1:58:03 PM networknerd0: Handling Basic Web Site deployment. 
1:58:06 PM networknerd0: Creating app_offline.htm 
1:58:06 PM networknerd0: KuduSync.NET from: 'D:\local\Temp\zipdeploy\extracted' to: 'D:\home\site\wwwroot' 
1:58:06 PM networknerd0: Copying file: 'Azure Functions Project 1.deps.json' 
1:58:06 PM networknerd0: Copying file: 'host.json' 
1:58:06 PM networknerd0: Copying file: 'bin\Azure Functions Project 1.dll' 
1:58:06 PM networknerd0: Copying file: 'bin\Azure Functions Project 1.pdb' 
1:58:06 PM networknerd0: Copying file: 'bin\extensions.json' 
1:58:06 PM networknerd0: Copying file: 'bin\function.deps.json' 
1:58:06 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Authentication.Abstractions.dll' 
1:58:06 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Authentication.Core.dll' 
1:58:06 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Authorization.dll' 
1:58:06 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Authorization.Policy.dll' 
1:58:06 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Hosting.Abstractions.dll' 
1:58:06 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Hosting.Server.Abstractions.dll' 
1:58:06 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Http.Abstractions.dll' 
1:58:06 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Http.dll' 
1:58:06 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Http.Extensions.dll' 
1:58:06 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Http.Features.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.JsonPatch.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Mvc.Abstractions.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Mvc.Core.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Mvc.Formatters.Json.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Mvc.WebApiCompatShim.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.ResponseCaching.Abstractions.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Routing.Abstractions.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.Routing.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.AspNetCore.WebUtilities.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.Azure.WebJobs.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.Azure.WebJobs.Extensions.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.Azure.WebJobs.Extensions.Http.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.Azure.WebJobs.Host.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.Azure.WebJobs.Host.Storage.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.Build.Framework.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.Build.Utilities.Core.dll' 
1:58:07 PM networknerd0: Copying file: 'bin\Microsoft.DotNet.PlatformAbstractions.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.Configuration.Abstractions.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.Configuration.Binder.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.Configuration.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.Configuration.EnvironmentVariables.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.Configuration.FileExtensions.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.Configuration.Json.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.DependencyInjection.Abstractions.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.DependencyInjection.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.DependencyModel.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.FileProviders.Abstractions.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.FileProviders.Physical.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.FileSystemGlobbing.dll' 
1:58:08 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.Hosting.Abstractions.dll' 
1:58:09 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.Hosting.dll' 
1:58:09 PM networknerd0: Copying file: 'bin\Microsoft.Extensions.Logging.Abstractions.dll' 
1:58:09 PM networknerd0: Omitting next output lines... 
1:58:10 PM networknerd0: Finished successfully. 
1:58:10 PM networknerd0: Running post deployment command(s)... 
1:58:10 PM networknerd0: Triggering recycle (preview mode disabled). 
1:58:13 PM networknerd0: Syncing 2 function triggers with payload size 250 bytes successful. 
1:58:14 PM networknerd0: Deployment successful. 
1:58:56 PM networknerd0: Querying triggers... 
1:59:19 PM networknerd0: WARNING: Some http trigger urls cannot be displayed in the output window because they require an authentication token. Instead, you may copy them from the Azure Functions explorer.
```

That last line from the Output window likely fired because we chose to stick with Function as the HTTP authorization level when creating this function.  Also, notice how our left-hand tree has updated to show the newly added function on the Azure side.  There is still a local copy in our project folder as well.

![](40_UpdatedTree.png)

If we furthermore expand Deployments in the tree, we can see the push deployment made to Azure.  Right-click this to see deployment logs or to trigger a redeploy at any point (redeploy not supported unless using Git).  There is an option to Open in Portal as well.  This will take you to the Azure portal to see a deployment overview (a page with a ton of parameters) as well as some basic activity logs (not as detailed as what can be seen in VS Code).

![](41_DeploymentsTree_1.png)

 
### Checking Our Work In Azure

Going back to our Function App networknerd0 in the Azure portal shows our newly deployed function as you would expect, but for some reason the Functions are marked as Read Only.

![](42_FunctionsReadOnly.png)

Selecting the newly created function from the tree in the screenshot above gives us a hint as to what might be amiss.  This says the app itself (the Function App) has been placed in read only mode due to publishing a generated function.json.  Reading [this post](https://docs.microsoft.com/en-us/azure/azure-functions/functions-dotnet-class-library#conversion-to-functionjson) highlights this has something to do with the build process used in a tool like VS Code.

![](43_ReadOnlyMessage-1024x351.png)

Let's notice something else interesting about the above screen.  We don't actually see the function code.  We just see the contents of function.json but (as you would expect) cannot make any changes to it.  We could still run the function using the testing tools in the portal, but without being able to see the code, that doesn't seem really helpful.

What if we contrast that with what we see when we select FN1-HTTP1-Test?  This was the function built and edited in the Azure portal in our last post.

![](44_FirstFunction-1024x379.png)

Notice the function.json file for this function has fewer parameters than for FN2\_HTTP2\_Test just as the previous articles explained.  Also, in the View files area, we could look at the run.csx file that contains all the C# code for the function but would not be able to edit it.  There was no run.csx file visible in the Azure portal for FN2\_HTTP2\_Test.  This area could still be used to run and test the function or get its URL but not to edit it.

Go back to our newly created function for a minute (FN2\_HTTP2\_Test).  Click the option to Get function URL.  We expect the URL to have an API key in its path due to the way we originally built the function's security.

![](45_GetURL.png)

If we copy that URL to the clipboard and paste into a browser, does it work as expected?  The API key was indeed part of the URL once the function was deployed to Azure even though we did not see it when working with the function in VS Code.

![](46_BrowserTest1.png)

And if we add the name parameter to the query string, we get an expected result.

![](47_BrowserTest2-1024x57.png)
 

### Testing a Code Change and Redeploy

At this point based on everything that has happened to this point, aren't you wondering what happens if we change the code a little and redploy to Azure?  Let's try it.  Navigate to the code for FN2\_HTTP2\_Test inside VS Code.  It can be done in a similar manner to what is shown here or via the explorer option.

![](48_ReturntoFunctionCode.png)

All we need to test with is a small change.  Let's change the response messages just slightly.  The code block we want to change is currently lines 28-30 as shown here.

![](49_CodeBlock-1024x79.png)

Change the code to the following, and save the changes. 
```
string responseMessage = string.IsNullOrEmpty(name) 
? "Seeing this message means the function executed successfully, but try passing a name parameter into the query string." 
: $"Mr. / Mrs. {name} successfully passed a name parameter into the query string when executing this function. ";
```

Now, let's re-run this code locally to confirm it works as expected before we publish to Azure.  After running it locally, we conduct two different tests (one with no name parameter and one with a name parameter) whose results are shown in the following screen shots.

![](50_Rerun1-1024x85.png)

![](51_Rerun2.png)

Based on the code snippet above that was changed, we can see that the output from the function is what we would expect.  We'll use the same method in VS Code as before to deploy to Azure and confirm that we want to overwrite our existing deployment.

Once the deploy succeeded, we saw a new deployment event in VS Code as you would expect.

![](52_NewDeploy.png)

If we test the function again by visiting the URLs assigned by Azure (which will be the same as before when we tested), the results should match our output from testing locally.  I confirmed on my own that everything worked post-deployment but did not provide screenshots for brevity.

### Lessons Learned

It certainly took some time to clear the errors in VS Code before deploying to Azure, and now that we've reached this point, I wondered if tweaking that project setting for azureFunctions.projectRuntime to 3 would have avoided a need to install .NET Core 2.X.  Just for fun, I uninstalled .NET Core 2.X and tried to run the function.  Unfortunately, it was not successful.  A stated requirement of .NET Core 2.X is really a requirement.

You'll probably notice that despite naming our function with hyphens, VS Code ended up converting them to underscores upon function creation.  It's a little annoying and disrupted our standard naming convention, but it's something to keep in mind.

I chose not to dig into using Git just yet in this post because I wanted to get used to VS Code first.  Perhaps we will explore it next time.

I had no idea doing a deployment from VS Code to a Function App would cause the app and functions inside it to be read-only inside the portal, but it happened.  The approach will have to be all or nothing it seems...using a code editor to perform all deployments and the portal only for different administrative tasks or using the portal for everything.  Remember that all functions inside a Function App will be affected even if you only deploy a single function with VS Code.

It was harder to get the development environment tweaked than I thought it might be, but I learned many things along the way.

### Other Posts in This Series

- Part 1 - [A First Voyage into Azure Functions and Function Apps]({% link _posts/2020-02-29-a-first-voyage-into-azure-functions-and-function-apps.md %})
- Part 2 - This post
- Part 3 - [Push and Pull – Using VS Code with Azure Repos]({% link _posts/2020-04-25-push-and-pull-using-vs-code-with-azure-repos.md %})
- Part 4 - [Azure Functions and C# Code Intricacies – An Azure Pipelines Prequel]({% link _posts/2020-05-25-azure-functions-and-c-code-intricacies-an-azure-pipelines-prequel.md %})
- Part 5 - [From VS Code to Azure Pipelines – Basic CI/CD with Azure Functions]({% link _posts/2020-07-04-from-vs-code-to-azure-pipelines-basic-ci-cd-with-azure-functions.md %})
- Part 6 - [The Next Wave: Exploring Tanzu Observability’s Integration with Azure Functions]({% link _posts/2020-07-20-the-next-wave-exploring-tanzu-observabilitys-integration-with-azure-functions.md %})
- Part 7 - [Introduction to Distributed Tracing with Tanzu Observability and Azure Functions]({% link _posts/2021-01-10-introduction-to-distributed-tracing-with-tanzu-observability-and-azure-functions.md %})
- Part 8 – Coming soon!

### Further Reading

These articles were very helpful during the tinkering process to write this post.  I highly recommend reading them if you're headed down this path.

- [Quickstart: Create an Azure Functions project using Visual Studio Code](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-csharp)
- [Develop Azure Functions by using Visual Studio Code](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs-code?tabs=csharp)
