# DT265 - Provision and Extend Multi-Tenant Solutions Based on CAP for SAP Cloud ERP

## Description

This repository contains the material for the SAP TechEd 2025 session called DT265 - Provision and extend multitenant solutions for SAP Cloud ERP.  

## Overview

This session introduces attendees to experience how to provision, configure, and extend multi-tenant solutions for your customers. Using a reference example, you will learn how to deliver customer-specific extensions on a scalable and resource-efficient multi-tenant architecture.

Multi-tenant side-by-side extensions based on the SAP BTP empower partners and customers to innovate on top of Cloud ERP and Cloud ERP Private in a scalable model. Building applications with SAP Build and running solutions on the SAP BTP means you can focus on your application domain and inherit enterprise-class SaaS qualities such as multitenancy, easy customer onboarding, seamless integration with the SAP Business Suite, and out-of-the-box extensibility.
 
Imaging you are working for "SLAM-Tech Solutions", an SAP Build partner with focus on speed, impact and bold execution in the tech space. SLAM-Tech Solutions offers a SaaS solution that extends SAP Cloud ERP with 40 new customers that need to be onboarded quickly. 
As participant of this hands-on session you serve one of those 40 customers and it will be your job to onboard your customer incl. provisioning a tenant, configuring the connection to Cloud ERP, and enhance to the standard solution fulfilling the wish list of your customer.

> Note: This session is done on basis of the [Partner Reference Application](https://github.com/SAP-samples/partner-reference-application) and the [Partner Reference Application Extension](https://github.com/SAP-samples/partner-reference-application-extension).

## Requirements

The requirements to follow the exercises in this repository are:

- SAP BTP Global Account
- SAP Business Application Studio
- SAP S/4HANA Public Cloud instance
- Provider Subaccount with the Partner Reference Application provided
- Subscriber Subaccount for each customer with trust configuration

## Exercises

- [Getting Started](exercises/ex0/README.md)
    - [Get an Overview Provider SAP BTP Subaccount](exercises/ex0/README.md#optional-get-an-overview-provider-sap-btp-subaccount)
    - [Get an Overview of the SAP BTP Subaccount of your Customer](exercises/ex0/README.md#get-an-overview-of-the-sap-btp-subaccount-of-your-customer)
    - [Configure the Development Environment](exercises/ex0/README.md#configure-the-development-environment)

- [Exercise 1 - Provision Your Multi-tenant Solution to Your Customer](exercises/ex1/README.md)
    - [Exercise 1.1 - Subscribe the Solution in the Application Subscription of Your Customer](exercises/ex1#exercise-11---subscribe-the-solution-in-the-application-subscription-of-your-customer)
    - [Exercise 1.2 - Configure SAP Build Work Zone](exercises/ex1#exercise-12---configure-sap-build-work-zone)
    - [Exercise 1.3 - Configure the Connection to SAP S/4HANA Cloud Public Edition](exercises/ex1#exercise-13---configure-the-connection-to-sap-s4hana-cloud-public-edition)

- [Exercise 2 - Build and Deploy a Tenant Extension for Your Customer](exercises/ex2/README.md)
    - [Exercise 2.1 - Extend the Persistence and the User Interface With Custom Fields](exercises/ex2#exercise-21---extend-the-persistence-and-the-user-interface-with-custom-fields)
    - [Exercise 2.2 - Develop a Fiori UI to Manage the Customer Entity](exercises/ex2#exercise-22---develop-a-fiori-ui-to-manage-the-customer-entity)
    - [Test the Extension](exercises/ex2/README.md#test-the-extension)
    
- [Exercise 3 (optional) - Personalize the User Interface for All Users of Your Customer](exercises/ex3/)


## Contributing

Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct

Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License

Copyright (c) 2024 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
