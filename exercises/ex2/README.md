# Exercise 2 - Extend the Poetry Slam Manager with a Customer-Specific Extension

In this exercise, you create a tenant-specific extension that lets you select a caterer for a poetry slam in your customer's **Poetry Slam Manager**.

> Note: Some steps are required to enable extensibility in the **Poetry Slam Manager** base application. You can get more details in chapter [Enable Consumer-Specific Extensions](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/50-Multi-Tenancy-Features-Tenant-Extensibility.md) of the Partner Reference Application.

## Exercise 2.1 - Extend the Persistence and the User Interface With Custom Fields

After completing below steps, you have extended the **Poetry Slam Manager** of your customer with a **Caterer** section in which a caterer for the event can be selected.

### Create a New Project Based on SAP Cloud Application Programming Model

First, create an extension project. This project is a standard SAP Cloud Application Programming Model-based project designed to extend the functionality of the **Poetry Slam Manager** application.

1. Open the SAP Business Application Studio in your **customer subaccount**.

2. Open the space you created in exercise 1.

3. To start a new development project, go to the settings and open the **Command Palette** or press CTRL + SHIFT + P.

4. Search for `SAP Business Application Studio: New Project from Template`.

5. Make sure the target folder path is set to `home/user/projects`. 

6. Choose **CAP Project** and select **Start**. 

7. Add the following attributes to the **CAP project** and choose **Finish**:
    - As **project name**, enter `partner-reference-extension-catering`.
    - As **runtime**, select `Node.js`.

    As a result, a folder named `partner-reference-extension-catering` is created. It contains a set of files to help you start a SAP Cloud Application Programming Model project.

8. Adapt the created *package.json* to incorporate the following configurations. For reference, have a look at the [package.json](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/package.json) file in the Partner Reference Application Extension.

    ```json
      "extends": "partner-reference-application",
      "workspaces": [
        ".base"
      ]
    ```

    - `extends` is the identifier used by the extension model to reference the base model. It must be a valid base application name, as it serves as the package name for the base model when executing `cds pull`.
    - `workspaces` is a list of folders, including the one where the base model is stored. If not already present, the `cds pull` command will automatically add this property to ensure proper configuration.

### Assign Extension Developer Role

To develop extensions for the **Poetry Slam Manager**, you require the authorizations to extend the base model.

1. In the **customer subaccount**, go to **Security** -> **Role Collections**.
2. Edit the role collection `PoetrySlamExtensionDeveloperRoleCollection` that was created by the **Poetry Slam Manager**. 
3. In section **Users**, start typing `DT265` in the **ID** field.
4. Select your user (`DT265-0*nn*@education.cloud.sap`) from the **Identity Provider** `Custom IAS tenant`. Replace *nn* with your customer number. 
5. Click the `+`-button.
6. Save the changes.

> Note: The `PoetrySlamExtensionDeveloperRoleCollection` role collection is established within the base model as part of the extension enablement process. For more details beyond this exercise, refer to [Partner Reference Application tutorial - Poetry Slam Manager Application with extensibility](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/50-Multi-Tenancy-Features-Tenant-Extensibility.md#application-enablement).

### Pull the Base Model

To enable local testing and syntax highlighting, you need to ensure that the model information of the **Poetry Slam Manager** base application is known to the extension project. The CDS compiler requires the base model as an NPM package within the `node_modules` folder.

The following section explains how to retrieve the model from the running application and make it available locally using the CDS pull command.

1. To simplify the use of multitenancy-related commands, such as `cds pull` and `cds push`, you can enable automatic authentication. First, log in using the appropriate credentials.

   1. Get the URL of the multitenancy extension module (MTX) deployed in the provider subaccount.

      1. In the SAP BTP cockpit of the **provider subaccount** ([*DT265_PROVIDER*](https://emea.cockpit.btp.cloud.sap/cockpit?idp=teched01.accounts.ondemand.com#/globalaccount/bab22592-b4b6-405e-add4-a6f3d9869306/subaccount/ed30d4c9-9da7-421c-bd08-c5ae51337fce/subaccountoverview)), navigate to the SAP BTP Cloud Foundry runtime space where the application is deployed. The **Applications** deployed to the space are shown.

      2. Select the **poetry-slams-mtx** link.

      3. Copy the **Application Route** from the **Application Overview**. Add it to your notes for later use (**ProviderMTXURL**).

   2. Get the **Client Credentials** of the MTX module.

      1. On the same screen, select **Environment Variables** of the MTX module.
      2. In the JSON file of the **System-Provided** section, locate the *xsuaa* array.
      3. Copy the **clientid** of the **credentials** property. Add it to your notes for later use (MTXClientID).
      4. Copy the **clientsecret** of the **credentials** property. Add it to your notes for later use (MTXClientSecret).

   3. Open a new terminal in your SAP Business Application Studio space (Control + Shift + C). 
   
   4. In the terminal, execute the following statement to log in to the MTX module. Use the entries and statement from your notes.

         ```bash
         cds login <ProviderMTXURL> -s <CustomerSubdomain> -c '<MTXClientID>':'<MTXClientSecret>' --plain
         ```

2. Pull the latest CDS model from the provider subaccount to the `partner-reference-extension-catering` extension project.

   1. Run the following command. Use the copied application route **ProviderMTXURL**. Use the statement from your notes.

      ```bash
      cds pull --from <ProviderMTXURL>
      ```

### Install the Base Model

The downloaded base model needs to be prepared for use in your extension project.

1. Install it by running the command:

   ```
   npm install
   ```

> Note: This links the base model in the workspace folder to the `node_modules/partner-reference-application` sub directory.

### Write the Extension Code

It's not mandatory to split the extension model into multiple files. However, for better maintainability, CAP recommends structuring it similarly to a standard SAP CAP application. The data model enhancements should be placed in the `db` folder, service enhancements in the `srv` folder, and UI annotations in the `app` folder. This approach ensures a well-organized project structure and aligns with best practices.

1. To extend the data model, you create a new entity called *x_Caterers* and extend the existing **PoetrySlams** entity by adding an association to it.

    1. Create a file named `catererManager.cds` in the **/db** folder, which references all the entity definitions (cds-file of the data model).

    2. Copy the following content into the newly created file:

      ```cds
      namespace  x_sap.samples.poetryslams.catering;
      using {sap.samples.poetryslams.PoetrySlams, cuid, managed,sap } from 'partner-reference-application';

      // Define a new code list.
      entity x_CuisineTypeCodes: sap.common.CodeList {
         key code : String(25) default '1' @title : '{i18n>cuisineType}'
      }

      // Define a new entity.
      entity x_Caterers : cuid,managed {
         name               : String(255) @mandatory @title : '{i18n>name}'; 
         contactPerson      : String(255) @title : '{i18n>contactPerson}';
         phone              : String(30) @title : '{i18n>phone}';
         email              : String @assert.format: '^[\w\-\.]+@([\w-]+\.)+[\w-]{2,4}$' @title : '{i18n>email}';
         cuisine            : Association to one x_CuisineTypeCodes;
         maxServiceCapacity : Integer @title : '{i18n>maxServiceCapacity}';
      }

      // Enhance the entity PoetrySlams of the base application.
      // Prefix x_ is based on the configuration maintained in the base application. https://cap.cloud.sap/docs/guides/multitenancy/mtxs#extensibility-config
      // This ensures that artifacts of the extension do not conflict with those in the base application, maintaining consistency and avoiding naming collisions.
      extend PoetrySlams with {
         x_caterer : Association to one x_Caterers @title: '{i18n>caterer}'
      }

      annotate x_Caterers with {
         cuisine @Common : { 
            Label : '{i18n>cuisine}',
            Text : {
               $value : cuisine.name,
               ![@UI.TextArrangement]: #TextOnly,
            }
         }
      };
      ```

      > Note: By using the `extend` directive, you can add new fields to existing entities, create new entities, and further more. Details beyond this exercise can be found in chapter [Extending the Data Model](https://cap.cloud.sap/docs/guides/extensibility/customization#extending-the-data-model) of the capire docmentation.
       
      > Note: The prefix x_ is based on the configuration maintained in the base application. This ensures that artifacts of the extension do not conflict with those in the base application, maintaining consistency and avoiding naming collisions. For more information beyond this exercise on how the prefix `x_` is defined in the base application, have a look at the chapter [Partner Reference Application tutorial - Poetry Slam Manager Application with Extensibility](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/50-Multi-Tenancy-Features-Tenant-Extensibility.md#application-enablement).

2. To extend the service model, you enhance the existing service of the base application. The newly created entities of the data model are automatically included in a read-only manner. They serve as targets for the corresponding compositions. You can also explicitly expose them as writable.

    1. Create a file called `catererManagerService.cds` in the **/srv** folder, which includes all service definitions (cds-file of the service).  

    2. Copy the following content into the newly created file: 

        ```cds
        using { x_sap.samples.poetryslams.catering.x_Caterers as caterers } from '../db/catererManager';

        // Extend the PoetrySlamService of the base application with the X_Caterers entity.
        extend service PoetrySlamService with{
            entity x_Caterers as projection on caterers;
        }
        ```

3. Once you have defined the domain and service model, you extend UI annotations. You add a new **Caterer** section between the **General** and the **Visitors** sections using the `... up to` and `...` keywords. The following code demonstrates how to seamlessly insert a new UI facet between the existing facets in the base application, enhancing its functionality while preserving the integrity of the current layout:

   1. Create a file called `catererManager.cds` in the **/app** folder, which references all UI annotations (cds-file of the UI).
   2. Copy the content of the [catererManager.cds](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/app/catererManager.cds) of the Partner Reference Application Extension into the newly created file.
   3. Create a new folder called `i18n` in the **partner-reference-extension-catering** folder.
   3. Create new file called `i18n.properties` in the newly created i18n-folder to support translation.
   4. Copy the content of the [i18n.properties](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/i18n/i18n.properties) of the Partner Reference Application Extension into the newly created file.

   > Note: Extending UI annotations in SAP CAP Extension Development allows you to customize and enhance the user interface of a base application without altering its core functionality. Details beyond this exercise can be found in chapter [Extending UI Annotations](https://cap.cloud.sap/docs/guides/extensibility/customization#extending-ui-annotations) of the capire docmentation.

### Create an Initial Data Set

For demo purposes, you add an initial data set that's available after you've started the application. You can specify the data in SAP CAP by creating a set of CSV files in the **/db/data** folder of the project. 

To create the initial data set, follow these steps:

   1. Open a new terminal in your SAP Business Application Studio space (Control + Shift + C). 
   
   2. In the terminal, execute the following statement to create the files with the initial data in the **/db/data** folder:
      ```
      cds add data -f x_sap.samples.poetryslams
      ```
 
   3. Copy the data from [x_sap.samples.poetryslams.catering-x_CuisineTypeCodes.csv](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/db/data/x_sap.samples.poetryslams.catering-x_CuisineTypeCodes.csv) into the `db/data/x_sap.samples.poetryslams.catering-x_CuisineTypeCodes.csv` file. It adds the cuisine data to the entity.
   
   4. Copy the data from [x_sap.samples.poetryslams.catering-x_Caterers.csv](https://github.com/SAP-samples/partner-reference-application-extension/blob/main/partner-reference-extension-catering/db/data/x_sap.samples.poetryslams.catering-x_Caterers.csv) into the `db/data/x_sap.samples.poetryslams.catering-x_Caterers.csv` file. It adds the caterer data to the entity.

   5. Delete the file `x_sap.samples.poetryslams.catering-x_CuisineTypeCodes.texts.csv`. It is required for translation and not used in this exercise.

> Note: This adds caterer data for demonstration purposes. It's not suitable for a production environment. 

> Note: For more details beyond this exercise, refer to the [Partner Reference Application tutorial - Create an Initial Data Set](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/14-Develop-Core-Application.md#create-an-initial-data-set).

### Deploy the Extension
  
Now that you have added your extension code, you deploy it by pushing the code to your customer subaccount. This ensures that your extension code is isolated to your subaccount, preventing any impact on other customer subaccounts.

To push the extension code to the customer subaccount, use the statement from your notes:

1. Use the URL of the MTX module deployed in the provider subaccount. You noted it down as **ProviderMTXURL**.

2. Use the subdomain from the customer subaccount. You noted it down as **CustomerSubdomain**.

```bash
cds push --to <ProviderMTXURL> -s <CustomerSubdomain>
```

## Exercise 2.2 - Test the Extension

Now, a new **Caterer** section is introduced in the **Poetry Slam Manager** application within your customer subaccount, enabling you to effortlessly select caterers for your events.

1. Open the SAP Build Work Zone launchpad you created and enhanced with the extension. Use the URL **LaunchpadSiteURL** you noted down before.
2. Choose **Poetry Slam Manager** solution of your customer.
3. Open the **Poetry Slam Events** app.
4. Select a published poetry slam. You can see that the new **Caterer** section is available.
5. Choose **Edit** and select a caterer for your event from the predefined list. The poetry slam is updated with the caterer information.
6. Save your changes.

## Summary

You've now extended the **Poetry Slam Manager** of your customer with a **Caterer** section in which a caterer for the event can be selected.

Continue with [developing a SAP Fiori UI to manage the caterer entity](../ex3/README.md).
