# Exercise 1 - Provision Your Multi-Tenant Solution to Your Customer

In this exercise, you create a subscription of your multi-tenant solution to the SAP BTP subaccount of your customer.

## Exercise 1.1 - Subscribe the Solution in the Application Subscription of Your Customer

After completing these steps, you have subscribed the SAP BTP multi-tenant application **Poetry Slam Manager** in the SAP BTP subaccount of your customer.

To provision the application on the subaccount for a specific customer, subscribe to the **Poetry Slam Manager** application:

1. In the SAP BTP cockpit of the **customer subaccount**, navigate to **Services** -> **Instances and Subscriptions**.
2. Create a subscription to **Poetry Slam Manager**.
3. Choose the **default** service plan. This is the multi-tenant SAP BTP application that is provided in the provider subaccount.

## Exercise 1.2 - Configure SAP Build Work Zone

After completing these steps, you have configured SAP Build Work Zone to access the subscribed application **Poetry Slam Manager**. 

### Configure SAP Build Work Zone

Since the web application and the destinations for the provided content are available, it's ready to be added to SAP Build Work Zone.

#### Configure the Content Channel of Your Web Application

1. In the SAP BTP Cockpit of the **customer subaccount**, go to **Services** -> **Instances and Subscriptions**.
2. Open **SAP Build Work Zone, standard edition**.
3. In the **Site Manager**, open the **Channel Manager**. As the web application is deployed in the provider subaccount, it's not automatically added as content to the **HTML5 Apps** content channel.
4. Create a new **Content Provider**. 
	1. As title, set *Poetry Slam Manager*.
	2. As description, set *Content of *Poetry Slam Manager* Solution*.
	3. As **Design-Time-Destination**, select *poetry-slams-cdm*.
	4. As **Runtime-Destination**, select *poetry-slams-rt*.
	5. Save the content channel.
	6. Refresh the page to see when the channel is successfully created.
	7. Use *Report* to see the created elements of your channel.
	 
		> Note: In case the creation fails, update the credentials of the destination *poetry-slams-cdm*. How the destination is created and where the credentials can be found, is described in [Create Destinations to Access the HTML5 Business Solutions of the Provider Subaccount](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/25-Multi-Tenancy-Provisioning.md#create-destinations-to-access-the-html5-business-solutions-of-the-provider-subaccount).

#### Review the Created Content of Your Web Application

1. Open the **Content Manager**.
2. The **Poetry Slam Manager Role** and the **Poetry Slam Visitor Role** were automatically created. The names in the **ID** fields are the names of the role collections that are created in the SAP BTP cockpit of the consumer account that handles the access for the application content.
3. Access one of the roles to view the assigned apps.
4. The **Poetry Slams** and the **Visitors** apps are automatically assigned. These were defined in the provided **Poetry Slam Manager** application.

> Note: In the **Poetry Slam Manager**, the **Poetry Slam Visitor Role** is used to handle read-only access. In this exercise, the role will not be configured and used.

#### Create a Launchpad Site

In this step, you create and review a launchpad site. 

1. Open the **Site Directory**. 
2. Create a site and enter a site name, for example, *Partner Reference Application*.
3. Open the **Role Assignments** area on the left side of the screen.
4. Edit the role assignments.
5. Enable the assignment status of the **Poetry Slam Manager Role**.
6. Save the changes.
7. To launch the site, open the **URL** provided in the **Properties** of the **Site Settings**. Note down the URL of the **Poetry Slam Manager** launchpad site for later us (**LaunchpadSiteURL**). On the site, you can't see any tiles yet. Before you can see the **Poetry Slams** and **Visitors** tiles, you need to set up the authorization roles. 

#### Configure Authorization Roles 

1. In the SAP BTP subaccount of your customer, go to **Security** -> **Role Collections**.
2. Edit the role collection `~poetry_slam_manager_poetrySlamManagerRole` that was created by SAP Build Work Zone. 
3. In section **Roles**, use the value help of the field **Role Name** to select the role `PoetrySlamManagerRole`.
4. Add the role to the role collection.
5. In section **Users**, start typing `DT265` in the **ID** field.
6. Select your user (`DT265-0*nn*@education.cloud.sap`) from the **Identity Provider** `Custom IAS tenant`. Replace *nn* with your customer number. 
7. Click the `+`-button.
8. Save the changes.

> Note: The trust configuration is already set up. In case you want to learn more about the preconditions to use the identity authentication service, check the chapter [Configure Authentication and Authorization](https://github.com/SAP-samples/partner-reference-application/blob/main/Tutorials/25-Multi-Tenancy-Provisioning.md#configure-authentication-and-authorization) of the Partner Reference Application.

### Test the Poetry Slam Manager with SAP Build Work Zone and the Integration with SAP S/4HANA Cloud Public Edition

Your customer can access the **Poetry Slam Manager** with the SAP Build Work Zone launchpad. They use SAP S/4HANA Cloud Public Edition projects to plan and staff events, to collect costs, and to purchase required equipments. Therefore, the **Poetry Slam Manager** is connected to the SAP S/4HANA Cloud Public Edition projects to plan the event. To test this integration, follow these steps:

1. Launch the SAP Build Work Zone site with the URL **LaunchpadSiteURL**, you noted during the launchpad site creation. 
	> Note: **Authorization changes aren't directly considered. You need to re-login. Log off from the application and then log in again.**
2. Choose the **Poetry Slam Events** tile. The application comes up and shows an empty list page.
3. To create sample data for mutable data types like poetry slams, visitors, and visits, choose **Generate Sample Data**. This action lists multiple poetry slams. Some are still in preparation and haven't been released yet, while others are already published.

    > Note: If you choose **Generate Sample Data** again, the sample data is set to the default values.

4. To view the details of a poetry slam, choose one that is neither canceled nor in draft mode. 
5. Choose **Create Project in SAP S/4HANA Cloud Public Edition**. As a result, the system creates a project in SAP S/4HANA Cloud Public Edition based on a project template.
6. After creating the project, you can see the project details in the **Project Data** section. The project ID is displayed as a link. To go to the project overview, select the project ID.

## Summary

You've now provisioned your **Poetry Slam Manager** solution to the SAP BTP subaccount of your customer and configured the SAP BTP subaccount of your customer. You can now continue with [extending the **Poetry Slam Manager** with a customer-specific extension](../ex2/README.md).
