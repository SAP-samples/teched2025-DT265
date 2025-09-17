# Getting Started

In this chapter, you will get familar with the development environment and the subscriber SAP BTP subaccount of the customer you serve.

## Get an Overview of the SAP BTP Subaccount of your Customer

After completing these steps you will have an overview about the subscriber SAP BTP subaccount of your customer.

1. Login to the SAP BTP subaccount of your customer.
2. Navigate to *Services* -> *Instances and Subscriptions*.
3. View the subscriptions. 
    - *Explain what is there and why*
4. View the instances. 
     - *Explain what is there and why*
5. Navigate to *Destinations*.
6. View the existing destinations.
     - *Explain what is there and why*
7. Navigate to *Trust Configuration*.
8. View the existing configurations.
     - *Explain what is there and why*

    - *View the entitlements, too????*

## Configure the Development Environment

After completing these steps you will have accessed and configured the SAP Business Application Studio to develop your customer-specific extension.

1. Login to the SAP BTP subaccount of your customer.
2. Navigate to *Services* -> *Instances and Subscriptions*.
3. Open the *SAP Business Application Studio*.
4. Create a new dev space.
5. Enter a *Dev Space name*, for example *PartnerReferenceApplicationExtension*.
6. Select *Full Stack Cloud Application*.
7. Create the dev space. The dev space is now in status *STARTING*. You can move on as soon the status changes to *RUNNING*.
8. Open the *PartnerReferenceApplicationExtension* dev space.

> Note: The preconditions for these steps are described in more detail in chapter [Prepare Your SAP BTP Account for Development](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/11-Prepare-BTP-Account.md) of the Partner Reference Application.


## (Optional) Get an Overview Provider SAP BTP Subaccount

After completing these steps you will have an overview about the provider SAP BTP subaccount.

1. Switch to the provider subaccount in the SAP BTP Cockpit.
2. Navigate to *Services* -> *Instances and Subscriptions*.
3. View the subscriptions. 
    - *Explain what is there and why*
4. View the instances. 
     - *Explain what is there and why*
5. Navigate to *Destinations*.
6. View the existing destinations.
     - *Explain what is there and why*
7. Navigate to *Trust Configuration*.
8. View the existing configurations.
     - *Explain what is there and why*

> Note: The configuration of the provider subaccount is described in more detail in chapter [Bill of Materials](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/01-BillOfMaterials.md) of the Partner Reference Application.

## Summary

Now that you have on overview of the SAP BTP subaccounts and configured your SAP Business Application Studio as development environment, continue to [Provision Your Multi-tenant Solution to Your Customer](../ex1/README.md)
