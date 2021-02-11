# Create an Azure Key Vault

* Go to the Azure portal, search for Key Vault in the search box and select it. 
* In the Page that follows click on Add - to add a new key vault
* You will be presented with a form that requires you to go through the entire series of steps that you would go through for any cloud resource creation

  - Choose subscription
  - Choose resource group
  - Pick key vault name
  - pick region
  - pick a pricing tier

Upto that point everything is simple, but then you get asked some questions about Recovery. You can read more about these on [Microsoft Docs for KeyVaults](https://docs.microsoft.com/en-gb/azure/key-vault/general/soft-delete-overview?WT.mc_id=Portal-Microsoft_Azure_KeyVault#soft-delete-behavior)

- soft delete - when enabled a keyvault marked as deleted are retained for a specific number of days - 90 by default
- days to retain - can override the default here
- purge protection - this is extra protection for the soft deleted items to be exempted from purge until the retention period has passed.

Moving on. Without making any further choices, just press `Review and Create` and create the keyvault already. 

## Add Secrets to the vault

The whole point of the Keyvault is to let us store some secrets. Let us keep our application settings like AppInsights instrumentation key in the keyvault as an example and our storage account keys in there. 

## Configure Access Policy on Function

Go to your Azure Function and find the Identity button under Settings in the left hand side navigation bar. Turn on System Assigned Managed Identity for the function app. This is important stuff. Save and Confirm the changes and you should be displayed a little info banner on that page that says something about "The resource is registered with Azure Active Directory...". Good stuff! You did it. 

## Configure Keyvault to be accessible by the function app

You can make keyvaults accessible from you function app through Role based application access or Access Policies. For simplicity we could try out the Access Policy method. The two methods of access are incompatible with one another. So do check with your platform engineering team on what they prefer. I would think, that for large organisations the RBAC method would be better. And in fact, this is the recommended way forward. 

Go back to your key vault. Find the `Access policies` button and then add an access policy for your function. 

In the pane that comes up, instead of choosing to `Configure from template (optional)`, be adventurous and follow the instructions.

* Select Get and List permissions for the Key, Secret and Certificates.
* Select the principal by pasting either the Object id of the Azure Function app or just type the name of the Azure function app in the search box and you should be able to select it.  
* Leave the Authorized Application unselected.
* Click Add or whatever final confirmation button you see on screen

You have just given an application a managed identity access to this newly created keyvault!

## Make keyvault secret accessible by function configuration

You can do this in many ways. One way is to use the URI to the secret and set that up as a value to an application setting of the Function App in the portal. 

Go to the Function App in the portal. Head to the Configuration section and add a new application setting. You may name it whatever you want. 
In this case, you could call it, `KeyVaultAppInsightInstKey` and then in the value, enter `@Microsoft.KeyVault(SecretUri=https://my.secret.uri.is/so/long/withsomething/here)`

If you had configured the access policies correctly and you click save then in the source column of the Application Settings table, you would see a green checkmark and `Key vault reference` written against the newly added setting. 

## Time to test if you can use this in the app

Go to your Function source code. Add the following lines whereever you can get it to run.

```csharp
    var instKey = Environment.GetEnvironmentVariable("AppInsightsInstrumentationKeyFromVault");
    log.LogInformation("Got the instrumentation key", instKey);
```

Run it now! This will not work locally! Great stuff right!

### Why would it not work locally?

The syntax `@Microsoft.KeyVault...` is something that the Azure portal understands. But there is no local Visual Studio technique that will make it possible for your locally running app to fetch the value of the actual secret. Solution, follow the best practices.

## How to make use of keyvault and also make it run and work locally

The best practice is to always refer to the KeyVault URI and in code during startup retrieve whatever values you want using that.

You can use [dependency injection in .NET Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-dotnet-dependency-injection). 

```csharp
[assembly: FunctionsStartup(typeof(MyFunctionStartupFileNamespace.Startup))]
namespace MyFunctionStartupFileNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            /**
             some configuration code here
            */
            var services = builder.Services; //that's your service collection right there
            // ... all dependency injection here
        }

        public override void ConfigureAppConfiguration(IFunctionsConfigurationBuilder builder)
        {
            builder.ConfigurationBuilder.AddAzureKeyVault(Environment.GetEnvironmentVariable("KeyVaultUri"));
        }
    }
}
```

This way, the `IConfiguration` instance would now have `AzureKeyVaultConfigurationProvider` and would be accessible anywhere in the application.

Read more about [Customizing Configuration Sources in Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-functions/functions-dotnet-dependency-injection#customizing-configuration-sources).
