# Module 2: Application Insights in Angular
The completion of this module will result in you being able to monitor your Angular application using Application Insights in Azure.

---

In this Lab you will learn to:

* [Azure Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview/?WT.mc_id=workshop-github-jsteam)
    * Create an instance of Application Insights
    * Use SDK to track events in your Angular application 
    
Learn more about Designing for efficiency and operations in this [Microsoft Learn module](https://docs.microsoft.com/en-us/learn/modules/design-for-efficiency-and-operations-in-azure/index/?WT.mc_id=workshop-github-jsteam)

### Demo App Starting Point
Here is the demo application in a state that's at a starting point to follow along with the steps in this module [Demo App Module 2 Start](https://github.com/Angular-Azure-Workshop/tacos-ui/tree/module02-start)

## Azure Applications Insights

1. In the browser, navigate to the Azure portal, https://portal.azure.com, and create a new Application Insights account 
![Create Azure Insights Account](https://tacofancy.blob.core.windows.net/tutorial/AppInsights.gif)
    
2. In your Angular project install *applicationinsights-js* npm package

```
npm i applicationinsights-js
```

3. In *app.component.ts* import and configure AppInsights. Retrieve instrumentation key from the Azure portal, the App Insights instance you created earlier, Overview page. Call downloadAndSetup method in the ngOnInit method

```javascript
/* import AppInsights */
import {AppInsights} from "applicationinsights-js"

/* Call downloadAndSetup to download full ApplicationInsights script from CDN and initialize it with instrumentation key */
AppInsights.downloadAndSetup({ instrumentationKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx" });
```

4. In *app.component.ts* configure AppInsights to track page views on route changed: 

```javascript
this.router.events.subscribe((event: Event) => {
      if (event instanceof NavigationEnd) {
        if (event.url === '/') {
          AppInsights.trackPageView('home');
        } else {
          AppInsights.trackPageView(event.url.substring(1));
        }
      }
    });
  }
```

5. Commit and push changes to github. Run app and navigate to different routes a couple of times. Then go back to the AppInsights overview page and navigate to *Application Map* to monitor details about page views
![Page Views](https://tacofancy.blob.core.windows.net/tutorial/PageViews.png)

6. Trigger a failure by changing the URL in *app.service.ts* to an invalid value. In the browser, got to http://localhost:4201/#/tacos and notice 404 error in the console. Now in the Azure portal go to the AppInsights instance and select Browser page. Choose Browser Failures and notice details about the 404 error
![Browser Failures](https://tacofancy.blob.core.windows.net/tutorial/BrowserFailures.png)

7. Continue experimenting by triggering exceptions in the Angular code and inspect them in the Browser failures tab. 


### Demo App Completed
For reference, here is the demo app after completing everything from this module: [Demo with Application Insights](https://github.com/Angular-Azure-Workshop/tacos-ui/tree/module02-finished)
