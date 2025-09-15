# DT265 - Provision and extend multitenant solutions for SAP Cloud ERP

## Description

This repository contains the material for the SAP TechEd 2025 session called DT265 - Provision and extend multitenant solutions for SAP Cloud ERP.  

## Overview

This session introduces attendees to experience how to provision, configure, and extend multi-tenant solutions for your customers. Using a reference example, you will learn how to deliver customer-specific extensions on a scalable and resource-efficient multi-tenant architecture.

Multi-tenant side-by-side extensions based on the SAP BTP empower partners and customers to innovate on top of Cloud ERP and Cloud ERP Private in a scalable model. Building applications with SAP Build and running solutions on the SAP BTP means you can focus on your application domain and inherit enterprise-class SaaS qualities such as multitenancy, easy customer onboarding, seamless integration with the SAP Business Suite, and out-of-the-box extensibility.
 
Imaging you are working for "SLAM-Tech Solutions", an SAP Build partner with focus on speed, impact and bold execution in the tech space. SLAM-Tech Solutions offers a SaaS solution that extends SAP Cloud ERP with 40 new customers that need to be onboarded quickly. 
As participant of this hands-on session you serve one of those 40 customers and it will be your job to onboard your customer incl. provisioning a tenant, configuring the connection to Cloud ERP, and enhance to the standard solution fulfilling the wish list of your customer.

## Requirements

The requirements to follow the exercises in this repository are:

- SAP BTP Global Account
- SAP Business Application Studio
- SAP S/4HANA Public Cloud instance
- Provider Subaccount with the Partner Reference Application provided
- Subscriber Subaccount with IDP... ?

## Exercises

Provide the exercise content here directly in README.md using [markdown](https://guides.github.com/features/mastering-markdown/) and linking to the specific exercise pages, below is an example.

- [Getting Started](exercises/ex0/)
    Get familar with the development environment and the Subscriber SAP BTP Subaccount of the customer you serve.
    *Have a look at entitlements*
- [Exercise 1 - Provision your multi-tenant solution to your customer](exercises/ex1/)
    - [Exercise 1.1 - Subscribe the solution in the application subscription of your customer](exercises/ex1#exercise-11-sub-exercise-1-description)
    - [Exercise 1.2 - Configure the connection to the Cloud ERP](exercises/ex1#exercise-12-sub-exercise-2-description)
    - [Exercise 1.3 - Configure SAP Build Work Zone](exercises/ex1#exercise-12-sub-exercise-2-description)
- [Exercise 2 - Build a tenant extension for your customer](exercises/ex2/)
    - [Exercise 2.1 - Extend the persistence and the user interface with custom fields](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 2.2 - Develop a Fiori UI to manage the customer entity](exercises/ex2#exercise-22-sub-exercise-2-description)
- [Exercise 3 - Deploy your extension to the application subscription of your customer](exercises/ex3/)
- [Exercise 4 - Test the application subscription including the integration with Cloud ERP and the customer-specific extension](exercises/ex4/)
- [Exercise 5 (optional) - Test the application subscription including the integration with Cloud ERP and the customer-specific extension](exercises/ex5/)
    - [Exercise 5.1 - Enable key-user UI flexibility](exercises/ex5#exercise-51-sub-exercise-1-description)
    - [Exercise 5.2 - Adopt the UI according your customer preferences and publish the changes to all users](exercises/ex5#exercise-52-sub-exercise-5-description)

**OR** Link to the Tutorial Navigator for example...

Start the exercises [here](https://developers.sap.com/tutorials/abap-environment-trial-onboarding.html).

**IMPORTANT**

Your repo must contain the .reuse and LICENSES folder and the License section below. DO NOT REMOVE the section or folders/files. Also, remove all unused template assets(images, folders, etc) from the exercises folder. 

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (c) 2024 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
