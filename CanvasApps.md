# Canvas App Best Practices

## Control naming convention

### Screens

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

### Controls

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

### Variables

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

## Collections and Galleries

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

## Patching v Forms

The two most common ways of updating a record are Patch() and SubmitForm().

The preferred approach is to use forms, where possible.

Key benefits of the form control:

- The form control has a property called “DisplayMode” – this can be New (create a new record), View (read only on an existing) or Edit (editing an existing record).
- it is possible to switch between display modes by using NewForm(), EditForm() or ViewForm() – allowing use of a single control for multiple purposes
- the SubmitForm() function allows you to write all details from the form to the database using a single line of code

Downsides of forms:

- can only write to one record, in one table – if you need to do more complex updates (such as updating an account and a contact in a single button click) you should use Patch()

## Accessibility

Accessibility is paramount to providing an inclusive app for end users.

### Layout and colour

Common sense and uncomplicated design will help apps be more accessible to all users.

When doing heavy customization of apps take note of the below suggestions. Power Apps themes are designed to meet accessibility standards.

- Ensure all elements are clearly visible and text is of sufficient size. All content must be easily read and understood by the naked eye.
- Ensure input elements are labelled on the screen. AccessibleLabel property defines what the screen reader will announce.
- If customizing colours, ensure the contrast ratio of text to background is 4.5:1 or greater. Software tools that assist this process are readily available.
- Ensure layout follows a logical flow when read from top to bottom, left to right.
- Create a responsive app so that low-vision users can zoom in and use it without scrolling back and forth.

### Keyboard

When testing your app's accessibility, ensure the app can be used by keyboard only, with or without a screen reader.

The Tab key should navigate to interactive elements in a logical order. You can create this with a logical app structure and by setting each control's TabIndex property accordingly.

- Label, Image, Icon, Shape controls: Set TabIndex to 0 if they are meant to be interactive. Otherwise, set TabIndex to -1.
- Do not set TabIndex greater than zero.
- Ensure that the Simplified tab index app setting is enabled.

### Screen readers

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

### Control type and structure

Using the right controls and grouping them will help screen reader users understand the structure of the app.

- Include at least one heading on each screen of the app. You can create headings by setting the Role property of a Label.
- Use a Button instead of a Label for interactive text.
- Group related content in Containers.

## Components and Component Libraries

Components allow you to reuse pats of your app in other screens (local components) or even other apps (component libraries).

In situations where there is a possibility of reuse, you should aways create components in a component library.

Further information on components and component libraries can be found at [Component library - Power Apps | Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/component-library)

