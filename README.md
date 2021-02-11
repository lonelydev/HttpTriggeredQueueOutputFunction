# Azure Functions

## What is an azure function?

As the name suggests, it is a function that runs on the cloud. But then you ask, all my code contains functions, and they can all be deployed on Azure, so wouldn't they all be azure functions?

Excellent question. I cannot blame you for such an inference.

Well, I oversimplified, when I said that it is just a function that runs on the cloud, I meant a whole load of things:

* Azure functions is a service offered by Microsoft on its cloud infrastructure, using which you can focus on just writing your application code and not worry about the maintenance of the hardware that it runs on.
* Functions on Azure can be charged on a pay as you go basis and hence you will only get charged according to the usage of the function.
* You also have the ability to scale out (more instances of the function automatically get live based on load) automatically based on your request load.

The idea behind this is generally known as [Serverless Computing](https://en.wikipedia.org/wiki/Serverless_computing)

Some other serverless offerings from Microsoft on Azure are:

* Microsoft Power Automate (never used it)
* Azure Logic Apps (build your apps visually, drag and drop activities and connect with several 100 application connectors to send notifications and what not)
* Azure Functions (what we are about to explore here)
* App Service Web Jobs (big brother of Azure Functions)

## So what?

The thing is, when I was first told about the things that I mentioned in the earlier section about an Azure Function, I was like "but I could use an Azure App Service for that and deploy a full blown application there".

You are not wrong in suggesting that. But quite often, you run into situations where you identify certain workflows and think of independent things that are to be executed part of that workflow as an activity or function.

You may not necessarily need something like a full blown application for that.

There are some scenarios where Azure functions kind of fit in neatly.

1. A simple HTTP API
2. Something that runs when a file is uploaded
3. Something that runs when there is a change in the database
4. A scheduled operation
5. An event processing system
6. Realtime data processing

The primary reason, why one would want to go the Azure Functions route is that they get all the benefits and control of maintaining an application, without having to worry about paying for a virtual machine and with additional automatic scale out options.

## Creating your first Azure function

This might sound like a lot of work. But it is a lot easier than you think. You can do this in many ways. I will use Visual Studio and a bit of the portal in some parts.

## Using Visual Studio

[Create an Azure Function in Visual Studio](./CreateAzureFunctionVisualStudio.md)

## Let us take a look at how to configure an important and useful feature

[Setting up Application Insights for your Azure function](./SetupAppInsights.md)

## Configure Azure Storage and link it to your app

[Setting up Azure Storage Account and link it to your app](./SetupStorageAccounts.md)

## Configure Continuous integration Pipeline on Azure DevOps

Azure Devops is a popular project management service offering from Azure. It provides you some cool stuff:

* Azure Boards - your agile/scrum/kanban boards to plan, track and collaborate
* Azure Pipelines - you get to build, test and deploy with CI/CD to the cloud!
* Azure Repos - host your prviate git repos and collaborate here
* Azure Test plans - Maintain a log of test execution and plans
* Azure Artifacts - Host your own nuget packages
* Extensions store - install cool extensions to your organisation free and paid!

So let us explore how to [create a pipeline yaml file](./SetupAzureDevopsPipeline.md)

## Function on Portal

What can we do with the function now?

We can run the function and we can see how it is logging all the requests to Application Insights. 

We can also run and test the function on Azure in the portal!

* Click on the Functions > Functions in the left hand side navigation bar

You'll be able to select the function you want in the right hand side preview pane and then presented with the option to test it!

## Cool function app local development trick in Visual Studio

If you ever wanted to download all the settings from the portal to your `local.settings.json` which is not committed to source control, you could use the package manager console and run a few commands. 
Run the following command inside the directory of the function app. if your solution directory and project directory are not the same, make sure you cd into your project's directory before you run the command. 

```pwsh
func azure functionapp fetch-app-settings <functionappnamegoesherewithoutanglebrackets>
func settings decrypt
```

That will only work within the **Visual Studio Nuget Package Manager environment**. This is not the Azure CLI, I am yet to find the equivalent to execute from Azure CLI so that you can achieve the same result. 

## How to setup KeyVaults for your Azure Function App

[How to Azure Key Vault your Function App](./AzureKeyVault.md)
