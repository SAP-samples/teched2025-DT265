# Exercise 2 - Build and Deploy a Tenant Extension for Your Customer

In this exercise, you will create a tentant extension *Caterer* to select a caterer for a poetry slam in the Poetry Slam Manager of your customer.

> Note: Some steps are required to enable extensibility in the base application *Poetry Slam Manager*. You can get more details in chapter [Enable Consumer-Specific Extensions](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/50-Multi-Tenancy-Features-Tenant-Extensibility.md) of the Partner Reference Application.

## Exercise 2.1 - Extend the Persistence and the User Interface With Custom Fields

After completing these steps you will have extended the Poetry Slam Manager of your customer, with a section *Caterer* in which a caterer for the event can be selected.

Follow the steps from here: https://github.com/SAP-samples/partner-reference-application-extension/blob/main/Tutorials/02-DataModelExtensibility.md

## Exercise 2.2 - Develop a Fiori UI to Manage the Customer Entity

After completing these steps you will have created a Fiori Elements app for managing caterer for the poetry slam events.

Follow the steps from here: https://github.com/SAP-samples/partner-reference-application-extension/blob/main/Tutorials/03-FioriUIForExtendedEntity.md

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

6. Select a published poetry slams

7. You can see that the new *Caterer* section is available. Choose *Edit*, select the newly created caterer for your event from the predefined list.

8. The poetry slam is updated with the caterer information. Save your changes.

## Summary

You've now extended the Poetry Slam Manager subscription of your customer with the *Caterer* extension.

Continue to [Personalize the User Interface for All Users of Your Customer](../ex3/README.md)

