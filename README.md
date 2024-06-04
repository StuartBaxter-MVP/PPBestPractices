# Welcome to Power Platform Best Practices!

![Power Platform Logo](https://decisioninc.com/wp-content/uploads/2022/10/microsoft_power_platform_icon_2022.png)

This is a community lead repo, and anyone is free to submit a pull request.

Feel free to refer to articles and blogs by other community members, but please do give them credit for thier work using a link to the article/blog post

# Power Platform Development Basics

## Product Specific Best Practices
- [Dataverse Best Practices](Dataverse.md)
- [Model Driven Apps Best Practices](ModelDrivenApps.md)
- [Canvas Apps Best Practices](CanvasApps.md)
- [Power Automate Best Practices](PowerAutomate.md)
- [Power Pages Best Practices](PowerPages.md)
- [Power BI Best Practices](PowerBI.md)
- [Copilot Best Practices](Copilot.md)

# General Advice

## Publishers

Solution publishers allow you to specify who created the solution. You should always use a unique publisher, for example:

- Display Name: Stuart Baxter
- Name: StuartBaxter
- Description: Microsoft MVP - Business Appplications
- Prefix: sbax
- Choice Value Prefix: 16886 

## Solutions

Solutions are containers for your customisations, everything we build should be contained with a solution, this will facilitate smooth deployment to other environments as part of the standard ALM process.

Components should be contained a single solution where possible, where multiple solutions are required, components should only be present in one solution, to reduce the risk of solution layering causing unexpected issues.

## Environment Strategy / ALM

As a minimum we should have at least 3 environments for every project:

Development – used to create solutions.

Test – used to test customisations.

Production – Live system

Solutions deployed to Test or Production should be managed solutions.

Use of environment variables will mean there is no need to create unmanaged layers in Production.

## Environment Variables

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

## Licencing

It is important to consider licencing impact of the solutions we build.

Power Apps Licencing comes in 3 models:

- Premium: Allows access to as many apps as you can build
- Per App: Allows access to a single app
- Per App Pay as You Go: Allows access to a single app, on a pay as you go basis, billed monthly based on unique active users. In short if a user logs on in a given month, they are billed for the month, if they don’t, there is no charge for that month

Details on request limits can be found at <https://learn.microsoft.com/en-us/power-platform/admin/api-request-limits-allocations#licensed-user-request-limits>

Power Automate Licencing also comes in 3 models:

- Premium – Access to as many flows as you can build
- Per Process – a single process
- Per process Hosted – a single process, with hosted machines

Details on request limits can be found at <https://learn.microsoft.com/en-us/power-platform/admin/api-request-limits-allocations#licensed-user-request-limits>

Running flows in the context of an app does not require additional Power Automate Licencing. Find out more about how to associate flows with an app at [Associate flows with apps - Power Automate | Microsoft Learn](https://learn.microsoft.com/en-us/power-automate/associate-flow-to-app)

## Dataverse v SharePoint

SharePoint is a popular option for data storage for Power Apps, due to not needing a premium licence, but should be carefully considered. This article details key considerations: [8 top reasons to use Dataverse or SharePoint lists in the Power Platform (sharepains.com)](https://sharepains.com/2021/04/15/dataverse-or-sharepoint-lists/#Data_volume)

It is worth noting that SharePoint can be customised to suit many business needs, but it is important to weigh up the development effort involved, as this can often outweigh the cost of buying premium licencing.

# Security Best Practices

## Environment Security Groups

Every environment, without exception, should be assigned a security group – failure to assign a security group will mean that the entire tenant can access the environment.

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



