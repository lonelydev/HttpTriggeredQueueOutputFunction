# Azure Functions


## What is an azure function?

As the name suggests, it is a function that runs on the cloud. But then you ask, all my code contains functions, and they can all be deployed on azure and so would they all be called azure functions?

Excellent question. 

Well, I oversimplified, when I said that it is just a function that runs on the cloud. 

  * Azure functions is a service offered by Microsoft on its cloud infrastructure, where by you can focus on just writing your application code and not worry about the maintenance of the hardware that runs it. 
  * Functions on Azure can be charged on a pay as you go basis and hence you will only get charged according to the usage of the function. 
  * You also have the ability to scale out automatically based on your request load. 

The idea behind this is generally known as [Serverless Computing](https://en.wikipedia.org/wiki/Serverless_computing)

## Creating your first Azure function

This might sound like a lot of work. But it is a lot easier than you think. You can do this in many ways. 
I'll see if I can explain all the ways. 


### Using Visual Studio

You really don't need to download the professional or enterprise edition of Visual Studio to achieve this. All you need is the community edition of Visual Studio with the azure development facility installed. 
You can download the latest [Visual Studio](https://visualstudio.microsoft.com/downloads/).

If you already have visual studio installed, then open visual studio installer and check if you have installed the Azure development tools. 
![vsinstallerazuredevelopment.png](images/vsinstallerazuredevelopment.png)

If the Azure development checkbox is checked already, then you don't need anything more. Else, just install it. 

#### Installed all the pre-requisites, now what?

First of all, open visual studio. 

![azcreatenewproject.png](images/azcreatenewproject.png)
Click on the New project option. 

Search for Azure Function template and click next.
![vsnewprojectazurefunction.png](images/vsnewprojectazurefunction.png)

Give your project a name and click create
![vsprojectname.png](images/vsprojectname.png)

This should present you with a function creation wizard that gives you plenty of options. 

We will take a look at these options in detail soon. 

![vscreatenewfunctionwizard.png](images/vscreatenewfunctionwizard.png)
