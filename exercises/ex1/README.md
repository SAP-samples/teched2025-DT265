# Exercise 1 - Provision Your Multi-tenant Solution to Your Customer

In this exercise, you will create a subscription of your multi-tenant solution to the SAP BTP subaccount of your customer.

## Exercise 1.1 - Subscribe the Solution in the Application Subscription of Your Customer

After completing these steps you will have subscribed the SAP BTP Multi-Tenant Application *Poetry Slam Manager* in the SAP BTP subaccount of your customer.

To provision the application on the subaccount for a specific customer, subscribe to the application:

1. In the SAP BTP cockpit of the subaccount of your Customer, navigate to *Instances and Subscriptions*.
2. Create a subscription to *Poetry Slam Manager* with service plan default. This is the multi-tenant SAP BTP application that is provided in the provider subaccount.

## Exercise 1.2 - Configure SAP Build Work Zone

After completing these steps you will have configured SAP Build Work Zone to access the subscribed application *Poetry Slam Manager*. For SAP Build Work Zone, destinations are required to access the provided solution of the provider subaccount. Additional configuration needs to be done to create the launchpad.

### Create Destinations to Access the HTML5 Business Solutions of the Provider Subaccount

The *Poetry Slam Manager* is a content provider and offers content using the common data model. Besides this, it creates destinations to access the provided content. In the customer subaccount, destinations for the design-time and the runtime of the provided content are required, too. Therefore, export the required subaccount destinations from the provider subaccount and import them to the consumer subaccount.

1. Export the destinations from the provider subaccount.

	1. Open the SAP BTP cockpit of the provider subaccount.
	2. Navigate to the *Destinations* view.
	3. Adapt the destinations in landscape eu10. This step is necessary because the HTML5 repository and SAP Build Work Zone services are hosted in eu10, while the application runs in eu10-004.
		1. In the *poetry-slams-rt* destination, remove *-004* from the *URL*. The last part of the URL should be: *launchpad.cfapps.eu10.hana.ondemand.com*.
		2. In the *poetry-slams-cdm* destination, remove *-004* from the *URL*, the *Token Service URL*, and the additional property *uri*. It should be: *cfapps.eu10.hana.ondemand.com*.
	4. Select the *poetry-slams-cdm* destination and export it. A *poetry-slams-cdm* file is stored on your computer.
	5. Select the *poetry-slams-rt* destination and export it. A *poetry-slams-rt* file is stored on your computer.
	6. Do not close the provider cockpit, as you'll need it for the next steps.

2. Import the destinations in your customer subaccount.
	1. Open the SAP BTP cockpit of your customer.
	2. Navigate to the *Destinations* view.
	3. Import the *poetry-slams-cdm* destination using the downloaded file. The values of the destination are taken over. The value of the *Client Secret* property can't be exported but you can get it as follows:

		1. Navigate to *Instances and Subscriptions* of the provider subaccount.
		2. Select the *poetry-slams-html5-runtime* service instance in the table.
		3. Choose *View Credentials* at the top right corner of the screen.
		4. From the Credentials file that opens, copy the value of the clientsecret.
		5. Navigate back to your design-time destination and add the value you just copied to the Client Secret property.

	4. Import the *poetry-slams-rt* destination using the downloaded file.
	5. Replace the URL with `https://<customer account subdomain>.launchpad.cfapps.eu10.hana.ondemand.com`.

### Configure SAP Build Work Zone

Since the web application and the destinations for the provided content are available, it's ready to be added to SAP Build Work Zone.

#### Configure the Content Channel of Your Web Application

1. In the SAP BTP Cockpit of your customer subaccount, goto *Instances and Subscriptions*.

2. Open *SAP Build Work Zone, standard edition*.
	
	> Note: SAP Build Work Zone is already subscribed in the subaccount of your customer. Additionally, your user is assigned to the role collection *Launchpad_Admin*, which allows you to configure SAP Build Work Zone. You can find more details about the predconditions in the chapter [Provision Your Multi-Tenant Application to Consumer Subaccounts](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/25-Multi-Tenancy-Provisioning.md) of the Partner Reference Application.

3. In the *Site Manager*, open the *Channel Manager*. As the web application is deployed in the provider subaccount, it is not automatically added as content to the *HTML5 Apps* content channel.

4. Create a new *Content Provider*. 
	1. Set the title *Poetry Slam Manager*.
	2. Set the description *Content of Poetry Slam Manager Solution*.
	3. Select *poetry-slams-cdm* as *Design-Time-Destination*.
	4. Select *poetry-slams-rt* as *Runtime-Destination*.
	5. Save the content channel.
	6. Use *Report* to see the created elements of your channel.
	 > Note: You must update the content channel every time you made changes to the web application. It may take some time to reflect the changes.

#### Review the Created Content of Your Web Application

1. Open the *Content Manager*.
2. The *Poetry Slam Manager Role* and the *Poetry Slam Visitor Role* were automatically created. The names in the *ID* fields are the name of the role collections that are created in the SAP BTP cockpit of the consumer account that handles the access for the application content.
3. Select one of the roles.
4. The *Poetry Slams* and the *Visitors* apps are automatically assigned. These were defined in the provided application.

#### Create a Launchpad Site

In this step, you create and review a launchpad site. 

1. Open the *Site Directory*. 
2. Create a site and enter a site name, for example, *Partner Reference Application*.
3. Edit the newly created site.
4. In the *Assignments* area on the right side of the screen, click into the search field. The *Poetry Slam Manager Role* should show up as a result.
5. Choose the plus behind the *Poetry Slam Manager Role*.
6. Choose the plus behind the *Poetry Slam Visitor Role*.
7. Save the changes.
8. To launch the site, open the *URL* provided in the *Properties* of the *Site Settings*. On the site, you can see no tiles yet. Before being able to see the *Poetry Slams* and *Visitors* tiles, you need to set up the authorizations roles.

#### Configure Authorization Roles 

In the SAP BTP subaccount of your customer, open the menu item *Role Collections* and edit the role collections that were created by SAP Build Work Zone, for example, ~poetry_slam_manager_poetrySlamManagerRole. 

For each role collection, add the role from the reference application and add your user of the identity provider.

| Role Collection                    				| Role		  			| User ID         					| User Identity Provider         	|
| :---                               				| :---					| :---			       				| :---			       				|
| `~poetry_slam_manager_poetrySlamManagerRole` 		| PoetrySlamManagerRole | `DT265-0XX@education.cloud.sap`	| Custom IAS tenant					|
| `~poetry_slam_manager_poetrySlamVisitorRole`		| PoetrySlamVisitorRole | `DT265-0XX@education.cloud.sap`  	| Custom IAS tenant					|

> Note: The trust configuration is already setup. In case you want to learn more about the preconditions to use the identity authentication service, check the chapter [Configure Authentication and Authorization](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/25-Multi-Tenancy-Provisioning.md#configure-authentication-and-authorization) of the partner reference application.

### Test the Poetry Slam Manager with SAP Build Work Zone

Launch the SAP Build Work Zone site with the URL you noted down during the launchpad site creation. Choose the *Manage Poetry Slams* tile. The application comes up.

## Exercise 1.3 - Configure the Connection to SAP S/4HANA Cloud Public Edition

After completing these steps you will have configured the connection to SAP S/4HANA Cloud Public Edition. The Poetry Slam Manager integrates SAP S/4HANA Cloud Public Edition to plan and staff poetry slam events, to collect costs, and to purchase required equipments. 

> Note: The configuration in the SAP S/4HANA Cloud Public Edition instance is already setup. In case you want to learn more about the setup, check the chapter [Configure the Integration with SAP S/4HANA Cloud Public Edition](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/34b-Multi-Tenancy-Provisioning-Connect-S4HC.md) of the Partner Reference Application.

### Set Up Destinations to Connect the SAP BTP Application to SAP S/4HANA Cloud Public Edition

In this section, three destinations are created to access SAP S/4HANA Cloud OData services:
- Destination **s4hc** to consume SAP S/4HANA Cloud OData services. In this session, basic authentication is used due to simpler demo use case. In the Partner Reference Application, the setup with principal propagation is described..
- Destination **s4hc-tech-user** to consume SAP S/4HANA Cloud OData services using a technical basic authentication.
- Destination **s4hc-url** to provide the SAP S/4HANA Cloud hostname of UI navigations and the name of the SAP S/4HANA Cloud Public Edition system as used by business users.

1. In the SAP BTP customer subaccount, you can create the destination *s4hc* to consume SAP S/4HANA Cloud OData services. 
    1. Go to *Connectivity* in the SAP BTP consumer subaccount.
    2. Choose *Destinations*.
    3. Create a *New Destination* with the following field values:

        | Parameter Name            | Value                                                                                         |
        | :------------------------ | :-------------------------------------------------------------------------------------------- |
        | *Name*:                   | *s4hc*                                                                                        |
        | *Type*:                   | *HTTP*                                                                                        |
        | *Description*:            | Destination description (For example, `SAP S/4HANA Cloud XXXXXX with principal propagation`)  |
        | *URL*:                    | **API-URL** of the *Communication Arrangement*                                        |
        | *Proxy Type*:             | *Internet*                                                                            |
        | *Authentication*:         | *BasicAuthentication*                                                                 |
        | *User*:                   | **User Name** of the *Communication User*                                             |
        | *Password*:               | **Password** of the *Communication User*                                              |

2. Create the *s4hc-tech-user* destination to consume SAP S/4HANA Cloud OData services using a technical communication user.

    In the SAP BTP consumer subaccount, go to *Connectivity* and choose *Destinations* to create a *New Destination* with the following field values:

    | Parameter Name    | Value                                                                                 |
    | :---------------- | :------------------------------------------------------------------------------------ |
    | *Name*:           | *s4hc-tech-user*                                                                      |
    | *Type*:           | *HTTP*                                                                                |
    | *Description*:    | Destination description (For example, `SAP S/4HANA Cloud XXXXXX with technical user`) |
    | *URL*:            | **API-URL** of the *Communication Arrangement*                                        |
    | *Proxy Type*:     | *Internet*                                                                            |
    | *Authentication*: | *BasicAuthentication*                                                                 |
    | *User*:           | **User Name** of the *Communication User*                                             |
    | *Password*:       | **Password** of the *Communication User*                                              |

3. Create the *s4hc-url* destination to launch SAP S/4HANA Cloud Public Edition apps and to store the name of the SAP S/4HANA Cloud Public Edition system used by business users. 

    In the SAP BTP consumer subaccount, go to *Connectivity* and choose *Destinations* to create a *New Destination* with the following field values:

    | Parameter Name    | Value                                                                                             |
    | :---------------- | :------------------------------------------------------------------------------------------------ |
    | *Name*:           | *s4hc-url*                                                                                        |
    | *Type*:           | *HTTP*                                                                                            |
    | *Description*:    | Destination description (For example, `SAP S/4HANA Cloud XXXXXX`)                                 |
    | *URL*:            | UI endpoint of the SAP S/4HANA Cloud Public Edition system (For example, `https://myXXXXXX.s4hana.ondemand.com`) |
    | *Proxy Type*:     | *Internet*                                                                                        |
    | *Authentication*: | *NoAuthentication*                                                                                |


### Test the Integration of the Poetry Slam Manager with SAP S/4HANA Public Cloud

 Your customer uses SAP S/4HANA Public Cloud projects to plan and staff events, to collect costs, and to purchase required equipments. Therefore, the Poetry Slam Manager is connected to the SAP S/4HANA Public Cloud projects to plan the event. To test this integration follow these steps:

1. Open the Poetry Slam Manager application of your customer with SAP Build Work Zone.

2. In the Poetry Slams application, an empty list is displayed.

3. To create sample data for mutable data, such as poetry slams, visitors, and visits, choose *Generate Sample Data*. As a result, multiple poetry slams are listed: Some are still in preparation and have not been released yet while others are already published. 
    > Note: If you choose *Generate Sample Data* again, the sample data is set to the default values.

4. To view the details of a poetry slam, choose one that is neither canceled nor in draft mode. 

5. Choose *Create Project in SAP S/4HANA Public Cloud*. As a result, the system creates a project in SAP S/4HANA Public Cloud based on a project template.

6. After creating the project, you can see the project details in the *Project Data* section. The project ID is displayed as a link. To go to the project overview, click the project ID.

## Summary

You've now provisioned your *Poetry Slam Manager* solution to the SAP BTP subaccount of your customer and configured the SAP BTP subaccount of your customer, continue to [Build a Tenant Extension for Your Customer](../ex2/README.md)
