# DT265 - Provision and Extend Multi-Tenant Solutions Based on CAP for SAP Cloud ERP

## Description

This repository contains the material for the SAP TechEd 2025 session called DT265 - Provision and extend multi-tenant solutions based on CAP for SAP Cloud ERP.  

## Overview

In this session, you experience how to provision, configure, and extend multi-tenant solutions for your customers. Using a reference example, you learn how to deliver customer-specific extensions on a scalable and resource-efficient multi-tenant architecture.

Multi-tenant side-by-side extensions based on the SAP BTP empower partners and customers to innovate on top of Cloud ERP in a scalable model. Building applications with SAP Build and running solutions on the SAP BTP lets you focus on your application domain. You inherit enterprise-class SaaS qualities such as multitenancy, easy customer onboarding, seamless integration with the SAP Business Suite, and out-of-the-box extensibility.
 
Imagine you are working for **SLAM-Tech Solutions**, a SAP Build partner that focuses on speed, impact, and bold execution in the tech space. SLAM-Tech Solutions offers a SaaS solution that extends SAP Cloud ERP. There are 40 new customers that need quick onboarding. 
As a participant of this hands-on session, you serve one of those 40 customers. Your job is to onboard your customer, which includes provisioning a tenant, configuring the connection to Cloud ERP, and enhancing the standard solution to fulfill your customers wish list.

> Note: This session is based on the [Partner Reference Application](https://github.com/SAP-samples/partner-reference-application) and the [Partner Reference Application Extension](https://github.com/SAP-samples/partner-reference-application-extension).

## Requirements

To follow the exercises in this repository, you need:

- SAP BTP global account
- SAP Business Application Studio
- SAP S/4HANA Cloud Public Edition instance
- SAP BTP provider subaccount with the Partner Reference Application provided
- SAP BTP subaccount for each customer with trust configuration 

## Exercises

- [Getting Started](exercises/ex0/README.md)
    - [Preparation for the Exercises](exercises/ex0/README.md#preparation-for-the-exercises)
    - [Get an Overview of the System Lanscape](exercises/ex0/README.md#get-an-overview-of-the-system-lanscape)
    - [Get an Overview of the SAP BTP Provider Subaccount](exercises/ex0/README.md#get-an-overview-of-the-sap-btp-provider-subaccount)
    - [Get an Overview of the SAP BTP Subaccount of Your Customer](exercises/ex0/README.md#get-an-overview-of-the-sap-btp-subaccount-of-your-customer)
    - [Configure the Development Environment SAP Business Application Studio](exercises/ex0/README.md#configure-the-development-environment)

- [Exercise 1 - Provision Your Multi-Tenant Solution to Your Customer](exercises/ex1/README.md)
    - [Exercise 1.1 - Subscribe the Solution in the Application Subscription of Your Customer](exercises/ex1/README.md#exercise-11---subscribe-the-solution-in-the-application-subscription-of-your-customer)
    - [Exercise 1.2 - Configure SAP Build Work Zone](exercises/ex1/README.md#exercise-12---configure-sap-build-work-zone)

- [Exercise 2 - Extend the Poetry Slam Manager with a Customer-Specific Extension](exercises/ex2/README.md)
    - [Exercise 2.1 - Extend the Persistence and the User Interface With Custom Fields](exercises/ex2/README.md#exercise-21---extend-the-persistence-and-the-user-interface-with-custom-fields)
    - [Exercise 2.2 - Test the Extension](exercises/ex2/README.md#exercise-22---test-the-extension)

- [Exercise 3 - Build and Deploy a Customer-Specific Extension to Manage the Caterer Entity](exercises/ex3/README.md)
    - [Exercise 3.1 - Develop a SAP Fiori UI to Manage the Caterer Entity](exercises/ex3/README.md#exercise-31---develop-a-sap-fiori-ui-to-manage-the-caterer-entity)
    - [Exercise 3.2 - Test the Extension](exercises/ex3/README.md#exercise-32---test-the-extension)
    
- [Exercise 4 (optional) - Personalize the User Interface for All Users of Your Customer](exercises/ex4/README.md)

## Summary

**You provided a subscription:**
- You created a subscription within minutes and configured the customers launchpad.
- SAP manages the tenant lifecycle, ensures isolation of customer data, provides a role-based identity access management and secure connections to customer ERP systems, …
- The subscription has almost no direct cost footprint; costs are caused by usage only! 

**You complemented the subscription by a customer-specific extension:**
- Your main tasks have been to extend the business model and the UI content.
- SAP blends the extension with the main app and manages the isolation of the extension, such that only this one customer can see and use the extension. 
- The extension has a zero direct cost footprint.

## Contributing

Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct

Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to Get Support

Support for the content in this repository is available during the scheduled time of the online session for which this content is designed. Outside of this session, you can request support through the [Issues](../../issues) tab.

## License

Copyright (c) 2024 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
