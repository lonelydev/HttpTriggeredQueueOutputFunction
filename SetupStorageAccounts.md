# Storage Account

You might be wondering "why though?".

I am not going to explain in-detail about what an azure storage is as part of this solution. It is large topic that has to be covered on its own.

## What is Azure Storage?

However, to put it simply, Azure Storage platform is a cloud storage solution provided by Microsoft on Azure. The service offers:

- Massively scalable object store for data objects
- Disk storage for virtual machines
- A file system service for the cloud
- A messaging store for reliable messaging
- NoSQL store

More details and what is guaranteed by using this service is provided in the [Azure Storage Services docs](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction?toc=/azure/storage/blobs/toc.json)

## What is a storage account?

An Azure Storage account contains all your storage data objects:

- Blobs
- Queues
- Files
- Tables
- Disks

The Account provides a unique namespace for your storage data that's accessible from anywhere using HTTP or HTTPS. 

## Why Do I need a storage for an Azure Function?

When creating an azure function app, you must create or link it to a general purpose Storage account that supports Blob, Queue or Table storage. Functions use Azure Storage for operations such as managing triggers and logging function executions. Ensure that the storage account you choose, supports those data structures. Some storage accounts do not support queues and tables. They maybe blob-only accounts or Azure Premium Storage and general purpose accounts with [ZRS (zone redundant storage) replication](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy#redundancy-in-the-primary-region).

## How do I do this?

I actually found that the Microsoft docs do a better job at explaining this than me. So please follow the instructions in [Azure Docs to create a storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1)

## I have a storage account, now what?

Well, now that you have created one, you have link that to your function. You can do that just like you did the Application Insights instance.

In the publish window, click next on the Azure Storage configure button and find your newly created azure storage in the right subscription and resource group and then click ok! That is all there is to it. You have now successfully created a function app and configured Application Insights and also a storage account.

Let us look at how to deploy this next. I mean, we have been publishing this using the Publish tab in Visual studio but it is always better to have a continuous integration setup in Azure or Github actions or something that would automatically trigger a build and then deploy your code whenever you make some code changes. 
