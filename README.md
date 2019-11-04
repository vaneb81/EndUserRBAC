# END USER RBAC

__Ability to enforce Role-Based-Access-Control (RBAC) to lock down access to specific record fields depending on the end user’s identity.__

__SA Maintainer__: [Vanessa Peralta](mailto:vanessa.peralta@mongodb.com) <br/>
__Time to setup__: xx mins <br/>
__Time to execute__: xx mins <br/>


## Description

This proof shows how MongoDB's Role Based Access Control (RBAC) capabilities can be extended through Stitch Backend as a Service and leveraged to govern and manage the access to a MongoDB database, its collections and specific data fields, based on Collection’s Advanced Rules through the Stitch UI for end users. A MongoDB Rule grants privileges to perform specific actions on collection’s specific fields.

In this proof we will deploy an Atlas cluster which automatically enforces Authentication and then we will define Authorization Roles and Rules for two specific users. For an example HTML app for supplies database use case where one user belongs to Marketing and can read specific fields from sales collection, another user belongs to Sales and can read all data from the same collection, and one Default role who can read limited fields from the collection.

## Setup
__1. Configure Laptop__
* [Download](https://www.mongodb.com/download-center/compass) and install Compass on your laptop

__2. Configure Atlas Environment__
* Log-on to your [Atlas account](http://cloud.mongodb.com) (using the MongoDB SA preallocated Atlas credits system) and navigate to your SA project
* In the project's Security tab, choose to add a new user, e.g. __main_user__, and for __User Privileges__ specify __Read and write to any database__ (make a note of the password you specify)
* In the Security tab, add a new __IP Whitelist__ for your laptop's current IP address
* Create an __M10__ based 3 node replica-set in a single cloud provider region of your choice with default settings
* In the Atlas console, for the database cluster you deployed, click the __Connect button__, select __Connect with MongoDB Compass__, copy the __Connection String Only__ to be used later
* On your provided cluster, select “ellipsis” button --> “Load Sample Dataset” option and Load Sample Dataset
* Open Compass and select YES to use clipboard string. Use __main_user__ and password previously created to __Connect__
* Identify samples_supplies database and “sales” collection, navigate to documents to see document’s fields

__3. Configure Atlas Stitch application__
* In Atlas console navigate on the left menu to Services-->__Stitch to “Create New Application”__
* Set Application Name to __“EndUserRBAC”__
* __Link to your M10__ cluster previously created
* Leave other settings as __default__ (Stitch Service Name: mongodb-atlas; Select a Deployment Model: Global; Select a Primary Region: Virginia (us-east-1)
* __“Create”__ Stitch Application
* In Stitch interface, at “Turn on Authentication” section, __turn on “Anonymous Authentication Enabled”__
* In the same Stitch Interface, at “Initialize a MongoDB Collection” section, __Add Collection__. “Database Name” : __sample_supplies__, “Collection Name” : __sales__
* In the same Stitch Interface, at “Execute a Test Request” take note __APP ID__ to be used later
* At the top of the page, identify the blue notification, click on “REVIEW & DEPLOY CHANGES”, confirm and click “DEPLOY” in the next window
