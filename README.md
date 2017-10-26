# Cognitive Services Pipeline

Sample pipeline feeding [Azure Cognitive Services](https://azure.microsoft.com/en-gb/services/cognitive-services/) computer vision API images from a queue and storing the analysis in a database, which is read and visualised from a web app.

## Requires

* [Azure subscription](https://azure.microsoft.com/en-us/) (for hosting and cognitive services)
* [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) (for deploying)
* [Node.js](https://nodejs.org/en/) (language used in function/web app)

## Deployment

### Provising the infrastructure in Azure

In Azure CLI:

```
cd CognitiveServicesPipeline
az login
az group create --name CognitiveServicesPipeline --location "South Central US"
az group deployment create -g CognitiveServicesPipeline --template-file azuredeploy.json --parameters @azuredeploy.parameters.json 
# escape @ in powershell with "`"
```

NOTE: You may have to update `azuredeploy.parameters.json` to change the `databaseAccountName` and `webAppName` as they create URLs and need to be unique in Azure. 

To delete resources:

```
az group delete --name CognitiveServicesPipeline
```

### Deploying the applications

To deploy the web app (from repo [CognitiveServicesPipelineWebApp](https://github.com/stevenalexander/CognitiveServicesPipelineWebApp)):

```
az webapp deployment source config --name cognitiveservicespipeline --resource-group CognitiveServicesPipeline --repo-url https://github.com/stevenalexander/CognitiveServicesPipelineWebApp.git --branch master --manual-integration
# sync for updates
az webapp deployment source sync --name cognitiveservicespipeline --resource-group CognitiveServicesPipeline
```

## Notes

* Deleting the resource group can take >2 mins due to deleting the Cosmo account
* Powershell can also be used for deployment, but publishing applications is fiddly and poorly documented for node