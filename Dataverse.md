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


