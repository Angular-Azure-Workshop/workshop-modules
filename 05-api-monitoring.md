# Module 5: Application Insights in Azure Functions
The completion of this module will result in you being able to monitor aspects of your Azure Functions app using Application Insights
---

## Monitor Serverless Endpoint

1. Install applications insight dependency:
```
npm i applicationinsights
```
2. Retrieve Application Insight Instrumentation key from the Overview page and add it to local.settings.json
```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "<function app storage connection string>",
    "FUNCTIONS_WORKER_RUNTIME": "node",
    "user": "<database user>",
    "password": "<database primary password>",
    "port": "10255"
    "APPINSIGHTS_INSTRUMENTATIONKEY":"<app insights key>"
  },
  "Host": {
    "CORS": "*"
  }
}
```
3. In index.js add code to import the package and initialize it:
```javascript
const appInsights = require('applicationinsights');
appInsights.setup();
const context = appInsights.defaultClient.context;
context.tags[context.keys.cloudRole] = 'backend';
appInsights.start();
```
4. Make a couple of requests from the UI and then navigate to Application Insights and see those requests being monitored
5. Throw an exception to see it in the analytics

### Demo App Completed
// TODO: add proper branch link in tacos-api repo
For reference, here is the demo app after completing everything from this module: [Demo with Application Insights in Azure Functions]()
