# Module 6: Authentication in Azure

The completion of this module will result in you being able to login/logout with a microsoft account through the Angular application using the Authentication/Authorization module in Azure Functions.

---

In this lab you will learn to:

* [Azure Authentication/Authorization](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings?WT.mc_id=workshop-github-jsteam#auth)
    - Enable and Configure Authentication/Authorization in Azure Functions
    - [Register your application with Microsoft Accounts](https://docs.microsoft.com/en-us/azure/app-service/app-service-mobile-how-to-configure-microsoft-authentication?WT.mc_id=workshop-github-jsteam)
    - Configure CORS and Allowed Redirects in Azure Functions so our Angular application can connect to it.
    - Add environment values to our Angular application to connect to the Azure Function
    
#### Other Docs for Reference:
1. [Authentication and Authorization in Azure App Service (applies the same to Azure Functions)](https://docs.microsoft.com/en-us/azure/app-service/app-service-authentication-overview?WT.mc_id=workshop-github-jsteam)
2. [Sign out of a session](https://docs.microsoft.com/en-us/azure/app-service/app-service-authentication-how-to?WT.mc_id=workshop-github-jsteam#sign-out-of-a-session)
    
## Azure Functions Authentication/Authorization
1. Navigate to the [Azure Portal](https://portal.azure.com/?WT.mc_id=workshop-github-jsteam)
2. On the Azure Portal, navigate to your Azure Functions App resource
3. On the Azure Functions **Overview** tab, locate the Azure Function URL
    > We will use this URL later
    
    ![image](https://user-images.githubusercontent.com/6265396/46899925-13dd7000-ce68-11e8-8399-3e77c2f83f11.png)

4. On the Azure Functions App **Platform features** page, towards the middle of the menu options, select **Authentication / Authorization**

    ![image](https://user-images.githubusercontent.com/6265396/46899936-4f783a00-ce68-11e8-9c40-40df3b72e5c7.png)
    
5. On the **Authentication / Authorization** page, make the following selections:
    - **App Service Authentication**: On
    - **Action to take when request is not authenticated**: Log in with Microsoft Account
6. On the **Authentication / Authorization** page, select **Save**

![Microsoft Auth Settings](https://user-images.githubusercontent.com/13558917/46318829-c595ba80-c5a5-11e8-9503-f389ce0ffbc0.png)

7. On the **Authentication / Authorization** page, select **Microsoft Not Configured**
8. On the **Microsoft Account Authentication Settings** page, click **These settings allow users to sign in with Microsoft Account. Click here to learn more.**

![Microsoft App Setup](https://user-images.githubusercontent.com/13558917/46318735-85ced300-c5a5-11e8-9f5b-dd365f0b5ee3.png)

9. On the **How to configure your App Service application to use Microsoft Account login** page, click **My Applications**

![My Application](https://user-images.githubusercontent.com/13558917/46318734-85ced300-c5a5-11e8-9512-f105e11e71a5.png)

10. On the **Application Registration Portal**, login with your Microsoft username/password

![App Registration Portal Login](https://user-images.githubusercontent.com/13558917/46318743-86676980-c5a5-11e8-8843-e6d492dda7a3.png)

11. On the **My applications** page, select **Add an app**

![Add an app](https://user-images.githubusercontent.com/13558917/46318742-86676980-c5a5-11e8-8086-e08f0033d377.png)

12. On the **Register applications** page, create a name for your app
13. On the **Register applications** page, select **Create**

![Register you application](https://user-images.githubusercontent.com/13558917/46318741-86676980-c5a5-11e8-9963-408b8aeaebd8.png)

14. On the **App Registration** page, select **Generate New Password**

![Generate new password](https://user-images.githubusercontent.com/13558917/46318740-86676980-c5a5-11e8-9132-3c6f6430c076.png)

15. On the **New password generated** popup, copy the password and paste it in a text file on your local computer
    > We will use this password later, but you will not be able to access the password after clicking **Ok** and closing the popup. Be sure to save this somewhere safe temporarily
16. On the **New password generated** popup, select **Ok**

![App Password](https://user-images.githubusercontent.com/13558917/46318931-2ae9ab80-c5a6-11e8-88c7-2c0bbabf8718.png)

17. On the main application page, locate the **Application Id** and copy and save it somewhere temporarily
    > **Note:** We will use this Application Id later
18. Click **Add platform** and choose **Web** when prompted
19. Make sure **Allow Implicit Flow** is checked and enter the callback URL as an entry under **Redirect URLs** using the format below:
    - **Formatted Azure Function Url:** `[Your Azure Function Url]/.auth/login/microsoftaccount/callback`
    - E.g., https://tacofancy-api.azurewebsites.net/.auth/login/microsoftaccount/callback
19. Add a **Logout URL** using the format below
    - **Formatted Azure Function Url:** `[Your Azure Function Url]/.auth/logout
    - E.g., https://tacofancy-api.azurewebsites.net/.auth/logout
20. Scroll to the bottom and click **Save**

![image](https://user-images.githubusercontent.com/6265396/46900114-f4941200-ce6a-11e8-8405-fbe4f1474874.png)

21. In the **Azure Portal** on the **Microsoft Account Authentication Settings** page, enter the following values:
    - **Client Id:** [Your Microsoft Application Id]
    - **Client Secret:** [Your Microsoft Application Password]
22. On the **Microsoft Account Authentication Settings** page, select **Ok**

![Microsoft Auth Settings](https://user-images.githubusercontent.com/13558917/46318737-85ced300-c5a5-11e8-9095-44b1c0f226a8.png)

21. On the **Authentication / Authorization** page, select **Save**

![Save Auth](https://user-images.githubusercontent.com/13558917/46318736-85ced300-c5a5-11e8-9c36-42bf0ff6278e.png)

## Enable CORS and Login Redirects

1. 

### Demo App Completed
// TODO: add proper branch link in tacos-ui repo
For reference, here is the demo app after completing everything from this module: [Demo with Azure Authentication]()
