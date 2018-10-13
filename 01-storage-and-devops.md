# Module 1: Azure Storage and DevOps
The completion of this module will result in you hosting an Angular application on Azure Blob Storage and deploying releases through Azure DevOps 

---

In this lab you will learn to:

* [Azure Storage](https://docs.microsoft.com/azure/storage/?WT.mc_id=workshop-github-jsteam)
    * Create a Storage Account using the Azure portal
    * Enable Static website hosting 
    * Deploy assets using the Storage extension in VS Code

* [Azure Devops](https://docs.microsoft.com/azure/devops/user-guide/index/?WT.mc_id=workshop-github-jsteam)
    * Create a new Devops project
    * Configure a new Build Pipeline for an Angular project
    * Configure a new Release Pipeline to deploy files to Azure Storage
    
To Learn more about Azure Storage checkout [Microsoft Learn path](https://docs.microsoft.com/en-us/learn/modules/store-app-data-with-azure-blob-storage/index/?WT.mc_id=workshop-github-jsteam )
    
## Azure Storage

1. In the browser, navigate to the Azure portal, https://portal.azure.com, and create a new Storage account. 
![Create Storage Account](https://tacofancy.blob.core.windows.net/tutorial/CreateStorageAccount.gif)

1. From the newly create Storage account Overview page, using the left hand side menu navigate to *Settings* -> *Static website* . Here, enable static website and add index.html in the *Index document name* input. 
![Enable Static Website](https://tacofancy.blob.core.windows.net/tutorial/EnableStaticWebsite.png)

1. In Visual Studio Code, go to the extensions panel and install the Azure Storage extension. Alternatively, you can install it from the [marketplace](https://marketplace.visualstudio.com/items/?WT.mc_id=workshop-github-jsteam&itemName=ms-azuretools.vscode-azurestorage)

1. **Fork** our app from https://github.com/Angular-Azure-Workshop/tacos-ui and switch to the *00-start* branch. Build for production by running `ng build --prod`

1. Once you have the extension installed and the tacos project forked and, go to Azure Panel, select storage and click on *Deploy to Static Website*. 
![Deploy to Static Website](https://tacofancy.blob.core.windows.net/tutorial/DeployStaticWebsite.gif)

## Azure Devops

1. In VS Code, go to the root of your project, create a file named *azure-pipelines.yml* and paste in the configuration bellow. ***Commit & push changes***
```yml
name: Tacos CICD

pool:
  vmImage: 'Ubuntu 16.04'

trigger:
  - master

steps:
  - task: NodeTool@0
    inputs:
      versionSpec: '8.x'
    displayName: 'Installing Node.js'

  - script: |
      npm install
      npm run build -- --prod
    displayName: 'Running npm install and build for prod'

  - task: CopyFiles@2
    inputs:
      sourceFolder: '$(System.DefaultWorkingDirectory)/dist/tacos-ui'
      contents: |
        **/*
      targetFolder: '$(Build.ArtifactStagingDirectory)'
      overWrite: true
    displayName: 'Copying built static files'

  - task: PublishBuildArtifacts@1
```
### Build Pipeline

1. In the browser, go to http://dev.azure.com and click Sign in to Azure DevOps, use your Microsoft account to sign in. On the landing page click *New project*, fill in the project name with tacos-ui and click *Create*

1. In the new project, got to *Pipelines* -> *Builds* and click *New Pipeline*. Select Github as source, Authorize Gitub and choose the project you forked earlier, branch *00-start*. Continue and configure the build to use *Configuration as code* and select the yml file you created earlier. 

1. Save and queue. Click on the build link to view the in progress project
![Build Pipeline](https://tacofancy.blob.core.windows.net/tutorial/Build_Pipeline.png)

### Release Pipeline

1. Go to *Pipelines* -> *Releases* , Start with empty project 

1. In the *Artifacts* tile click add and select *Build* source type. From there select the project & build pipeline you created earlier 

1. In the *Stages* tile, click *1 job*, *Add Task* and select *Azure File Copy* from the list . 
    * Change version to *2 Preview* 
    * Set source to $(System.DefaultWorkingDirectory)/<your_proj_name_here_>/drop
    * Choose Azure Subscription and ***click Authorize***
    * Select Azure Blob as *Destination Type*
    * Enter the name of the Storage Account you created earlier in the *RM Storage Account* input
    * Set *$web* for *Container Name*

1. Go back to *Builds* and Queue a new build
