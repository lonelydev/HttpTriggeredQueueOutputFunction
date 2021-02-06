# Application Insights

Logging and monitoring a key part of every application in general. It becomes even more important when you go serverless as you don't have the option to log into a server and look for the operating system logs there. So it would be foolish to not configure logging for your Azure function. 

Luckily, Azure functions, are hooked into Microsoft's telemetry logging platform - Application Insights. As Microsoft describes it

    Application insights, a feature of Azure Monitor, is an extensible Application Performance Management service for developers and DevOps professionals.

Refer the [Application Insights official documentation](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview) to read more about this amazing offering. I can assure you that every one of my cloud hosted application is configured to push logs to Application Insights. There is a lot to talk about its features, however, for this tutorial, let us sticking to understanding how to configure your function app to log into app-insights. 

## Configuration

If you are using Visual Studio, like I was showing you in the earlier page, you can configure application insights directly from the IDE, within the same Publish window.

![publishoptions6postdeploymentscreen.png](images/publishoptions6postdeploymentscreen.png)

In the service dependencies section, you might have noticed that it shows Application Insights a configure button. 

It is totally upto you to do this in the IDE or the portal. For convenience and continuity, I'm going to do it from the IDE. I have done it from the portal too and I must admit, Microsoft has done a great job at keeping the experience very consistent. 

    PS: I will not be sharing as many screenshots as I did earlier. This is due to the feedback I received about screenshots. Applications evolve and appearances might change. So I'm better off describing the inputs in words than in pictures. Although a picture speaks a thousand words, it is probably not great for this context. 

### On the portal

If you want to configure your function with application insights on the portal, head to the [portal](https://portal.azure.com/), search for the function using the search input box, find your function and then look for `Application Insights` in the navigation pane and you wil be presented the option to `Turn on Application Insights`. 

If you clicked on the button, the remaining set of input required, will be similar whether you do it from the IDE or the portal.

### On the IDE

- Click on the configure button that you saw in the screenshot earlier that is next to the Application Insights icon in the Service Dependencies. 

- Choose your subscription from the list if you have many. I think by default, it selects the subscription associated to the function. 

- This step is dependent on whether you already have an AppInsights instance in the subscription. If you do, you may not want to add another. But if you don't, click on the button to add Application Insights. At the moment, it is something sort of a plus sign.

- Key in the name of the Application Insights instance, Subscription (again), a resource group (I'd just stick to the one that the function is associated to), Location (same choice as the function). The defaults loaded might actually be just the same as the function. 
  - AppInsight instance names, can be the same as the function. This tells those joining your team that you are only logging information from this particular function app for reasons that you only know. A common practice is to direct all logging from related applications, or a Bounded Context, that belongs to a certain environment (development, staging, etc.) to a shared instance of application insights. This helps in getting a birds-eye-view of the interaction between these different related apps in the `Application Maps` section.

- Once you have added an AppInsights instance, you can configure that application insights to be the target of the telemetry from the app. The IDE's wizard would present to you the ability to save the `APPINSIGHTS_CONNECTIONSTRING` into your Azure app settings. Accept it and proceed to confirm that you can close the wizard on successful configuration! The configure button will now be accomponied with a green success symbol - a check/tick symbol.

- You have now successfully created a new appinsights instance and you really want to see if it is working now. 

- Copy the URL of the function app from the publish window

- Paste it into our browser and add the following part to the url
  - `api/yourfunctionname?name=somename`
  - You could use a REST client like [Rest Client - Postman](https://www.postman.com/downloads/) and test it too

- Do keep that in mind. Your HTTP Function Url, isn't really the url that you copied from the Publish window or the portal. Every function app can contain more than one function. This is exactly why each of the functions within the same function app, will have a different url.

Are you ready to go then?
Well, in my case, I had already configured the storage account on the portal. So I could run the function right away hosted on Azure using the browser itself and it started logging to the application insights instance too.

Let us take a brief look at [Setting up Azure Storage Account and link it to your app](./SetupStorageAccounts.md)