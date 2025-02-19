# Manage Azure AI Services Security

## High Level Methodology

### Provision an Azure AI Services resource

First type **Azure AI services multi-service account**. You will then be navigated to the screen below, where you must then click **Create Azure AI services multi-service account**
<img width="1412" alt="Screenshot 2025-02-19 at 3 36 54 PM" src="https://github.com/user-attachments/assets/ec8c95d7-dd40-4a59-a0be-77d7ac885cfb" />

From here, configure your resource by placing it withion the subscription and resource group of your choice, configuring it with a name, region and pricing tier. Once done, press **Review + create**
<img width="738" alt="Screenshot 2025-02-19 at 3 40 53 PM" src="https://github.com/user-attachments/assets/f0dfffbe-6df1-4bfe-a739-00ca030eb8e3" />

Review the configuration, then press **Create**. this will start the resource deployment process
<img width="725" alt="Screenshot 2025-02-19 at 3 43 07 PM" src="https://github.com/user-attachments/assets/57d0c9f3-219b-41af-a7ec-34cd0508e900" />


### Manage authentication keys

Locate the newly deployed resource, then navigate to the **Keys and Endpoint** section, where you can find the required keys (**Key 1** and **Key 2**), **Location/Region** and **Endpoint**
<img width="735" alt="Screenshot 2025-02-19 at 3 45 22 PM" src="https://github.com/user-attachments/assets/f253580b-354e-4308-b770-7fcd4cf5e472" />

There is also a way of doing this from the command line, using:
```bash
 az cognitiveservices account keys list --name <resourceName> --resource-group <resourceGroup>
```

If a key becomes compromised, or the developers who have it no longer require access, you can regenerate it in the portal but can also do so using the Azure CLI:

```bash
 az cognitiveservices account keys regenerate --name <resourceName> --resource-group <resourceGroup> --key-name key1
```

### Secure key access with Azure Key Vault
Keys can be stored in `.env` files, though this approach is not as secure as using **Azure Key Vault**. To do this we first need to create a key vault and add a secret

#### Create a key vault and add a secret

First, navigate to **Key vaults**
<img width="744" alt="Screenshot 2025-02-19 at 4 00 43 PM" src="https://github.com/user-attachments/assets/b3d9813b-4dee-4de4-949f-9fc3c195de93" />

Then select **Create key vault**
<img width="743" alt="Screenshot 2025-02-19 at 4 02 04 PM" src="https://github.com/user-attachments/assets/6ea8f5f2-df1b-4806-818a-1a07992c8aa4" />

On the **Basics** page, configure the basics such as the **Key vault name**, etc.
<img width="399" alt="Screenshot 2025-02-19 at 4 05 03 PM" src="https://github.com/user-attachments/assets/34c41914-44ba-4ec7-a973-18f0d81692ad" />

On the **Access configuration** page, select the **Permission model** as **Vault access policy** (A Key Vault access policy determines whether a given security principal, namely a user, application or user group, can perform different operations on keys, secrets and certificates) and check the newly displayed checkbox. once done, press **Review + create**. Wait for the deployment to start and finish.
<img width="736" alt="Screenshot 2025-02-19 at 4 08 52 PM" src="https://github.com/user-attachments/assets/adeec559-83e1-42af-b3b9-8af25341fd1d" />

In the newly deployed **Key vault** resource, navigate to **Secrets**
<img width="738" alt="Screenshot 2025-02-19 at 4 11 55 PM" src="https://github.com/user-attachments/assets/82720d0e-2b63-4737-adaf-2bde1cf137fd" />

Once done, select **+ Generate/Import** and configure it in the following way. Here my **Secret value** has been set to the **Key 1** value from the previously created AI Services resource. Once done press **Createw**
<img width="604" alt="Screenshot 2025-02-19 at 4 13 38 PM" src="https://github.com/user-attachments/assets/f15e7b5c-8700-41f8-90ac-2a2dd2e5dba3" />

We now need to access the secret in the key vault, and to do so must use a service principal that has access to the secret

#### Create a service principal
```bash
 az ad sp create-for-rbac -n "api://<spName>" --role owner --scopes subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>
```

In this case:
<img width="1163" alt="Screenshot 2025-02-19 at 4 30 00 PM" src="https://github.com/user-attachments/assets/da7c1223-a668-4329-8f80-5c3f9a4b4bbd" />

The output of this command takes the following form:
```
{
    "appId": "abcd12345efghi67890jklmn",
    "displayName": "api://ai-app-",
    "password": "1a2b3c4d5e6f7g8h9i0j",
    "tenant": "1234abcd5678fghi90jklm"
}
```

Now we need the **object ID** of the service principal. For this we run:
```
az ad sp show --id <appId>
```





## Resources 
* [Manage Azure AI Services Security - Microsoft Learn](https://microsoftlearning.github.io/mslearn-ai-services/Instructions/Exercises/02-ai-services-security.html)
* [Get started with Azure Cloud Shell](https://learn.microsoft.com/en-us/azure/cloud-shell/get-started/classic?tabs=azurecli)



