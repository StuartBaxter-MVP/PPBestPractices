# Model Driven Apps Best Practices

## Model Driven App Development

### Form design basics

Forms should be kept simple, containing the minimal number of controls required to allow the user to do their job.

If you need to customise an out of the box form, the recommendation is to create a copy of the form and make your updates there.

Forms should consider “F pattern”. This means that the most important information is in the top left section of the form, and the least important is on the bottom right.

Sections and tabs should be used to enhance the user experience and careful consideration should be given to the logical groupings of controls into sections and tabs. It should be easy to find the information that you are looking for, and not require navigating backwards and forwards between lots of sections and tabs.

When designing a form, you should consider how the user will interact with it, and order according to the ser journey they are performing.

Business rules or JavaScript can be used to show and hide sections based on conditionality, but again, careful consideration should be given to how this will impact the user journey. It is also possible to create multiple forms and assign them to specific security roles, which is an effective way of only displaying relevant controls on a form.

If users are performing a linear journey through the form, you could employ a business process flow to allow quick progression thorough the journey.

#### Form naming conventions

- Form Names should be camel case, prefixed with the name of the solution and suffixed by the type of form it is, if required enter additional information between the prefix and suffix to describe the purpose of the form (e.g. expenses_Main or expenses_AccountDetails_QuickView)
- Tab labels should be sentencing case (e.g. Summary).
- Tab names should be camelCase and suffixed with \_Tab (e.g. summary_Tab or customerDetails_Tab)
- Section labels should be all caps (e.g. ACCOUNT INFORMATION)
- Section names should be camel case and suffixed with \_Section (e.g. accountInformation_Section)
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

There are some limitations to be aware of. Customisation (including hiding) of out of the box commands is not currently supported – for these scenarios you should make use of Ribbon Workbench via XRM Toolbox to hide the command, and recreate it using make.powerapps.com

### Accessibility

Model Driven Apps are assessable by design - however the following should be considered:

- Naming of sections and controls consider that a screen reader might be used. Hiding of section labels will mean that the screen reader does not read the section label.
- Logical tabbing order should be employed, by design, the selected control will move vertically through a section when pressing the tab key – always test your app to ensure that the tab order makes sense.
- Column descriptions are a great tool to increase accessibility – these should not be left blank, with no exceptions.

