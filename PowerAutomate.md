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

