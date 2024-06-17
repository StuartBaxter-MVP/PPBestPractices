# What is Dataverse?

Dataverse lets you securely store and manage data that's used by business applications. Data within Dataverse is stored within a set of tables. A table is a set of rows (formerly referred to as records) and columns (formerly referred to as fields/attributes). Each column in the table is designed to store a certain type of data, for example, name, age, salary, and so on. Dataverse includes a base set of standard tables that cover typical scenarios, but you can also create custom tables specific to your organization and populate them with data by using Power Query. App makers can then use Power Apps to build rich applications that use this data.

# Database Design

When designing your database, careful consideration should be given to the tables used, and the relationships between them. You should focus on minimising the number of instances of the same data being held.

For example: Orders could hold the billing address for the customer but if a customer made 100 orders, there would be 100 instances of the same information being stored – this could be simplified by holding the address on the customer record and creating a 1:N relationship between customer and order.

Dataverse is the preferred data source for all Power Platform apps – therefore the advice below applies to Dataverse but could also be applied to most other data sources.

### 1 to Many Relationships

One to many relationships are useful where there is a parent record and multiple children.

For example: one sports team has many players, or one account might have many contacts.

Effective use of 1:N relationships will remove the need for having duplicate fields and allows easy identification of which “customer” a record relates to.

### Many to Many Relationships

One-to-many (1:N) table relationships establish a hierarchy between rows. With Many-to-many (N:N) relationships there is no explicit hierarchy. There are no lookup columns or behaviours to configure. Rows created using Many-to-many relationships can be considered peers and the relationship is reciprocal.

One example of a many-to-many relationship is defined between two standard tables included with the Dynamics 365 Sales app. The opportunity table has an N:N relationship with the competitor table. This allows for multiple competitors to be added to the opportunity and multiple opportunities associated with the same competitor.

### Manual Intersect Relationships

N:N relationships cannot have additional properties, in instances where this might be needed, an intersect entity can be created.

For example, a user might be associated with a skillset, with a level of “associate”, “senior” or “principal” – the intersect table would contain a look up to the user, and the skill, and a choice field for level.

### Activity Tables

In Dataverse, activities are tasks that you or your teams perform when they contact customers. For example, sending letters or making telephone calls. You can create activities for yourselves, can assign them to someone else, or can share them with other users or teams.

An activity is any action which can be entered on a calendar and has time dimensions (start time, stop time, due date, and duration) that help determine when the action occurred or is to occur. Activities have some basic properties that help determine what action the activity represents, for example, subject and description. An activity state can be opened, cancelled, or completed.

The completed status of an activity will have several sub status values associated with it to clarify the way that the activity was completed.

Activities involve one or more participants, called activity parties in Dataverse. For a meeting activity, the participants are those contacts or users attending the meeting. For a telephone call or fax activity, the parties are the caller and the person who is called.



# Application Lifecycle Management
Application Lifecycle Management (ALM) encompasses the governance, development, and maintenance of applications. It encompasses various disciplines such as requirements management, software architecture, development, testing, maintenance, change management, continuous integration, project management, deployment, and release management. ALM tools streamline communication and collaboration between software development teams and related departments, automating software development and delivery processes. ALM integrates these disciplines to drive efficiency through predictable and repeatable software delivery.
This document focuses on the delivery and testing aspects of ALM.

## Environments

Environments serve as spaces to store, manage, and share Power Platform projects, separating apps based on roles, security requirements, or target audiences.
Solutions are containers within an environment, akin to "folders" for projects, grouping together apps and flows with a common purpose. Solutions can be moved between environments through export and import, forming the foundational principle of our ALM strategy. This enables development in one environment and usage in another, allowing for building and testing without impacting users or data, crucial for systems where downtime or data loss is more than an inconvenience.

## Publishers

Solution publishers allow you to specify who created the solution. You should always use a unique publisher, for example:

- Display Name: Stuart Baxter
- Name: StuartBaxter
- Description: Microsoft MVP - Business Appplications
- Prefix: sbax
- Choice Value Prefix: 16886

## Solutions

- **Unmanaged solutions** are developed. Unmanaged solutions are used in development environments while you make changes to your application. Unmanaged solutions can be exported either as unmanaged or managed. Exported unmanaged versions of your solutions should be checked into your source control system. Unmanaged solutions should be considered your source for Microsoft Power Platform assets. When an unmanaged solution is deleted, only the solution container of any customizations included in it is deleted. All the unmanaged customizations remain in effect and belong to the default solution.

- **Managed solutions** are deployed. Managed solutions are deployed to any environment that isn't a development environment for that solution. This includes test, UAT, SIT, and production environments. Managed solutions can be serviced independently from other managed solutions in an environment. As an ALM best practice, managed solutions should be generated by exporting an unmanaged solution as managed and considered a build artifact. Additionally:

   - You can't edit components directly within a managed solution. To edit managed components, first add them to an unmanaged solution.
   - When you do this, you create a dependency between your unmanaged customizations and the managed solution. When a dependency exists, the managed solution can't be uninstalled until you remove the dependency.
   - Some managed components can’t be edited. To verify whether a component can be edited, view the Managed properties.
   - You can't export a managed solution.
   - When a managed solution is deleted (uninstalled), all the customizations and extensions included with it are removed.

When developing solutions, it is best practice to select your preferred solution to ensure that all componets created as part of the build are included. further information about preferred solutions can be found at [Set the preferred solution - Power Apps | Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/preferred-solution)


### Connection References
A connection is a wrapper around an API that enables communication between underlying services and Microsoft Power Automate and Microsoft Power Apps. It allows users to connect accounts and utilize pre-built actions and triggers to build apps and workflows.

A connection reference, a solution component, contains information about a connector. Both canvas apps and operations within Power Automate flows bind to a connection reference. Importing a connection reference into a target environment requires no further configuration. To change a specific connection associated with a canvas app or flow, the connection reference component within the solution is edited.
Leveraging connection references in our solutions ensures that when updates are released to managed solutions, all apps and flows remain connected to their appropriate solutions.

### Environment Variables

EVs are a key component of healthy ALM and serve to allow different values to be held in different environments. An example is a SharePoint data source, in development it could point to one site, with a different site in test and production.

Environment Variables should always be created inside a solution – and contain a descriptive display name, as well as a more detailed description.

If you have several variables that are related, you could use a JSON EV to hold multiple properties.

For example:

{

"property1": "lorem ipsum",

"property2": "dolor sit amet",

"property3": "consectetur adipiscing elit",

"property4": "sed do eiusmod"

}

Further info on environment variables can be found at [Use environment variables in Power Platform solutions - Power Apps | Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/EnvironmentVariables)

## Dataverse v SharePoint

SharePoint is a popular option for data storage for Power Apps, due to not needing a premium licence, but should be carefully considered. This article details key considerations: [8 top reasons to use Dataverse or SharePoint lists in the Power Platform (sharepains.com)](https://sharepains.com/2021/04/15/dataverse-or-sharepoint-lists/#Data_volume)

It is worth noting that SharePoint can be customised to suit many business needs, but it is important to weigh up the development effort involved, as this can often outweigh the cost of buying premium licencing.


# Security Best Practices

## Environment Security Groups

Every environment, without exception, should be assigned a security group – failure to assign a security group will mean that the entire tenant can access the environment.

## Security Roles
When creating security roles ensure the naem of the security role is indicative of the tables/solution the security role is being used in alongside the level of access being provided for example "Expenses App - Admin" 

## App Security Groups

Apps should always be shared with security groups as opposed to individuals. The approach is different for canvas and model driven apps.

### Canvas Apps

1. Create a security Group in Entra ID and add the appropriate members
2. Open make.powerapps.com
3. Select your app and click share
4. Share the app with the security group created in step 1

### Model Driven Apps

1. Create a security group in Entra ID and add the appropriate members
2. Create a team in admin.powerplatform.microsoft.com of type “Entra ID security Group”, associated with the group created in step 1
3. Create a security role (no permissions needed) named after the app (e.g. “Expenses App Access”)
4. Assign the security role created in step 3 with the team created in step 2
5. Share the app with the security role created in step 3

## Assigning Security Roles to Teams

The preferred approach is to create Entra ID linked teams in Dataverse, and assign all security roles to teams, then members to those teams.

This means that if someone moves position within the customers business, they can be removed from security group X, and added to security group Y with minimal effort – and in turn inherit the correct permissions from the team.

## Nesting security groups

The preferred approach is to nest security role groups inside app groups, and in turn nest app groups into environment security groups. The result will be that users who are a member of a security role group, will gain permissions to access the app, and the environment, after being added to a single security group.
