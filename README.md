# github-actions-demo

## Prerequisites
- An Azure account with an active subscription.
- Visual Studio Code
- Azure CLI
- .NET 8.0 SDK

## Setup Azure Environment
1. In your terminal, run:
    ```shell
    az login
    ```
   Copy the value of the id: field to a safe place. We'll call this **AZURE_SUBSCRIPTION_ID**. Example:
    ```shell
    [
        {
        "environmentName": "AzureCloud",
        "homeTenantId": "ed9*****-****-4016-****-b***********0",
        "id": "d57*****-****-4ec3-****-ac8d84d029e2", # <-- Copy this id field
        "isDefault": true,
        "name": "ASD Lab",
        "state": "Enabled",
        "tenantId": "ed9*****-****-4016-****-b***********0",
        "user": {
            "name": "****.*****@******.com",
            "type": "user"
        }
        }
    ]
    ```
1. In your terminal, run the command below to create the resource group. You can also use the azure portal to create the resource.
    ```shell
    az group create --location $LOCATION --name $RESOURCE_GROUP --subscription $AZURE_SUBSCRIPTION_ID
    ```
1. In your terminal, run the command below to create the Azure App Service Plan. You can also use the azure portal to create the resource.
    ```shell
    az appservice plan create --resource-group $RESOURCE_GROUP --name $AZURE_APP_PLAN --is-linux --sku F1 --subscription $AZURE_SUBSCRIPTION_ID
    ```
1. In your terminal, run the command below to create the Azure Web App. You can also use the azure portal to create the resource.
    ```shell
    az webapp create --resource-group $RESOURCE_GROUP --plan $AZURE_APP_PLAN --name $AZURE_WEBAPP_NAME -r "DOTNETCORE:8.0" --subscription $AZURE_SUBSCRIPTION_ID
    ```
1. In your terminal, run the command below to create the Service Principal for deploying the app to the Azure App.
Service. You can also use the azure portal to create the resource.
    ```shell
    az ad sp create-for-rbac --name "github-demo" --role contributor --scopes "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/$RESOURCE_GROUP/providers/Microsoft.Web/sites/$AZURE_WEBAPP_NAME"
    ```
    Copy the entire contents of the command's response, we'll call this **AZURE_CREDENTIALS**. Example:
    ```shell
    {
      "clientId": "<GUID>",
      "clientSecret": "<GUID>",
      "subscriptionId": "<GUID>",
      "tenantId": "<GUID>",
      (...)
    }
    ```
## Setup GitHub
1.  Go to GitHub, click on this repository's **Secrets and variables > Actions** in the Settings tab.
1.  Click **New repository secret**.
1.  Name the secret **AZURE_CREDENTIALS** and paste the entire contents from the terminal command you entered previously.
1.  Click **Add secret**
