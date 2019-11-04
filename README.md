# EndUserRBAC
<b>Required Capability: Ability to enforce Role-Based-Access-Control (RBAC) to lock down access to specific record fields depending on the end user’s identity.</b>
<br><br>
<b>Description</b>
<br>
This proof shows how MongoDB's Role Based Access Control (RBAC) capabilities can be extended through Stitch Backend as a Service and leveraged to govern and manage the access to a MongoDB database, its collections and specific data fields, based on Collection’s Advanced Rules through the Stitch UI for end users. A MongoDB Rule grants privileges to perform specific actions on collection’s specific fields.
<br><br>
In this proof we will deploy an Atlas cluster which automatically enforces Authentication and then we will define Authorization Roles and Rules for two specific users. For an example HTML app for supplies database use case where one user belongs to Marketing and can read specific fields from sales collection, another user belongs to Sales and can read all data from the same collection, and one Default role who can read limited fields from the collection.
<br><br>
<b>Setup</b>
<br>
