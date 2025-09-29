# Exercise 2 - Build and Deploy a Tenant Extension for Your Customer

In this exercise, you will create a tentant extension *Caterer* to select a caterer for a poetry slam in the Poetry Slam Manager of your customer.

> Note: Some steps are required to enable extensibility in the base application *Poetry Slam Manager*. You can get more details in chapter [Enable Consumer-Specific Extensions](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/50-Multi-Tenancy-Features-Tenant-Extensibility.md) of the Partner Reference Application.

## Exercise 2.1 - Extend the Persistence and the User Interface With Custom Fields

After completing these steps you will have extended the Poetry Slam Manager of your customer, with a section *Caterer* in which a caterer for the event can be selected.

### Create a New Project Based on SAP Cloud Application Programming Model

First, create an extension project, which is a standard SAP Cloud Application Programming Model based project designed to extend the functionality of the Poetry Slam Manager application.

1. Open the SAP Business Application Studio in your customer subaccount.

2. To start a new development project, go to the settings and open the *Command Palette...*.

3. Search for `SAP Business Application Studio: New Project from Template`.

4. Choose *CAP Project* and then click on *Start*. Make sure the target folder path is set to `home/user/projects`.

5. Add the following attributes to the *CAP Project* and then click on *Finish*:
    - As *project name*, enter `partner-reference-extension-catering`.
    - Select `Node.js` as your *runtime*.

    As a result, a folder named `partner-reference-extension-catering` is created, containing a set of files to help you start an SAP Cloud Application Programming Model project.

6. Adapt the created *package.json* to incorporate the following configurations:

    ```json
    {
      ...
      "extends": "partner-reference-application",
      "workspaces": [
        ".base"
      ]
      ...
    }
    ```

    - `extends` is the identifier used by the extension model to reference the base model. It must be a valid base application name, as it serves as the package name for the base model when executing `cds pull`.
    - `workspaces` is a list of folders, including the one where the base model is stored. If not already present, the `cds pull` command will automatically add this property to ensure proper configuration.

### Assign Extension Developer Role

To extend the base model, assign the `PoetrySlamExtensionDeveloperRoleCollection` role collection to your user in the customer subaccount. 

> Note: The `PoetrySlamExtensionDeveloperRoleCollection` role collection is established within the base model as part of the extension enablement process. For more details, refer to [Partner Reference Application Tutorial - Poetry Slam Manager Application with extensibility](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/50-Multi-Tenancy-Features-Tenant-Extensibility.md#application-enablement).

### Pull the Base Model

To enable local testing and syntax highlighting, you need to ensure that the model information of the base application (the *Poetry Slam Manager*) is known to the extension project. The cds compiler requires the base model as an NPM package within the node_modules folder.

The following section explains how to retrieve the model from the running application and make it available locally using the cds pull command.

1. To simplify the use of multitenancy-related commands, for example, `cds pull` and `cds push`, by enabling automatic authentication, first log in using the appropriate credentials.

   1. Get the URL of the multitenancy extension module (MTX) deployed in the provider subaccount.

      1. In the SAP BTP cockpit of the provider subaccount, navigate to the SAP BTP Cloud Foundry runtime space where the application is deployed.

      2. In the space cockpit, navigate to *Applications* and select the link *poetry-slams-mtx*.

      3. Copy the *Application Route* from the *Application Overview* for later use (**provider mtx URL**).

      4. Do not close the window. you will require it in step 3.

   2. Get the subdomain from the subscriber subaccount.

      1. In the SAP BTP cockpit of the subscriber subaccount, navigate to *Overview*.

      2. Copy the *Subdomain* from the *General* section and note it as **subscriber domain**. 

   3. Get the *Client Credentials* of the MTX module deployed in the provider subaccount.

      1. Go back to the *poetry-slams-mtx* MTX module of the provider subaccount.

      3. Select *Environment Variables*, navigate to VCAP_SERVICES, locate xsuaa, and copy the *clientid* and *clientsecret*.

      4. In the terminal of your development environment execute the following statement to log into the MTX module.

            ```bash
            cds login <PROVIDER-MTX-APP-URL> -s <SUBSCRIBER-SUBDOMAIN> -c '<CLIENT-ID>':'<CLIENT-SECRET>' --plain
            ```

2. Pull the latest cds model from the provider subaccount to the extension project `partner-reference-extension-catering`.

   1. Run the following command. Use the copied application route **provider mtx URL**.

    ```bash
    cds pull --from <PROVIDER-MTX-APP-URL>
    ```

### Install the Base Model

To prepare the downloaded base model for usage in your extension project, install it by running the `npm install` command.

> Note: This will link the base model in the workspace folder to the subdirectory node_modules/partner-reference-application.

### Write the Extension Code

It is not mandatory to split the extension model into multiple files; however, for better maintainability CAP recommends structuring it similarly to a standard SAP CAP application. The data model enhancements should be placed in the `db` folder, service enhancements in the `srv` folder, and UI annotations in the `app` folder. This approach ensures a well-organized project structure and aligns with best practices.

1. Extending the data model.

    1. In the sample extension application, you will create a new entity called *x_Caterers* and extend the existing *PoetrySlams* entity by adding an association to it.

        ```cds
        // Extend the entity with new x_Caterers entity
        extend PoetrySlams with {
          x_caterer : Association to one x_Caterers; 
        }
        ```

       > Note: By using the `extend` directive, you can add new fields to existing entities, create new entities, and extend existing entities with new associations. Additionally, you can add compositions to both existing and new entities, set default values, define range checks, and apply value list validations to new or existing fields. Furthermore, you can enforce mandatory field checks and introduce unique constraints on new or existing entities, ensuring data integrity and consistency. 

    2. Create a file [catererManager.cds](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/db/catererManager.cds) in the */db* folder, which will reference all the entity definitions (cds-file of the database).

        > Note: The prefix `x_` is added to entities, namespaces, or fields based on the configuration defined during extensibility enablement in the base application. This ensures that artifacts created in the extended application do not conflict with those in the base application, maintaining consistency and avoiding naming collisions. For more information, refer to the [Partner Reference Application Tutorial - Poetry Slam Manager Application with extensibility](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/50-Multi-Tenancy-Features-Tenant-Extensibility.md#application-enablement).

        > Note: For more details on defining domain model, refer to the [Partner Reference Application Tutorial - Define the Domain Models](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/14-Develop-Core-Application.md#define-the-domain-models).

2. Extending the service model.

    After you've defined the domain model with its entities, define a set of services to add API's to the application. In the existing service, the newly created entities are automatically included as they serve as targets for the corresponding compositions. Additionally, these new entities are automatically exposed in a read-only manner as CodeLists.

    1. New entities are automatically exposed in a read-only manner. If you need to change this behavior, you can explicitly expose them.

        ```cds
        extend service PoetrySlamService with{
            entity x_Caterers as projection on caterers;
        }
        ```

    2. Create a file called [catererManagerService.cds](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/srv/catererManagerService.cds) in the */srv* folder, which will reference all the service definitions (cds-file of the service).

3. Extending UI annotations. Once you have defined the domain and service model, you extend UI annotations. 

    1. In the sample extension application, you will add a new *Caterer* section between the *General* and *Visitors* sections using the `... up to` and `...` keywords. The following code demonstrates how to seamlessly insert a new UI facet between the existing facets in the base application, enhancing its functionality while preserving the integrity of the current layout.

        ```cds
        annotate PoetrySlamService.PoetrySlams with @(
          UI : {
            Facets                        : [
              ... up to {ID    : 'GeneralData'},
              {
                $Type : 'UI.ReferenceFacet',
                ID    : 'CatererData',
                Label : '{i18n>caterer}',
                Target: '@UI.FieldGroup#Caterer'
              },
              ...
            ],
          }
        );
        ```

        > Note: Extending UI annotations in SAP CAP Extension Development allows you to customize and enhance the user interface of a base application without altering its core functionality. This can involve adding new UI elements such as sections, fields, and tables, modifying existing annotations, adjusting field visibility and read-only status, and defining value helps or lists. You can also reorganize the layout of pages, configure smart tables and forms, and add action buttons. By using the `extend` directive in CDS models, you can seamlessly introduce customizations, improve user experience, and ensure flexibility in adapting to business requirements while maintaining the integrity of the original application.

    2. Create a file called [catererManager.cds](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/app/catererManager.cds) in the */app* folder, which will reference all the UI annotations (cds-file of the UI).

    3. Configure [internationalization(i18n)](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/i18n/i18n.properties) to align with specific requirements, like supporting several languages.

### Create an Initial Data Set

  Create an initial data set which will be available after you've started the application. You can specify the data in SAP Cloud Application Programming Model by creating a set of CSV files in the /db/data folder of the project. 

  1. Create a file called [x_sap.samples.poetryslams.catering-x_CuisineTypeCodes.csv](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/db/data/x_sap.samples.poetryslams.catering-x_CuisineTypeCodes.csv) in the */db/data* folder, which adds the cuisine data to the entity.

  2. Create a file called [x_sap.samples.poetryslams.catering-x_Caterers.csv](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/db/data/x_sap.samples.poetryslams.catering-x_Caterers.csv) in the */db/data* folder, which adds the caterer data to the entity.

  > Note: This adds caterers data for demonstration purposes, which is not suitable for a production environment. 

  > Note: For more details, refer to the [Partner Reference Application Tutorial - Create an Initial Data Set](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/14-Develop-Core-Application.md#create-an-initial-data-set)

### Deploy the Extension
  
  Now that you have added your extension code, you deploy it by pushing the code to your subscriber subaccount. This ensures that your extension code is isolated to your subaccount, preventing any impact on other subscriber subaccounts.

  To push the extension code to the subscriber subaccount.
  
  1. Use the URL of the MTX module deployed in the provider subaccount. You noted it as **provider mtx URL**.

  2. Use the subdomain from the subscriber subaccount. You noted it as **subscriber domain**.

  ```bash
  cds push --to <PROVIDER-MTX-APP-URL> -s <SUBSCRIBER-SUBDOMAIN>
  ```

 > Note: With that a new `Caterer` section is introduced in the Poetry Slam Manager application within your subscriber subaccount, enabling you to effortlessly select caterers for your events.

## Exercise 2.2 - Develop a Fiori UI to Manage the Customer Entity

After completing these steps you will have created a Fiori Elements app for managing caterer for the poetry slam events.

> Note: The SAP BTP Cloud Foundry runtime environment is required in the subaccount of your customer, as the web application will be deployed to manage caterer information. In this demo, it is already enabled. For more details how this was done, refer to the [Partner Reference Application Tutorial - Enable SAP BTP Cloud Foundry Runtime](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/22-Multi-Tenancy-Prepare-Deployment.md#enable-sap-btp-cloud-foundry-runtime).

### Get Poetry Slam Manager EDMX

You consume the metadata of the *Poetry Slams* OData service from the customer subaccount, where the backend is independently managed. This metadata is then utilized to create the Fiori Elements project. To obtain the metadata, follow the steps outlined below:

1. Copy the poetry slams application URL from the subscriber subaccount.

    1. In the SAP BTP cockpit of the subscriber subaccount, navigate to *Instances and Subscriptions* in the *Services* section.

    2. Copy the URL of the *Poetry Slam Manager* application on the *Subscription* tab.

2. Open any web browser, such as *Google Chrome*, and enable the *Developer Tools*.

    > Note: You can enable *Developer Tools* in *Google Chrome* using a keyboard shortcut:
    >
    > 1. Windows/Linux: Press `Ctrl + Shift + I` or `F12`.  
    > 2. Mac: Press `Cmd + Option + I`.

3. Paste the application URL into the browser's address bar and press *Enter*.

4. In the *Network* tab's filter bar, type `metadata` to filter the requests related to the metadata.

5. Copy the response of the metadata API call.

6. Create a folder (e.g. *partner-reference-extension-catering-ui*) in your workspace in SAP Business Application Studio.

7. Create a new file (e.g. PoetrySlamService.edmx) paste the metadata.

### Add a Web Application with SAP Fiori Elements

You create the Fiori Elements project by uploading the metadata using the *SAP Fiori Elements Application Wizard* under *partner-reference-extension-catering-ui* folder.

1. To start a new development project, go to the settings in SAP Business Application Studio from your subscriber subaccount and open the Command Palette.
2. To start the wizard, search for *SAP Business Application Studio: New Project from Template* in the *Command Palette...*.
3. Select the *SAP Fiori Generator* module template and then click on *Start*. 
4. Choose *List Report Page*. and click on *Next*.
5. Select the data source and the OData service as follows:
   - *Data source*: *Upload a Metadata Document*
   - *Metadata file path*: *Upload the metadata file created in the previous section*
6. Select the main entity from the list:
   - *Main entity*: *x_Caterers*
   - *Automatically add table columns*: *Yes*
   - *Table Type*: *Responsive*
7. Add further project attributes:
   - *Module name*: *caterer*
   - *Application title*: *Caterers*
   - *Application namespace*: leave empty
   - *Description*: *Application to create and manage caterers*
   - *Project folder path*: *Choose the project folder*
   - *Enable Typescript*: *No*
   - *Add deployment configuration*: *Yes*
   - *Add FLP configuration*: *No*
   - *Configure Advanced Options*: *No*
8. Select the deployment configuration:
   - *Please choose the target*: *Cloud Foundry*
   - *Destination name*: *none*
   - *Add application to managed application router*: *Yes*
9. Choose *Finish*. The wizard creates the *caterer* folder, which contains all files related to the user interface.

### Fine-Tune the User Interface

Creating the extension UI follows the same process as developing the standard UI for the Poetry Slam Manager application. For more information, you can have a look at the [Partner Reference Application - Fine-Tune the User Interface](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/14-Develop-Core-Application.md#fine-tune-the-user-interface).

Copy the following files:

1. Copy the file[`manifest.json`](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering-ui/caterer/webapp/manifest.json). It describes the application structure, routing, services, dependencies, and SAP Fiori launchpad integration. The *crossNavigation* section must be defined to enable intent-based navigation, allowing the app to be launched using a specified semantic object and action.
2. Copy the file [`annotation.xml`](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering-ui/caterer/webapp/annotations/annotation.xml). It defines UI-specific annotations that dictate how data is displayed and behaves in the SAP Fiori application.
3. Copy the file [`i18n.properties`](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering-ui/caterer/webapp/i18n/i18n.properties). It takes care of the translation of the UI into different languages.

### Add Destination Route to App Router

To ensure secure communication between the new SAP Fiori application and the core Poetry Slam Manager OData service, all requests from the SAP Fiori application must be routed. This is achieved by configuring the routing rules in the [xs-app.json](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering-ui/caterer/xs-app.json) file. Add the following destination route as the first entry in the route configuration.

```json
{  
   ...   
   "source": "^/odata/v4/poetryslamcaterer/(.*)$",
   "target": "/odata/v4/poetryslamservice/$1",
   "destination": "poetry-slams-caterer",
   "authenticationType": "xsuaa",
   "csrfProtection": true
   ...
}
```

> Note: For more information about the properties, check the tutorial [Partner Reference Application Extension - Add Destination Route to App Router](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/Tutorials/03-FioriUIForExtendedEntity.md#add-destination-route-to-app-router).

### Service Broker Setup

To access the *Poetry Slams* OData services from your SAP Fiori application, you must use the service broker. The service broker enables secure access to application OData services by utilizing tenant-specific credentials and authorizations, ensuring effective tenant isolation in a multi-tenant application environment. 
In this demo scenario the service broker is already setup in the customer subaccount.

> Note: In case you want to get more information on how the service broker is added to the Poetry Slam Manager application, refer to the [Partner Reference Application Tutorial - Enable API Access to SAP BTP Applications Using Service Broker](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/42a-Multi-Tenancy-Service-Broker.md#enable-api-access-to-sap-btp-applications-using-service-broker).


### Create the Destination to Access the Base Application

You need to create a destination in the subscriber subaccount to connect to the *Poetry Slams* OData service using the service broker instance. This destination is already configured in the [xs-app.json](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering-ui/PoetrySlamService.edmx) file to enable tenant-specific data calls.

In the SAP BTP consumer subaccount, go to *Connectivity* and choose *Destinations* to create a *New Destination* with the following field values:

| Parameter Name    | Value                                                                                             |
| :---------------- | :------------------------------------------------------------------------------------------------ |
| *Name*            | *poetry-slams-caterer*                                                                            |
| *Type*            | *HTTP*                                                                                            |
| *Description*     | Destination description (For example, `SAP S/4HANA Cloud XXXXXX`)                                 |
| *URL*             | Get the URL of the application service from the provider subaccount. |
| *Proxy Type*      | *Internet*                                                                                        |
| *Authentication*  | *OAuth2UserTokenExchange*                                                                         |
| *Client Id*       | Get the client id from the service key of the service broker instance created in the subscriber subaccount.                                                                         |
| *Client secret*   | Get the client secret from the service key of the service broker instance created in the subscriber subaccount.                                                                         |
| *Token Service URL Type*: | *Dedicated*                                                                               |
| *Token Service URL*:      | `https://<SUBSCRIBER-SUBDOMAIN>.authentication.eu12.hana.ondemand.com/oauth/token`        |

- *URL*: Get the URL of the application service from the provider subaccount:
   1. In the SAP BTP cockpit of the provider subaccount, navigate to the SAP BTP Cloud Foundry runtime space where the application is deployed.
   2. In the space cockpit, navigate to *Applications* and choose the link *poetry-slams-srv*.
   3. Copy the *Application Route* from the *Application Overview* for later use (**provider service URL**).

- *Client Id* and *Client secret* - Get the client id and client secret from the service key of the service broker instance created in the subscriber subaccount.
   1. In the SAP BTP cockpit of the subscriber subaccount, navigate to the SAP BTP Cloud Foundry runtime space where the service broker instance is created.
   2. Get the *client id* and *client secret* from the service key of service broker instance.

- *Token Service URL*: Use the **subscriber domain**, you noted before.    


### Deploy to Cloud Foundry

Now that you have completed all the development and configurations, you can proceed to deploy the application.

1. Open a new terminal and log on to SAP BTP Cloud Foundry runtime:
   1. Run the command `cf login`.
   2. Enter the SAP BTP Cloud Foundry runtime API of your environment, for example, `https://api.cf.eu10.hana.ondemand.com`.

      1. In the SAP BTP cockpit of the customer subaccount, navigate to *Overview*.

      2. Copy the *API Endpoint* from *Cloud Foundry Environment*.

   3. Enter your development user and password.
   4. Select the org of the SAP BTP customer subaccount for the application.
   5. Select the SAP BTP Cloud Foundry runtime space (*app*).

2. To install the dependencies, run the command `npm run install`.

3. To build the application, run the command `npm run build:mta`.

4. To deploy the application, run the command `npm run deploy`.

### Configure SAP Build Work Zone

You already created a *Launchpad Site* in *SAP Build Work Zone* for the *Poetry Slam Manager* application. Now you have to add *Caterer* web application.

#### Add Web Application to the Content Manager

The *Content Manager* is the central place to manage applications, roles, and groups in *SAP Build Work Zone*. Applications must be added to the content repository before they can be assigned to users.

1. Open the application *SAP Build Work Zone, standard edition* under *Instances and Subscriptions* in the customer subaccount. The site manager is opened.
2. Open the *Channel Manager*.
3. Update the *HTML5 Apps* content channel to get the latest changes of your application.
4. Go to the *Content Manager*.
5. Click on *Content Explorer*.
6. Choose *HTML5 Apps*.
7. Choose the *Caterer* web application.
8. Click on *Add*.

#### Assign the Application to a Role

Role assignments define who can access and interact with an application within the launchpad. By assigning the application to the *Everyone* role (or any specific role based on business needs), you ensure that authorized users can see and use the application.

1. Open the *Content Manager*.
2. Choose the *Everyone* role (available by default).
3. Assign the *Caterer* application to this role.
4. Save your changes.

#### Create a New Group for the Application

Creating a Group allows you to categorize applications based on functionality, user roles, or business processes. This improves accessibility by ensuring that related applications appear together in a structured manner.

1. Open the *Content Manager*.
2. Create a new *Group*.
3. Enter a *Title* and *Description* for better organization.
4. Add the *Caterer* application to the newly created group.
5. Save your changes.

#### Configure Role Assignments in Site Settings

By assigning the *Everyone* role to your site, you provide authorized users with the necessary permissions to access the launchpad, navigate through available applications, and use the applications seamlessly.

1. Navigate to the *Site Directory*.
2. Open *Site Settings*.
3. Go to *Role Assignments*.
4. Assign the *Everyone* role.
5. Save your changes.

## Test the Extension

1. Now, let's add a new caterer. Launch the *Caterer* application.

    1. In the SAP BTP cockpit of the subscriber subaccount, navigate to *HTML5 Applications* under *HTML5*.

    2. Choose the *Caterer* application from the *Application* table.

2. A list of existing caterers is displayed in the table. Choose *Create* and enter the following details:

    1. *Name*: *Goose*
    
    2. *Contact Person*: *Peter*
    
    3. *Phone*: *+555-1212*
    
    4. *Email*: *peter@goosesap.com*
    
    5. *Cuisine*: *Mexican*
    
    6. *Maximum Service Capacity*: *300*

3. Choose *Create* to add a new caterer.

4. Switch to the Poetry Slam Manager application of your customer.

5. Open *Manage Poetry Slams*.

6. Select a published poetry slams.

7. You can see that the new *Caterer* section is available. Choose *Edit*, select the newly created caterer for your event from the predefined list.

8. The poetry slam is updated with the caterer information. Save your changes.

## Summary

You've now extended the Poetry Slam Manager subscription of your customer with the *Caterer* extension.

Continue to [Personalize the User Interface for All Users of Your Customer](../ex3/README.md)

