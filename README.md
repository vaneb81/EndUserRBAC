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

__4. Configure Atlas Stitch Users__
* In Stitch console navigate on the left menu to Control-->__Users__
* Navigate to the top tab menu of “Providers”, identify the Provider “Email/Password” and click on right “Edit” button
* Turn on Provider
* Under “User Confirmation Method” select “Automatically confirm users”
* Under “Email Confirmation URL” fill with test string [http://test](http://test)
* Under “Password Reset URL” fill with test string [http://test](http://test)
* “Save” your changes at the bottom right button
* At the top of the page, identify the blue notification, click on “REVIEW & DEPLOY CHANGES”, confirm and click “DEPLOY” in the next window
* Return to top tab menu “Users” and Add a MARKETING user:
  * Email address: [marketing@yahoo.com](marketing@yahoo.com)
  * Password: password123
  * Confirm Password: password123
* Add SALES user:
  * Email address: [sales@yahoo.com](sales@yahoo.com)
  * Password: password123
  * Confirm Password: password123
* At the top of the page, identify the blue notification, click on “REVIEW & DEPLOY CHANGES”, confirm and click “DEPLOY” in the next window

__5. Configure Atlas Stitch Rules__
* In Stitch console navigate on the left menu to MongoDB Cluster-->__Rules__
* The page displays the Initialized MongoDB Collection “sample_supplies.sales”
* Add 4 fields (review field names that __match exactly__ as they are loaded in sales collection, you can review those in compass which was previously connected):
    * saleDate
    * storeLocation
    * couponUsed
    * purchaseMethod
* Edit “owner” role clicking pencil icon
* Rename role as __“_Marketing_”__
* In “Apply When” section you need to specify that this role will apply when authenticated user belongs to Marketing, place in the box the following code:
    ```bash
    {
    "%%user.data.email": "marketing@yahoo.com"
    }
    ```
* Leave Document-Level Permissions unchecked for Insert and Delete Documents
* Click “Done Editing”
* In the fields listed, uncheck “Write” boxes and select “Read” only for: couponUsed, purchaseMethod and storeLocation. __“saleDate” is the only one unchecked, this role will not see this field.__
* Click “Save” in the top right button
* Create a New Role called __“_Sales_”__
* Insert in “Apply When” the following code:
    ```bash
    {
    "%%user.data.email": "sales@yahoo.com"
    }
    ```
* Leave Document-Level Permissions unchecked for Insert and Delete Documents
* Click “Done Editing”
* In the fields listed, select “Read” for all the fields: couponUsed, purchaseMethod, saleDate and storeLocation. __This role will see All Data.__
* Click “Save” in the top right button
* At the top of the page, identify the blue notification, click on “REVIEW & DEPLOY CHANGES”, confirm and click “DEPLOY” in the next window
* Create a New Role called __“_default_”__
* Leave all settings empty
* Click “Done Editing”
* In the fields listed, select “Read” only for: purchaseMethod and storeLocation. __“saleDate” and “couponUsed” are unchecked, this role will not see those fields.__

__6. Configure Stitch Web Application__
* Download on your machine the file __“_EndUserRBAC_source.html_”__ at any path and edit it
* At the top you will identity JavaScript import to use stitch
    ```bash
    <script src="https://s3.amazonaws.com/stitch-sdks/js/bundles/4/stitch.js"></script>
    ```
* Replace __<APP_ID>__ in line 6 with the previously recorded APP ID from you Stitch Application [Refer to step 3].
* Replace __<DATABASE_NAME>__ in line 13 with the name of your configured Database in your Stitch Application [Refer to step 3].
* Replace __<COLLECTION_ID>__ in line 16 with the name of your configured Collection in your Stitch Application [Refer to step 3].
* Identify the “find” query that HTML is executing at “displayComments()” function
* Identify the 2 login functions that specify Marketing and Sales roles (“logInMarketing” & “logInSales”)
* Identify “logout” function that logs Out current connected user and logs with an Anonymous user, this function uses the “Default” role to see all the information.
* You are free to play around with HTML code!
