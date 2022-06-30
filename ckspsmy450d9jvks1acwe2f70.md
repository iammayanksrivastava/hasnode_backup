## Using Bicep for Infrastructure as Code on Azure


Simplifying your Platform Engineering using the Azure Native language for Infrastructure deployment

It’s been a while I have written something over here. In fact, a lot has happened since my last post. I joined Royal FrieslandCampina in the month of June this year and I must say that I have been keeping really busy with the awesome things we plan to do with Data and Azure. I could not get any time to try out new things or do some hands-on work.

So what better topic than Infrastructure as a Code and within that talk about Azure Bicep. Let’s see what Bicep and few features which I liked about the language. Also, please feel free to [click here](https://github.com/iammayanksrivastava/azurebicep) to visit my GitHub page.

![Photo by Photo by Anastase Maragos on [Unsplash](https://cdn.hashnode.com/res/hashnode/image/upload/v1629792928434/5cJYyigat.html)](https://cdn-images-1.medium.com/max/12000/1*GCh21VhbF8a0xT6v7wR9hA.jpeg)*Photo by Photo by Anastase Maragos on [Unsplash](https://unsplash.com/photos/FP7cfYPPUKM)*

Please note that this is my first attempt to evaluate Azure Bicep to help me decide if I want to add this tool to my stack or not. I am still working on building the Infrastructure as a Code for my Azure Stack as per the best practices.

### So what is Bicep?

Azure Bicep is a Domain Specific Language. Yes, it is a Language, comparable to Terraform language that tells Terraform how to manage a given collection of Infrastructure. Bicep works as an abstraction layer on top of the ARM templates which simplifies the overall development experience with a very simple, clear syntax and you can build the code in line with the principles of modularity and reuse.

### Some nice things about Bicep

* One of the reasons why I always liked Terraform was because it was very easy to understand. Unlike the ARM, Bicep is super easy to understand and code. You can follow the [documentation](https://docs.microsoft.com/en-us/azure/templates/microsoft.storage/storageaccounts?tabs=json) which is very detailed and clear. Unlike the complication of JSON, Bicep is easy to write and follow. For instance, declaring parameters using bicep vs JSON

```
## Declaring parameters in bicep 
param location string = 'london'

#Declaring parameters in json

"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "london"
  }
}
```


* Bicep modules help in breaking down the complexity of your infrastructure code. A Bicep module is a set of one or more resources to be deployed together. Modules are also a way to make Bicep code even more reusable. You can have a single Bicep module that lots of Bicep templates use. You can centralize the Bicep modules as per the standards and multiple business units can use the same modules in their templates to deploy the components.

* The use of Parameters and Variables makes it very easy to make your Bicep templates or modules reusable and generic.

* Bicep templates integrate with Azure DevOps. You can easily write a YAML template and integrate your Bicep template as a task.

* One of the struggles which I always had with Terraform was the unavailability of new Azure features. Bicep helps you overcome this challenge. All the features whether in GA or Preview are available from Day 1.

* If you already have a setup using ARM templates, you can still decompile the ARM templates into Bicep templates and switch to Azure Bicep. I am yet to try these features but from what I read [here](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/decompile?tabs=azure-cli), it looks like it's an easy job.

Should we make Azure Bicep part of the stack? From an Architecture standpoint, I would suggest using Bicep as an Azure Native Infrastructure as a Code tool in the Technology stack.

### Installing Bicep

Installing Bicep is very easy. I would not spend much time explaining how to install Bicep. Please [follow](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/install) this link to understand the detailed steps to install the tool. In a nutshell, install VS Code and Bicep CLI to create templates. I would strongly suggest installing the Bicep extension in VSCode to ease your life while creating the templates. provides language support and resource autocompletion. The extension helps you create and validate Bicep files.

**Write a module for Storage Account**

![Module in Bicep for Storage Account](https://cdn.hashnode.com/res/hashnode/image/upload/v1629792930448/dt0e6Op3g.png)*Module in Bicep for Storage Account*

**Using the module in the Bicep Template**

![Bicep template which uses the module](https://cdn.hashnode.com/res/hashnode/image/upload/v1629792932618/S_n02xLoh.png)*Bicep template which uses the module*

Please note that you can either define the parameters inline within the template or create the parameters in a separate file and reference it within your code. For this example, I have written all the parameters inline within the template file.

**Integrating Bicep with Azure DevOps**

You can write a YAML file to deploy the Bicep template using Azure CLI. You can define the deployment as a task within the Yaml file and execute it from Azure Pipelines.

![YAML file to create a DevOps pipeline](https://cdn.hashnode.com/res/hashnode/image/upload/v1629792934806/oo5bjWqhQ.png)*YAML file to create a DevOps pipeline*

You can then execute the Bicep template using the Azure DevOps pipeline which will return the details of the resource deployed within the Azure Resource group of your subscription.

![Azure DevOps pipeline for Bicep](https://cdn.hashnode.com/res/hashnode/image/upload/v1629792936774/IQZ4S5HGS.png)*Azure DevOps pipeline for Bicep*

Please note that this was a very quick and dirty attempt by me to test whether Azure Bicep is a good fit for an Enterprise to be used as Infrastructure as a code. It took me a couple of hours to write a module and a template to create a Virtual Network, a subnet, and deploy a storage account within that Subnet and I managed to make the code pretty generic. I would love to hear what you think about Azure Bicep. Do you think you will use it in your stack? I am ready to do so..

**Reference**

1. Azure Bicep Documentation: [https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/)

1. GitHub Project Azure Bicep: [https://github.com/Azure/bicep](https://github.com/Azure/bicep)