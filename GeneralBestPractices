# Power Platform Development Basics

## Product Specific Best Practices
- [Dataverse Best Practices](Dataverse.md)
- [Model Driven Apps Best Practices](ModelDrivenApps.md)
- [Canvas Apps Best Practices](CanvasApps.md)
- [Power Automate Best Practices](PowerAutomate.md)
- [Power Pages Best Practices](PowerPages.md)
- [Power BI Best Practices](PowerBI.md)
- [Copilot Best Practices](Copilot.md)

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

# App Development

## Canvas App Development

### Control naming convention

#### Screens

Screen names should reflect the purpose of the screen, so that it’s easier to navigate through complex apps in PowerApps Studio.

What’s less obvious is that screen names are read aloud by screen readers, which are needed for users who have vision accessibility needs. Therefore, it’s imperative that you use plain language to name your screens, and that the names include spaces and no abbreviations. Also, we recommend that you end the name with the word “Screen,” so that the context is understood when the name is announced.

Here are some good examples:

- Home Screen
- Thrive Help Screen

Here are some bad examples:

- Home
- LoaderScreen
- EmpProfDetails
- Thrive Help

#### Controls

Controls should be appropriately named to describe the type and purpose of the control.

All control names on the canvas should use camel case. They should begin with a three-character type descriptor, followed by the purpose of the control. This approach helps identify the type of control and makes it easier to build formulas and search.

| **Control Type** | **Prefix** |
| --- | --- |
| Button | btn |
| Combo box | cmb |
| Date picker | dte |
| Drop down | drp |
| Html text | htm |
| Icon | ico |
| Image | img |
| label | lbl |
| Shape (rectangle, circle, etc) | shp |
| Text input | txt |
| Timer | tim |

Control names must be unique across an application. If a control is reused on multiple screens, the short

screen name should be suffixed at the end – for example, galBottomNavMenuHS, where “HS” stands for “Home Screen.” This approach makes it easier to reference the control in formulas across screens.

Here’s a good example:

- lblUserNameHS

Here is a bad example:

- User Name

#### Variables

- Be descriptive of the variable’s function. Think about what the variable is bound to and how it’s used, and then name it accordingly.
- Prefix your global and context variables differently.

Be smart! PowerApps lets context variables and global variables share the same names. This can cause confusion, because, by default, your formulas use context variables unless the disambiguation operator is used. Avoid this situation by following these conventions:

- Prefix context variables with loc.
- Prefix global variables with gbl.
- The name after the prefix should indicate the intent/purpose of the variable. Multiple words can be used and don’t have to be separated by any special characters (for example, spaces or underscores), provided that the first letter of each word is capitalized.
- Use Camel casing. Begin your variable names with a prefix in lowercase letters, and then capitalize the first letter of each word in the name (that is, lowerUppperUpper).

Here are some good examples:

- Global variable: gblFocusedBorderColor
- Context variable: locSuccessMessage

Here are some bad examples:

- dSub
- rstFlds
- hideNxtBtn
- ttlOppCt
- cFV
- cQId

Avoid short and cryptic variable names such as EID. Use EmployeeId instead.

When there are many variables in an app, you can just type the prefix in the formula bar to see a list of the available variables. If you follow these guidelines to name your variables, you’ll be able to find them very easily in the formula bar as you develop your app.

Ultimately, this approach leads to quicker app development.

### App.Formulas v App.OnStart

Code written in the app on start will take time to run, and ultimately slow down the loading of your app.

Where possible use App formulas instead.

- The formula’s value is always available. There is no timing dependency, no App.OnStart that must run first before the value is set, no time in which the formula’s value is incorrect. Named formulas can refer to each other in any order, so long as they don’t create a circular reference. They can be calculated in parallel.
- The formula’s value is always up to date. The formula can perform a calculation that is dependent on control properties or database records, and as they change, the formula’s value automatically updates. You don’t need to manually update the value as you do with a variable.
- The formula’s definition is immutable. The definition in App.Formulas is the single source of truth, and the value can’t be changed somewhere else in the app. With variables, it is possible that some code unexpectedly changes a value, but this is not possible with named formulas. That doesn’t mean a formula’s value needs to be static – it can change – but only if dependencies change.
- The formula’s calculation can be deferred. Because its value is immutable, it can always be calculated when needed, which means it need not actually be calculated until it is needed. If the value is never used, the formula need never be calculated.

Formula values that aren’t used until screen2 of an app is displayed need not be calculated until screen screen2 is visible. This can dramatically improve app load time.

When creating a named formula, you should use a similar naming convention to variables, but prefix with nf.

### Collections and Galleries

Use appropriate naming for your collection:

- Be descriptive of the collection’s contents. Think about what the collection contains and/or how it’s used, and then name it accordingly.
- Collections should be prefixed with col. • The name after the prefix should indicate the intent or purpose of the collection. Multiple words can be used and don’t have to be separated by spaces or underscores, provided that the first letter of each word is capitalized.
- Use Camel casing. Begin your collection names with a lowercase col prefix, and then capitalize the first letter of each word in the name (that is, colUpperUpper).

Here are some good examples:

- colMenuItems
- colThriveApps

Here are some bad examples:

- orderscoll
- tempCollection
- My Collection

When there are many collections in the app, you can just type the prefix in the formula bar to see a list the available collections. As for variables, if you follow these guidelines to name your collections, you’ll be able to find them very easily in the formula bar as you develop your app.

Avoid using Filter() in a collection on non-unique values such as name. The reason is that if there is more than 1 'John Smith' in company, then the collection will not always work as expected.

By Filtering on a unique value like ID or Email, we can ensure that the Filter always works as it should.

Good example: ClearCollect(colEmployees, Filter(Employees, Email = User().Email));
Bad example: ClearCollect(colEmployees, Filter(Employees, Title = User().FullName));

### Collections - Performance Tips

When you are creating a collection which is getting data from a data source, use the ShowColumns function to make sure you are only pulling in the relevant data.

Good example: ClearCollect(colEmployees, ShowColumns(Employees, "First Name", "Last Name", "Job Title", "Manager"));
Bad example: ClearCollect(colEmployees, Employees);

### Patching v Forms

The two most common ways of updating a record are Patch() and SubmitForm().

The preferred approach is to use forms, where possible.

Key benefits of the form control:

- The form control has a property called “DisplayMode” – this can be New (create a new record), View (read only on an existing) or Edit (editing an existing record).
- it is possible to switch between display modes by using NewForm(), EditForm() or ViewForm() – allowing use of a single control for multiple purposes
- the SubmitForm() function allows you to write all details from the form to the database using a single line of code

Downsides of forms:

- can only write to one record, in one table – if you need to do more complex updates (such as updating an account and a contact in a single button click) you should use Patch()

### Accessibility

Accessibility is paramount to providing an inclusive app for end users.

#### Layout and colour

Common sense and uncomplicated design will help apps be more accessible to all users.

When doing heavy customization of apps take note of the below suggestions. Power Apps themes are designed to meet accessibility standards.

- Ensure all elements are clearly visible and text is of sufficient size. All content must be easily read and understood by the naked eye.
- Ensure input elements are labelled on the screen. AccessibleLabel property defines what the screen reader will announce.
- If customizing colours, ensure the contrast ratio of text to background is 4.5:1 or greater. Software tools that assist this process are readily available.
- Ensure layout follows a logical flow when read from top to bottom, left to right.
- Create a responsive app so that low-vision users can zoom in and use it without scrolling back and forth.

#### Keyboard

When testing your app's accessibility, ensure the app can be used by keyboard only, with or without a screen reader.

The Tab key should navigate to interactive elements in a logical order. You can create this with a logical app structure and by setting each control's TabIndex property accordingly.

- Label, Image, Icon, Shape controls: Set TabIndex to 0 if they are meant to be interactive. Otherwise, set TabIndex to -1.
- Do not set TabIndex greater than zero.
- Ensure that the Simplified tab index app setting is enabled.

#### Screen readers

The following screen readers have been verified to work with Power Apps:

- JAWS: Microsoft Edge
- Narrator: Microsoft Edge
- NVDA: Google Chrome, Firefox
- TalkBack: Google Chrome, Power Apps mobile
- VoiceOver: Power Apps mobile, Safari (macOS, iOS, iPadOS)

To ensure a satisfying experience with the screen reader it is recommended to:

- Ensure all input controls have the AccessibleLabel property set.
- For images, set AccessibleLabel to an appropriate description.
- If a picture is not used as a button or a link (i.e. icon is there just for the decoration) and should not be read by the screen reader, make sure the AccessibleLabel is empty or not set.
- If a picture or an icon is used as a button, then set TabIndex to 0 and AccessibleLabel to the link description.

#### Control type and structure

Using the right controls and grouping them will help screen reader users understand the structure of the app.

- Include at least one heading on each screen of the app. You can create headings by setting the Role property of a Label.
- Use a Button instead of a Label for interactive text.
- Group related content in Containers.

### Components and Component Libraries

Components allow you to reuse pats of your app in other screens (local components) or even other apps (component libraries).

In situations where there is a possibility of reuse, you should aways create components in a component library.

Further information on components and component libraries can be found at [Component library - Power Apps | Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/component-library)

## Model Driven App Development

### Form design basics

Forms should be kept simple, containing the minimal number of controls required to allow the user to do their job.

If you need to customise an out of the box form, the recommendation is to create a copy of the form and make your updates there.

Forms should consider “F pattern”. This means that the most important information is in the top left section of the form, and the least important is on the bottom right.

Sections and tabs should be used to enhance the user experience and careful consideration should be given to the logical groupings of controls into sections and tabs. It should be easy to find the information that you are looking for, and not require navigating backwards and forwards between lots of sections and tabs.

When designing a form, you should consider how the user will interact with it, and order according to the ser journey they are performing.

Business rules or JavaScript can be used to shoe and hide sections based on conditionality, but again, careful consideration should be given to how this will impact the user journey. It is also possible to create multiple forms and assign them to specific security roles, which is an effective way of only displaying relevant controls on a form.

If users are performing a linear journey through the form, you could employ a business process flow to allow quick progression thorough the journey.

#### Form naming convention

- Form Names should be camel case, prefixed with the name of the solution and suffixed by the type of form it is, if required enter additional information between the prefix and suffix to describe the purpose of the form (e.g. expenses_Main or expenses_AccountDetails_QuickView)
- Tab labels should be sentencing case (e.g. Summary).
- Tab names should be camelCase and suffixed with \_Tab (e.g. summary_Tab or customerDetails_Tab)
- Section names should be all caps (e.g. ACCOUNT INFORMATION)
- Section labels should be camel case and suffixed with \_Section (e.g. accounInformation_Section)
- Avoid using duplicate labels across multiple sections, as this will cause confusion for users (especially those using screen readers)
- Subgrid labels should be all caps (e.g. CONTACTS)
- Subgrid names should be camel case and suffixed by \_Grid (e.g. contacts_Grid)

### View Design basics

Views should have short, but descriptive names – for example “My Active Accounts” is clearly a view of active accounts belonging to the logged-on user.

The left most column in all views should be the primary name column for the record, this allows users to clearly tell which record they are looking at.

Views should contain the minimal number of columns required to serve a specific purpose – it is better to have more views than to have one monolithic view showing every possible column in a table.

Ensure that when setting column widths, the width is enough to accommodate the values held in the column – for example setting “created on” column to 125 pixels wide will display the full created date and time.

Recommended best practice is to apply the Power Apps Grid control – more info on this control can be found at [Power Apps grid control - Power Apps | Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/maker/model-driven-apps/the-power-apps-grid-control)

### Custom commands

Custom commands are buttons in the horizontal menu of a model driven app – OOTB examples are “New”, “Save”, etc.

Previously, this type of customisation would require use of Ribbon Workbench (an external tool packaged as part of XRM Toolbox) – this can now be achieved in make.powerapps.com

Recommended best practice is to create custom commands using the below approach:

[Customize the command bar - Power Apps | Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/maker/model-driven-apps/use-command-designer#create-or-edit-modern-commands)

There are some limitations to be aware of. Customisation (including hiding) of out of the box commands is not currently support – for these scenarios you should make use of Ribbon Workbench via XRM Toolbox to hide the command, and recreate it using make.powerapps.com

### Accessibility

Model Driven Apps are assessable by design - however the following should be considered:

- Naming of sections and controls consider that a screen reader might be used. Hiding of section labels will mean that the screen reader does not read the section label.
- Logical tabbing order should be employed, by design, the selected control will move vertically through a section when pressing the tab key – always test your app to ensure that the tab order makes sense.
- Column descriptions are a great tool to increase accessibility – these should not be left blank, with no exceptions.

# Power Automate Best Practices

## Naming Convention

### Naming flows

Flows should be prefixed by the solution name, followed by the type of trigger, followed by a summary of the actions undertaken, separated by “|” for example:

- Expenses App | Daily Recurrence | Send reminders for draft expenses

In the case of child flows, you should follow the same convention, but prefix with “child_”

- child_Expenses App | When a user does not have a manager | Send expenses to department head and notify HR that user does not have a manager

### Naming Actions

Actions should always be renamed to describe what the action is doing, to minimise confusion, remove spaces and suffix a description of the actions purpose on the OOTB name.

For example:

A list rows that finds all active accounts could be renamed as “ListRows_GetAllActiveAccounts”

## List Rows Actions

When using list rows, make every effort to minimise the number of rows returned, and the number of columns within those rows.

Select columns allows you to return only a subset of columns, make use of this feature to reduce the size of the response object.

Filter rows allows you to specify which rows should be returned using ODATA, where possible avoid queries with no filters, as this will return all rows in your table.

Row Count allows you to limit the number of rows to a maximum of a defined number (e.g. 100 will return 100 rows, if the table only contains 74, it will return all 74 rows)

## Using First() Expression to avoid unnecessary Apply to each

Commonly when finding a record by anything other than the GUID, you would use list rows and enter a filter (e.g. name e.g. 'John Smith Ltd’), and a row count of 1.

You can then use a first expression to get details from the one account that is returned

e.g. first(outputs('ListRows-GetJohnSmithLtd')?\['body/value'\]) would return the entire account (all columns)

You can refine this query further to return a specific column within the account object by adding this to the end of the query.

e.g. first(outputs('ListRowsGetJohnSmithLtd')?\['body/value'\])?\['accountnumber'\] would return the account number only from the account returned by your list rows

## Select Action v Apply to each for creating arrays

The select action allows you to work with arrays and create new arrays using details from the source array.

For example, if you wanted to create an array of account number and name separated by a hyphen, you could list all accounts, returning account number and name, then use an apply to each, containing an append to array variable.

The same could be achieved in a single API call by using a select action:

From: outputs('ListRows-GetJohnSmithLtd')?\['body/value'\]

Map:

Left column: AccountDetails

Right Column: concat(item()?\['accountnumber'\],' - ',item()?\['name'\])

The returned array would look like this:

\[

&nbsp; {

&nbsp;   "AccountDetails": "JS-123456 - John Smith ltd"

&nbsp; }

\]

You can use any expression in the right column of “Map”, which allows for calculations, if statements, and any other type of logic you can imagine.

## Concurrency

### Flow run concurrency

Flow run concurrency allows you to control how many runs of a flow will trigger at the same time. For example: if you have a flow that takes over an hour, running on an hourly basis, you could set the concurrency to 1. This would mean that the run due to start at 10am would only begin after the run that started at 9am. The 10am run would show a status of “queued”.

### Apply to each concurrency

Concurrency on apply to each allows you to process multiple actions in parallel. For example: if you have a recurring flow that lists 200 rows, performs some logic then updates a different row based on the outcome, without concurrency, the steps inside the apply to each would be performed one after the other. Applying concurrency of 50 means that the first 50 are processed, then the following 50, and so on. The result is that 200 sets of actions could be performed in 4 iterations of the loop (as opposed to 200)

When setting variables in an apply to each, concurrency should be set to one, to avoid the variable being updated by several iterations at the same time, potentially causing incorrect values to be used.

## Variables v Compose v Expressions

Variables can be set and reset as many times as you want within a flow. Compose actins can store a value for the duration of a flow, based on logic chosen by the maker, but can only hold one value for the duration of the flow.

Recommended practice is to use compose actions if a value is set only once and used in multiple places (one compose action v two actions to initialise a variable, then set it later to a static value)

If the output is only used once within your flow, consider skipping use of a compose or variable entirely and simply enter the expression into the required field on your flow. Consider the following example:

Variable:

- Initialise variable: AccountNameAndNumber
- Set Variable: first(outputs('ListRows-GetJohnSmithLtd')?\['body/value'\]?\[‘accountnumber’\]
- Send an email V2 action, with subject: variables('AccountNameandNumber')

Compose Action:

Compose: first(outputs('ListRows-GetJohnSmithLtd')?\['body/value'\]?\[‘accountnumber’\]

Send an email V2 action, with subject: outputs('Compose-ConcatNameAndAccountNumber')

Expression directly in subject field on send an email V2: first(outputs('ListRows-GetJohnSmithLtd')?\['body/value'\]?\[‘accountnumber’\]

The outcome is the same in all three scenarios, but setting a variable would use 3 actions, and populating in a compose action would use two. Entering the expression directly into the field will reduce the number of actions in your flow, and in turn reduce run times.

## Conditions v Switches

A condition allows you to define one or more conditions that will return true or false. Switches allow you to check the value of a field, and take different actions based on the value (e.g. if Name is John, do X, if name if Pete, do Y, if name is Sam, do Z)

Best practice is to avoid using nested if statements as this can create a pyramid effect in your flow.

Where possible, use switch if the condition has more than one possible outcome – if you need to check multiple conditions and have more than one outcome, consider using a series of conditions, and effective use of terminate to stop the flow at the correct points – this is known as guard conditions, the concept is explained in [this article](https://jonasr.app/flow-guards/)

Using guard conditions will result in an easy to read, and easy to maintain flow.

## Scope actions

The scope action supports encapsulation of actions into logical groupings.

For example: you might have a flow with hundreds of actions, using scopes allows you to group the actions such as “Get Data”, “Process Data”, “Send chasers”.

The result is a flow that is easier to read, and using descriptive names on your scope Actions could make your flow easier to maintain.

When one action in a scope fails, the entire scope will fail, this can be useful in error handling scenarios.

## Error Handling

Code first developers often make use of the try-catch approach when dealing with error handling, this can be achieved in cloud flows using scope actions and run after (more info on run after can be found at [Configure run after option - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/error-handling/2-configure-run-after))

- “Scope One” contains the actions you are trying to do (“try”)
- “Scope two” contains your error handling and is set to run after “Scope One” fails or times out (i.e. not if it succeeds, or is skipped) you could send an email to a support team, or post a message in a teams chat or channel, the actions in the catch scope will vary from project to project, but the overall theme is the same – telling someone that a business critical flow has not successfully completed so they can take the necessary action to ensure business continuity.

## Child Flows

Child flows allow you to reuse logic in multiple other flows. They are called from one or more “parent flows” and accept inputs, returning outputs.

A common use case for child flows is sending of emails with a specific format, or performing a complex calculation that is used in several flows.

Child flows have a manual trigger (with optional inputs) and should end with a respond to app or flow action (to provide an output to the parent flow).

An example child flow might accept a date of birth as an input, calculate age of the person in years and months based on today’s date, and return this value in a defined format to a parent app or flow.

Further information about child flows can be found at [Create child Flows - Power Automate | Microsoft Learn](https://learn.microsoft.com/en-us/power-automate/create-child-flows)
