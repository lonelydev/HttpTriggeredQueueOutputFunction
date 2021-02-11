# Continuous Integration and Continuous Delivery

Every software project that has several contributors would benefit from some verification like a build, some linting or something of that sort to ensure that any pull request to the main branch doesn't really break the live application.

## Continuous integration

This is a practice where developers merge their changes to the main branch as often as possible and as soon as the changes are merged or sometimes even when a pull request is created, a build is triggered automatically that compiles code and runs some unit tests and ensures that the project continues to work after the merge. 

It is an automated check on whether the application is broken or not by the new code. 

## Continuous Delivery

So earlier we saw that there were automated builds being triggered by the merge or pull request. Now, how about triggering a process that would ready your application to be deployed. This process is called continuous delivery. Getting a commit/merge, to trigger a build that creates artifacts that are ready to be released to production at the click of a button.

## Continuous Deployment

This is an extension to the earlier step, where you not just create the artifact and be ready for someone to click a button, but you actually automate the deployment directly to your subscribers/consumers without any human intervention. This is like the dream for many small companies that are just beginning to embrace tech. And very few companies have really achieved this level of development practices.

## How do I do this for my Azure function?

There are many ways to do this for your Azure function based on where your repository is hosted.

The [microsoft docs page for functions deployment](https://docs.microsoft.com/en-us/azure/azure-functions/functions-continuous-deployment) is a great starting point.

### Pre-requisites

As stated in the docs that I referenced earlier, the project's folder structure must follow a certain pattern. 
Every Function must have its function.json. The entire function app must have a single host.json and any shared code must be in its own folder.

```
   FunctionApp
    | - host.json
    | - MyFirstFunction
    | | - function.json
    | | - ...  
    | - MySecondFunction
    | | - function.json
    | | - ...  
    | - SharedCode
    | - bin
```

To remind you, the `host.json` file lists the runtime configurations for your entire function app and not just a particular function and hence must be in the root folder of the function app. 

As function apps can be written in multiple languages, however, for all function apps verison 2.x and higher, you cannot have one function app, with different functions written in different languages. The entire app will be of one specific language.

All details of this can be seen in the docs I referenced in the earlier section.

### Deploy using Azure Devops - Azure Pipelines

Azure Pipelines as described earlier is part of an offering on Azure Devops. Let us explore ways to deploy our azure function by describing the steps in a [YAML file](https://yaml.org/).

In your YAML file you will describe how to build and then deploy your Azure Function App.

You could just copy and paste the yaml snippet for the programming language that used to build your azure function app from the [docs](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-azure-devops?tabs=csharp)

In this case, csharp. Copy the build section and then the deploy section and create a yaml file. 

Often you will have to create sections where you override some application settings too. 

### Create your Yaml using Azure Devops Pipeline

When you go to the Pipelines section of your organisation's Azure Devops, it will allow you to create a new azure pipeline. 

This will take you through some steps, seeking your input in which repository you want to create this yml file for and will automatically analyse your repository to find out what type of project it is and also seek the subscription from you and finally generate the YAML. 

Based on my experience this is the easiest way to setup the YAML file. And you'll be able to make further changes to this too. I only tried out hte yml creation using the CLI thinking that would be quicker, as I come from a linux background. But nope. I can assure you it is not. So please stick to the Azure Devops pipeline way of creating a yaml file. This way you not only get the build but also the deploy tasks created in the YAML.

In fact following the wizard on the website will also allow you to save the changes to the repository! 

When your pipeline is ready, go select the pipelin from Azure Devops and run it. 

### Try YAML creation using Azure CLI (might as well not do this as the earlier method is better)

First go download and install [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

Then open up your project directory in a terminal.

Run the following command, in the function project's directory (not the solution directory, unless they are the same directory): 
`az functionapp devops-pipeline create`

Can be used to create a yaml file that describes a build pipeline in Azure!

If you are running this for the first time, you'll be asked to login to your account by using `az login` command, which should open your browser so that you can log into the Azure Portal, after which the Azure command line will fetch all the subscriptions that you have. 

Now if you ahve more than one subscription, then you have to let the app know which subscription you'd like to deploy this app to. It will most likely automatically pick the first ever subscription you had. 

To check what subscription the CLI is referring to, use `az account show`. This will print the account and subscription.
Only the subscription that it has chosen as a target. 

To set another subscription as the target, run `az account set --subscription <subscription name or id>`

Then run the earlier comand again in the same location as the `host.json` file.

A thing to note is that the command is a bit over the top. It will initialise a new git repo locally inside your project directory! I'm glad that I know the workings of git to undo this crazy behaviour. But it did create a yml file. 
However, it will ask you for the following:
 
* Where it should pick the source code from
* the name of the function app
* the name of the azure devops organisation (in case you have many)
* the name of the project on azure devops
* the repository in azure devops where you'd like to push your yaml file to.

This cli tool also creates a default git repo with the main branch named `master` inside your project directory, this could also confuse you into thinking what just happened to the branch you were working on. All because it initialised this new git repo inside the function app directory. 

Anyway, I've averted the disaster locally by not agreeing to force push the changes to remote. 

Let's take a look at the yml file. 

I'm slightly disappointed. It only created the yaml for the build. But we can take a look at how to deploy later. or just refer to the earlier [Deploy Using Azure Devops Pipelines](###deploy-using-Azure-Devops---azure-pipelines)

## How to setup KeyVaults for your Azure Function App

[How to Azure Key Vault your Function App](./AzureKeyVault.md)

[Back to Main page](./README.md##function-on-portal)
