# Exercise 1 - Provision Your Multi-tenant Solution to Your Customer

In this exercise, you will create a subscription of your multi-tenant solution to the SAP BTP subaccount of your customer.

## Exercise 1.1 - Subscribe the Solution in the Application Subscription of Your Customer

After completing these steps you will have subscribed the SAP BTP Multi-Tenant application *Poetry Slam Manager* in the SAP BTP subaccount of your customer.

To provision the application on the subaccount for a specific customer, subscribe to the *Poetry Slam Manager* application:

1. In the SAP BTP cockpit of the subaccount of your Customer, navigate to *Instances and Subscriptions*.
2. Create a subscription to *Poetry Slam Manager*.
3. Select the service plan *default*. This is the multi-tenant SAP BTP application that is provided in the provider subaccount.

## Exercise 1.2 - Configure SAP Build Work Zone

After completing these steps you will have configured SAP Build Work Zone to access the subscribed application *Poetry Slam Manager*. For SAP Build Work Zone, destinations are required to access the provided solution of the provider subaccount. Additional configuration needs to be done to create the launchpad.

### Configure SAP Build Work Zone

Since the web application and the destinations for the provided content are available, it's ready to be added to SAP Build Work Zone.

#### Configure the Content Channel of Your Web Application

1. In the SAP BTP Cockpit of your customer subaccount, go to *Instances and Subscriptions*.
2. Open *SAP Build Work Zone, standard edition*.
3. In the *Site Manager*, open the *Channel Manager*. As the web application is deployed in the provider subaccount, it is not automatically added as content to the *HTML5 Apps* content channel.
4. Create a new *Content Provider*. 
	1. Set the title *Poetry Slam Manager*.
	2. Set the description *Content of *Poetry Slam Manager* Solution*.
	3. Select *poetry-slams-cdm* as *Design-Time-Destination*.
	4. Select *poetry-slams-rt* as *Runtime-Destination*.
	5. Save the content channel.
	6. Use *Report* to see the created elements of your channel.
	 > Note: You must update the content channel every time you made changes to the web application. It may take some time to reflect the changes.

#### Review the Created Content of Your Web Application

1. Open the *Content Manager*.
2. The **Poetry Slam Manager* Role* and the *Poetry Slam Visitor Role* were automatically created. The names in the *ID* fields are the name of the role collections that are created in the SAP BTP cockpit of the consumer account that handles the access for the application content.
3. Select one of the roles.
4. The *Poetry Slams* and the *Visitors* apps are automatically assigned. These were defined in the provided *Poetry Slam Manager* application.

#### Create a Launchpad Site

In this step, you create and review a launchpad site. 

1. Open the *Site Directory*. 
2. Create a site and enter a site name, for example, *Partner Reference Application*.
3. Open the *Role Assignments* area on the left side of the screen.
4. Edit the role assignments.
5. Click into the search field. The *Poetry Slam Manager Role* should show up as a result.
6. Enable the assignment status of the role *Poetry Slam Manager Role*.
7. Enable the assignment status of the role *Poetry Slam Visitor Role*.
8. Save the changes.
9. To launch the site, open the *URL* provided in the *Properties* of the *Site Settings*. Note the URL of the *Poetry Slam Manager* launchpad site for later use (**launchpad site URL**). On the site, you can see no tiles yet. Before being able to see the *Poetry Slams* and *Visitors* tiles, you need to set up the authorizations roles. 

#### Configure Authorization Roles 

In the SAP BTP subaccount of your customer, open the menu item *Role Collections* and edit the role collections that were created by SAP Build Work Zone, for example, ~poetry_slam_manager_poetrySlamManagerRole. 

For each role collection, add the role from the Partner Reference Application and add your user of the identity provider.

| Role Collection                    				| Role		  			| User ID         					| User Identity Provider         	|
| :---                               				| :---					| :---			       				| :---			       				|
| `~poetry_slam_manager_poetrySlamManagerRole` 		| PoetrySlamManagerRole | `DT265-0*nn*@education.cloud.sap`	| Custom IAS tenant					|
| `~poetry_slam_manager_poetrySlamVisitorRole`		| PoetrySlamVisitorRole | `DT265-0*nn*@education.cloud.sap` | Custom IAS tenant					|

Authorization changes are not directly taken into account. The user needs to re-login. Therefore, log off from the SAP BTP subaccount and login again.

> Note: The trust configuration is already setup. In case you want to learn more about the preconditions to use the identity authentication service, check the chapter [Configure Authentication and Authorization](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/25-Multi-Tenancy-Provisioning.md#configure-authentication-and-authorization) of the Partner Reference Application.

### Test the Poetry Slam Manager with SAP Build Work Zone and the Integration with SAP S/4HANA Public Cloud

Your customer can access *Poetry Slam Manager* with the SAP Build Work Zone launchpad. They use SAP S/4HANA Public Cloud projects to plan and staff events, to collect costs, and to purchase required equipments. Therefore, the *Poetry Slam Manager* is connected to the SAP S/4HANA Public Cloud projects to plan the event. To test this integration follow these steps:

1. Launch the SAP Build Work Zone site with the URL **launchpad site URL**, you noted during the launchpad site creation. 
2. Choose the *Poetry Slam Events* tile. The application comes up and shows an empty list page.
3. To create sample data for mutable data, such as poetry slams, visitors, and visits, choose *Generate Sample Data*. As a result, multiple poetry slams are listed: Some are still in preparation and have not been released yet while others are already published. 

    > Note: If you choose *Generate Sample Data* again, the sample data is set to the default values.

4. To view the details of a poetry slam, choose one that is neither canceled nor in draft mode. 
5. Choose *Create Project in SAP S/4HANA Public Cloud*. As a result, the system creates a project in SAP S/4HANA Public Cloud based on a project template.
6. After creating the project, you can see the project details in the *Project Data* section. The project ID is displayed as a link. To go to the project overview, click the project ID.

## Summary

You've now provisioned your *Poetry Slam Manager* solution to the SAP BTP subaccount of your customer and configured the SAP BTP subaccount of your customer, continue to [Build a Customer-Specific Extension for Your Customer](../ex2/README.md)
