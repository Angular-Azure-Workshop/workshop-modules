# Module 4: Azure CosmosDB 
The completion of this module will result in you connecting your Azure Function to a CosmosDB instance running MongoDB and reading the data from there.
---

## Create an Azure Cosmos DB Account
1. In the portal, create a new Azure Cosmos DB Account. Select MongoDB or SQL API for your account. Create a new database named admin and a collection names Recipes
1. In VS Code, go to the extensions panel and install CosmosDB extension. Alternatively, go to the [marketplace]( https://marketplace.visualstudio.com/items/?WT.mc_id=workshop-github-js-team&itemName=ms-azuretools.vscode-cosmosdb)  
1. Sign in and explore the account you have just created. Update the Recipes collection to include the two documents from https://tacofancy.blob.core.windows.net/data/data.json 

## Read data from Azure Functions
1. Install mongoose npm package
```
npm i mongoose
```
2. Create new folder called *shared* to place code that's going to be shared in the future:
    * mongo.js
```javascript
const mongoose = require('mongoose');
/**
 * Set to Node.js native promises
 * Per http://mongoosejs.com/docs/promises.html
 */
mongoose.Promise = global.Promise;

const auth = {
  user: process.env.user,
  password: process.env.password
};

const mongoUri = `mongodb://${process.env.user}.documents.azure.com:${
  process.env.port
}/?ssl=true`;

let client = null;
function connect() {
  mongoose.set('debug', true);
  return new Promise((resolve, reject) => {
    if (client == null) {
      mongoose
        .connect(
          mongoUri,
          { auth: auth }
        )
        .then(_client => {
          client = _client;
          resolve(_client);
        })
        .catch(err => reject(err.status));
    } else {
      resolve(client);
    }
  });
}

module.exports = {
  connect,
  mongoose
};
```
    * recipe.model.js
```javascript
const mongoose = require('mongoose');
const RecipeSchema = new mongoose.Schema(
  {
    category: {
      type: String,
      enum: [
        'SHELLS',
        'BASE_LAYERS',
        'MIXINS',
        'SEASONINGS',
        'CONDIMENTS',
        'FULL_TACOS'
      ]
    },
    title: String,
    description: String,
    ingredients: [String],
    steps: [String],
    tags: [String]
  },
  {
    collection: 'Recipes',
    read: 'nearest'
  }
);
const Recipe = mongoose.model('Recipe', RecipeSchema);
module.exports = Recipe;
```
    * recipe.service.js
```javascript
const Recipe = require('./recipe.model');
const ReadPreference = require('mongodb').ReadPreference;
require('./mongo').connect();

function getRecipes() {
  var countQuery = Recipe.count();
  const docQuery = Recipe.find({}).read(ReadPreference.NEAREST);
  return new Promise((resolve, reject) => {
    docQuery
      .exec()
      .then(recipes => {
        if (recipes.length === 0)
          resolve('There are no results matching your query.');
        else {
          countQuery.exec().then(count => {
            resolve({ items: recipes, totalItems: count });
          });
        }
      })
      .catch(err => {
        reject(err.status);
      });
  });
}

module.exports = {
  getRecipes
};
```
3. In index.js import *recipe.service* and make request to read data: 
```javascript
const recipes = require('../shared/recipe.service');
module.exports = async function(context, req) {
  context.log('JavaScript HTTP trigger function processed a request.');
  await recipes
    .getRecipes()
    .then(data => {
      context.res = {
        body: data
      };
    })
    .catch(err => {
      context.res = {
        status: 500,
        body: err
      };
    });
};
```
4. Update local.setting.json to include your database config:
```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "<function app storage connection string>",
    "FUNCTIONS_WORKER_RUNTIME": "node",
    "user": "<database user>",
    "password": "<database primary password>",
    "port": "10255"
  },
  "Host": {
    "CORS": "*"
  }
}
```
5. Run and check that UI still works

### Demo App Completed
// TODO: add proper branch link in tacos-api repo
For reference, here is the demo app after completing everything from this module: [Demo with Azure CosmosDB]()
