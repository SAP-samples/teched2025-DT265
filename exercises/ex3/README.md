# Exercise 3 - Build and Deploy a Customer-Specific Extension to Manage the Caterer Entity

In this exercise, you create a tenant-specific extension called **Caterer Management**. This extension lets you manage caterers.

> Note: The SAP BTP Cloud Foundry runtime environment is required in the subaccount of your customer, as the web application will be deployed to manage caterer information. In this demo, it is already enabled. For more details beyond this exercise, refer to the [Partner Reference Application tutorial - Enable SAP BTP Cloud Foundry Runtime](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/22-Multi-Tenancy-Prepare-Deployment.md#enable-sap-btp-cloud-foundry-runtime).

## Exercise 3.1 - Develop a SAP Fiori UI to Manage the Caterer Entity

After completing these steps, you have created a SAP Fiori elements app to manage caterers.

### Get Poetry Slam Manager EDMX

You consume the metadata of the **Poetry Slams** OData service from the customer subaccount where the back end is independently managed. This metadata is then utilized to create the SAP Fiori elements project. To obtain the metadata, follow the steps outlined below:

1. Create a folder,for example `partner-reference-extension-catering-ui` in your workspace in SAP Business Application Studio. The path should be **/home/user/projects-folder/partner-reference-extension-catering-ui**.
   
   1. Open a new terminal in SAP Business Application Studio (Strg + Shift + C).
   2. The terminal should show the **partner-reference-extension-catering** folder. Navigate to **projects** folder by using the command:
      ```
      cd ..
      ```  
   3. Run the following command to create a new folder:
      ```
      mkdir partner-reference-extension-catering-ui
      ```  
   4. Navigate to the new folder in the terminal by using the command: 
      ```
      cd partner-reference-extension-catering-ui
      ```  
   5. Add the folder to your workspace. Choose the sandwich button on the top left corner and choose **File -> Add Folder to Workspace...**.
   6. Select the newly created folder called **partner-reference-extension-catering-ui**.
   
   Your project structure should look like this:
   <br><img src="images/FolderStructure.png" alt="folder structure">

2. Create a new file, called **PoetrySlamService.edmx**, in the folder **partner-reference-extension-catering-ui**.
3. Copy the content of the [PoetrySlamService.edmx](./data/PoetrySlamService.edmx) into the newly created file.

   > Note: For more information on how to get the metadata, you can have a look at the [Partner Reference Application Extension tutorial - Get Poetry Slam Manager EDMX](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/Tutorials/03-FioriUIForExtendedEntity.md#get-poetry-slam-manager-edmx).

### Add a Web Application with SAP Fiori Elements

You create the SAP Fiori elements project by uploading the metadata using the **SAP Fiori Elements Application Wizard** under the **partner-reference-extension-catering-ui** folder.

1. To start a new development project, go to the settings in SAP Business Application Studio from your customer subaccount and open the **Command Palette**.
2. To start the wizard, search for **SAP Business Application Studio: New Project from Template** in the **Command Palette...**.
3. Select the **SAP Fiori Generator** module template and choose *Start*. 
4. Choose **List Report Page** and choose *Next*.
5. Select the data source and the OData service as follows:
   - **Data source**: Upload a Metadata Document
   - **Metadata file path**: Upload the metadata file created in the previous section
6. Select the main entity from the list:
   - **Main entity**: x_Caterers
   - **Automatically add table columns**: Yes
   - **Table Type**: Responsive
7. Add further project attributes. For attributes that aren't listed, use the default:
   - **Module name**: caterer
   - **Application title**: Caterers
   - **Application namespace**: leave empty
   - **Description**: Application to create and manage caterers
   - **Project folder path**: Choose the project folder. Make sure that the target folder path is set to `/home/user/projects/partner-reference-extension-catering-ui`.
   - **Add deployment configuration**: Yes
8. Select the deployment configuration:
   - **Please choose the target**: Cloud Foundry
   - **Destination name**: none
   - **Add Router Module**: Add Application to Managed Application Router
9. Choose **Finish**. The wizard creates the **caterer** folder, which contains all files related to the user interface.

### Fine-Tune the User Interface

Creating the extension UI follows the same process as developing the standard UI for the **Poetry Slam Manager** application. For more information beyond this exercise, you can have a look at the [Partner Reference Application tutorial - Fine-Tune the User Interface](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/14-Develop-Core-Application.md#fine-tune-the-user-interface).

Copy the following files:

1. Overwrite the `caterer/webapp/manifest` file with the [`manifest.json`](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering-ui/caterer/webapp/manifest.json) file from the Partner Reference Application Extension. It describes the application structure, routing, services, dependencies, and SAP Fiori launchpad integration. The **crossNavigation** section must be defined to enable intent-based navigation, allowing the app to be launched using a specified semantic object and action.
2. Overwrite the `caterer/webapp/annotations/annotation.xml` file with the [`annotation.xml`](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering-ui/caterer/webapp/annotations/annotation.xml) file from the Partner Reference Application Extension. It defines UI-specific annotations that dictates how data is displayed and behaves in the SAP Fiori application.
3. Overwrite the `caterer/webapp/i18n/i18n.properties` file with the [`i18n.properties`](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering-ui/caterer/webapp/i18n/i18n.properties) file from the Partner Reference Application Extension. It takes care of the translation of the UI into different languages.

### Add Destination Route to App Router

To ensure secure communication between the new SAP Fiori application and the core **Poetry Slam Manager** OData service, all requests from the SAP Fiori application must be routed. This is achieved by configuring the routing rules in the [xs-app.json](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering-ui/caterer/xs-app.json) file. 

1. Add the following destination route as the first entry in the route configuration:

```json
{     
   "source": "^/odata/v4/poetryslamcaterer/(.*)$",
   "target": "/odata/v4/poetryslamservice/$1",
   "destination": "poetry-slams-caterer",
   "authenticationType": "xsuaa",
   "csrfProtection": true
}
```

> Note: For more information about the properties, check the [Partner Reference Application Extension tutorial - Add Destination Route to App Router](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/Tutorials/03-FioriUIForExtendedEntity.md#add-destination-route-to-app-router).

### Service Broker Setup

This chapter provides information only. In this demo scenario, the service broker is already set up in the customer subaccount. You don't need to take any action.

To access the **Poetry Slams** OData services from your SAP Fiori application, the service broker is set up. It enables secure access to application OData services by utilizing tenant-specific credentials and authorizations. This ensures effective tenant isolation in a multi-tenant application environment. 

> Note: In case you want to get more information on how the service broker is added to the **Poetry Slam Manager** application, refer to the [Partner Reference Application tutorial - Enable API Access to SAP BTP Applications Using Service Broker](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/42a-Multi-Tenancy-Service-Broker.md#enable-api-access-to-sap-btp-applications-using-service-broker).

### Create the Destination to Access the Base Application

You need to create a destination in the customer subaccount to connect to the **Poetry Slams** OData service using the service broker instance. This destination was configured in the [xs-app.json](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering-ui/PoetrySlamService.edmx) file to enable tenant-specific data calls in an earlier step.

1. First add the information from the service key of the service broker instance *psm-sb-sub1-full* to your notes:
   1. In the SAP BTP cockpit of the **customer subaccount**, navigate to **Services** -> **Instances and Subscriptions**.
   2. View the credentials of the service broker instance *psm-sb-sub1-full*.
   3. Use the *psm-servicebroker* in section *endpoints* as **ServiceBrokerEndpoint**. 
   4. Get the **ServiceBrokerClientID** and **ServiceBrokerClientSecret** from the *uaa*-section.
   5. For the **ServiceBrokerTokenServiceURL**, get the *URL* of the *uaa*-section and add `/oauth/token`. 

2. Go to **Connectivity** and choose **Destinations**.

3. Create a new destination with the following field values:

| Parameter Name    | Value                                                                                             |
| :---------------- | :------------------------------------------------------------------------------------------------ |
| **Name**            | `poetry-slams-caterer`                                                                          |
| **Type**            | `HTTP`                                                                                          |
| **Description**     | Destination description. For example, `Poetry Slams Manager`.                                   |
| **URL**             | Use entry ServiceBrokerEndpoint from your notes.                                                |
| **Proxy Type**      | `Internet`                                                                                      |
| **Authentication**  | `OAuth2UserTokenExchange`                                                                       |
| **Client Id**       | Use entry ServiceBrokerClientID from your notes.                                                |
| **Client secret**   | Use entry ServiceBrokerClientSecret from your notes.                                            |
| **Token Service URL**:      | Use entry ServiceBrokerTokenServiceURL from your notes.                                 |
| **Token Service URL Type**: | `Dedicated`                                                                             |

### Deploy to Cloud Foundry

Now that you have completed all the development and configurations, you can proceed to deploy the application.

1. In the SAP Business Application Studio, open a new terminal and navigate into the `home/user/projects/partner-reference-extension-catering-ui` folder.
2. Log on to SAP BTP Cloud Foundry runtime:
   1. Run the command: 
      
      ```
      cf login -a https://api.cf.eu10-004.hana.ondemand.com --origin teched01-platform
      ```

   2. Enter your user and password.
   3. Select the org of the **SAP BTP customer subaccount for the application (*dt2650nn*)**. Replace nn with the number of your customer.
      > Note: The SAP BTP Cloud Foundry runtime space of your customer (*DT265*) is selected by default.

3. Navigate into the **caterer** folder with the command:
   ```
   cd caterer
   ```
4. To install the dependencies, run the command:
   ```
   npm install
   ```
5. To build the application, run the command:
   ```
   npm run build:mta
   ```

6. To deploy the application, run the command:
   ```
   npm run deploy
   ```

### Configure SAP Build Work Zone

You already created a *Launchpad Site* in SAP Build Work Zone for the **Poetry Slam Manager** application. Now you have to add the **Caterer** web application.

#### Add Web Application to the Content Manager

The Content Manager is the central place to manage applications, roles, and groups in SAP Build Work Zone. Applications must be added to the content repository before they can be assigned to users.

1. Open the **SAP Build Work Zone, standard edition** application under **Services -> Instances and Subscriptions** in the **customer subaccount**. The site manager is opened.
2. Open the **Channel Manager**.
3. Update the **HTML5 Apps** content channel using the **Update content** button on the right side of the table to get the latest changes of your application.
4. Go to the **Content Manager**.
5. Select **Content Explorer**.
6. Choose **HTML5 Apps**.
7. Select the **Caterer** web application.
8. Choose **Add**.

#### Assign the Application to a Role

Role assignments define who can access and interact with an application within the launchpad. By assigning the application to the **Everyone** role or any specific role based on business needs, you ensure that authorized users can see and use the application. The **Everyone** role is assigned to a site by default.

1. Open the **Content Manager**.
2. Click on the **Everyone** role (available by default).
3. Edit the role.
4. Assign the **Caterer** application to this role.
5. Save your changes.

#### Create a New Group for the Application

Creating a group allows you to categorize applications based on functionality, user roles, or business processes. This improves accessibility by ensuring that related applications appear together in a structured manner.

1. Open the **Content Manager**.
2. Create a new **Group**.
3. Enter a title, for example **Caterer**, and a description, for example **Maintain caterer** for better organization.
4. Add the **Caterer** application to the newly created group.
5. Save your changes.

## Exercise 3.2 - Test the Extension

1. Now, let's add a new caterer. Launch the **Caterer** application.

    1. Open the SAP Build Work Zone launchpad you created and enhanced with the extension. Use the URL **LaunchpadSiteURL** you noted down before.
    2. Choose **Caterer**.

2. A list of existing caterers is displayed in the table. Choose **Create** and enter the following details:

    1. **Name**: Goose   
    2. **Contact Person**: Peter 
    3. **Phone**: +555-1212
    4. **Email**: peter@goosesap.com
    5. **Cuisine**: Mexican
    6. **Maximum Service Capacity**: 300

3. Choose **Create** to add a new caterer.
4. Switch to the **Poetry Slam Manager** solution of your customer.
5. Open the **Poetry Slam Events** app.
6. Select a published poetry slam.

   You can see that the new **Caterer** section is available.
7. Choose **Edit** and select the newly created caterer for your event from the predefined list. The poetry slam is updated with the caterer information.
8. Save your changes.

## Summary

You've now extended the **Poetry Slam Manager** subscription of your customer with the **Caterer Management** extension.

Continue with [personalizing the user interface for all users of your customer](../ex3/README.md).