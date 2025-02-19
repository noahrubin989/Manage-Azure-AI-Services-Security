# Manage Azure AI Services Security

Security is a critical consideration for any application, and as a developer you should ensure that access to resources such as Azure AI services is restricted to only those who require it.

Access to Azure AI services is typically controlled through authentication keys, which are generated when you initially create an Azure AI services resource.

## Revision Questions

1. You need to regenerate the primary subscription key for an Azure AI Services resource that an app uses. What should you do first to minimize service interruption for the app?

Switch the app to use the secondary key

2. You want to store the subscription keys for an Azure AI Services resource securely, so that authorised apps can retrieve them when needed. What kind of Azure resource should you provision?

Azure Key Vault

3. When running code on your computer that connects to Azure AI Services, you receive an error that access is denied due to Virtual Network/Firewall rules. What configuration do you need to set in the Azure AI Services instance?

In Networking properties, add your client IP address to the Firewall allowed list.

## Resources 
* [Manage Azure AI Services Security - Microsoft Learn](https://microsoftlearning.github.io/mslearn-ai-services/Instructions/Exercises/02-ai-services-security.html)
* [Get started with Azure Cloud Shell](https://learn.microsoft.com/en-us/azure/cloud-shell/get-started/classic?tabs=azurecli)



