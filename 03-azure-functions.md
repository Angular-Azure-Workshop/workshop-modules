
# Module 3: Azure Functions (Serverless)
The completion of this module will result in you creating an Azure Function that reads data from blob storage. You'll be able to connect your Angular application to use this API as well.

## Create an Azure Function app

1. In the Azure portal, create a new function app. Use the Storage account you created earlier.
1. Once the app is created, open its resource group and open the Storage account inside. Locate its connection string and copy it. We'll be using this storage account for local debugging and to read data for our recipe list.

## VS Code 

1. Install VS Code Azure Functions extension from extensions panel or from the [marketplace]( https://marketplace.visualstudio.com/items/?WT.mc_id=workshop-github-jsteam&itemName=ms-azuretools.vscode-azurefunctions)
2. Go to the Azure panel and from the Azure Functions extension create a new function app and a new function of type HttpTrigger. Name it GetRecipes. 
3. Run and debug locally 

## Read data from Blob Storage

1. In your storage account create a new blob container named data and upload a file names data.json with the contents of https://tacofancy.blob.core.windows.net/data/data.json 
2. Update function.json to include a new blob input binding. Read mode about it in [docs](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-blob/?WT.mc_id=workshop-github-js-team) 

```json
{
    "name": "data",
    "type": "blob",
    "direction": "in",
    "connection": "AzureWebJobsStorage",
    "path": "data/data.json"
}
```
3. Update local.settings.json with Connection String for your storage account:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "<function app storage connection string>",
    "FUNCTIONS_WORKER_RUNTIME": "node"
  }
}
```
4. Update index.js to read data from binding
```javascript
module.exports = async function(context, req) {
  context.log('JavaScript HTTP trigger function processed a request.');
  context.res = {
    body: context.bindings.data
  };
};
```

## Connect your Angular app to read data from the function 

1. Update *tacos.service.ts* to use function url, i.e. http://localhost:7071/api/GetRecipes 
2. Update local.settings.json to include CORS settings:
```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "<function app storage connection string>",
    "FUNCTIONS_WORKER_RUNTIME": "node"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*"
  }
}
```
3. Restart function app - runtime doesn't listen to changes to local.settings.json file.

### Demo App Completed
// TODO: add proper branch link in tacos-api repo
For reference, here is the demo app after completing everything from this module: [Demo with Azure Functions]()
