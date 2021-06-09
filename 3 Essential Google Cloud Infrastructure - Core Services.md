# 3 Essential Google Cloud Infrastructure - Core Services

## Table of contents

1. Introduction
2. Cloud IAM
3. Storage and Database Services
4. Resource Management
5. Resource Monitoring

## 2. Cloud IAM

### 2.01. Overview

Overview
Cloud Identity and Access Management (IAM). A sophisticated system, built on top of e-mail like address names, job type roles, and granular permissions. 

### 2.02. Cloud IAM

Cloud IAM
Who - what - where
* Who
* Can do what
* On which resource


Who - person, group, organization
Can do that - specific privileges or actions
On which resource - Any GCP resource/service
For example, I have the privilege of Compute viewer - read-only access to get and list Compute resources w/o being able to read the data stored on them.
Cloud IAM objects
  



* Organization - root node
   * Folders
      * Projects
         * Resources - each resource has exactly one parent


Policies (Roles, Members) -can be set at any level. 


Organization resource - represents your company. Roles at this level are inherited by all resources under the org
Folder resource - could represent your department
Projects - represent a trust boundary within your company, services within the same project have a default level of trust.


If you change the resource hierarchy, the policy hierarchy also changes. For example, moving a project to a different org will update projects IAM policy to inherit from the new organization’s Cloud IAM policy.


Child policies cannot restrict access granted at the parent level. Therefore it is a best practice to follow the principle of least privilege. Always select the smallest scope that is necessary for the task to reduce your exposure to risk. 

### 2.03. Organization

Organization
* Organization node is a root node for GCP resource
* Org. roles:
   * Organization Admin: control over all cloud resources, useful for admin
   * Project Creator: controls project creation, control over who can create projects
Creating and managing organizations
* Created when a G Suite or Cloud Identity account creates a GCP project


G Suite or Cloud Identity super administrator:
* Assign organization admin role to some users
* Be a point of contact in case of recovery issues
* Control the lifecycle of G Suite or Cloud Identity account and Organization resource


Organization admin:
* Define IAM policies
* Determine the structure the resource hierarchy
* Delegate responsibility for components such as networking, billing and REsource Hierarchy through IAM roles
* Does not include roles for other actions, such as creating folders, org. Admin must assign additional roles to their account
Folders
  



Additional grouping mechanism and isolation boundaries b/w projects:
* Diff. legal entities
* Departments
* Teams
* Products
* etc.
Folders allow delegation of administration rights
Resource manager roles:
Policies are inherited top to bottom


* Organization:
   * Admin - full control over all resources
   * Viewer - view access over all resources
* Folder:
   * Admin - full control over folders
   * Creator - Browse hierarchy and create folders
   * Viewer - View folders/resources below a resource
* Project:
   * Creator - create new projects and migrate new projects into an organization 
   * Deleter - deletes projects

### 2.04. Roles




Roles
Define “Can do what” and “on which resources” part of Cloud IAM.


Three types of rules:
* Primitive
* Predefined
* Custom
Primitive role
 are original roles available in the GCP console. Broad roles, affect all resources in that project. Offer fixed, coarse-grained levels of access:
* Owner
   * Add/remove members
   * Delete projects
   * And ...
* Editor
   * Deploy apps
   * Modify code
   * Configure server
   * And ...
* Viewer
   * Read-only access


* Billing administrator
   * Manage billing
   * add/remove administrators
Predefined roles
Apply to a particular GCP service in a project. Offer more fine-grained permissions on particular services


InstanceAdmin Role:
* Compute.instances.delete
* Compute.instances.get
* Compute.instances.list
* Compute.instances.setMachineType
* Compute.instances.start
* Compute.instances.stop


The permissions themselves are classes and methods in APIs.


Compute Engine IAM roles:
* Compute Admin - Full control od all Compute Engine resources (compute.* )
* Network Admin - Permissions to create, modify, and delete networking resources, except for firewall rules and SSL certificates
* Storage Admin - permissions to create, modify and delete disks, images, and snapshots


Custom roles
let you define a precise set of permissions


Instance Operator Role:
* Compute.instances.get
* Compute.instances.list
* Compute.instances.start
* Compute.instances.stop



### 2.05. Demo: Custom roles

Demo: Custom Roles
Specify instance Operator Role
Navigation menu -> Products -> IAM & admin -> Roles -> Create Role:
* Title: Instance Operator
* Add permissions -> Filter permissions by role: compute.instances.*
   * Compute.instances.get
   * Compute.instances.list
   * Compute.instance.reset
   * Compute.instance.resume
   * Compute.instances.start
   * Compute.instances.stop
   * Compute.instances.suspend
* Create

### 2.06. Members


Members
Define “who” part of Cloud IAM.


  



Five different types of members:
* Google Account 
* Service Account
* Google Group
* G Suite Domain
* Cloud Identity Domain
Google Account
Developer, administrator, or any other person that interacts with GCP. Any email address that is associated with google account ban be an identity.

### 2.07. Service Accounts



Service Account
An account that belongs to your application, instead of the individual end-user. You can create as many service accounts as needed.
Google Group
Named collection of Google accounts and/or Service accounts. Every group has a unique email address, associated with the group. Google Groups are a convenient way to apply an access policy to a collection of users. You can add/change access controls for a whole group at once, instead of changing access control one at the time for individual users or service accounts.
G Suite Domain
Represent your organization’s internet domain name, for example, example.com. 
When you add a user to your G Suite Domain, a new account is created for the user, for example, user@example.com.
Cloud Identity Domain
GCP customers that are not G Suite customers, can get the same capabilities through Cloud Identity. Cloud Identity lets you manage users and groups, using the Google Admin console, without benefits like Gmail, docs, calendar, etc. Cloud Identity is available in free and premium editions. Premium adds features for mobile device management. 


You can not use Cloud IAM to manage users or groups, instead you can use Cloud Identity for that.


If you have an existing Active Directory or LDAP, you can use Google Cloud Directory Sync to get users and groups in GCP.
  

Service Accounts
Service Accounts provide an identity for carrying out server-to-server interactions.
* Programs running with Compute Engine instances can automatically acquire tokens with credentials
* Tokens are used to access and service API in your project and any other services that granted access to that service account
* Service accounts are convenient when you are not accessing user data


Service accounts are identified by an email address
123845678986-compute@project.gserviceaccount.com


Three types of accounts:
* User-created (custom)
* Built-in (Compute Engine and Application Engine default service accounts)
* Google APIs service account (Runs internal Google processes on your behalf)
Default Compute Engine service account
* Automatically created per project with automatically generated name and email address
* The name has -compute suffix: 39xxx0964-compute@developer.gserviceaccount.com
* Automatically added as project Editor
* By default, enabled on all instances created using gcloud or GCP console


  

Customizing scoped for a VM
* Scopes can be changed after an instance is created
* For user-created service accounts, use Cloud IAM roles instead
Service Account permissions
* Default service accounts: primitive and predefined
* User-created service accounts: predefined roles
* Roles for service account can be assigned to groups or users


Service accounts authenticate with keys
* GCP-managed: cannot be downloaded and are automatically rotated
* User-managed: create, manage and rotate yourself

### 2.08. Cloud IAM best practices

Cloud IAM best practices
Leverage and understand the resource hierarchy
* Use projects to group resources that share the same trust boundary
* Check the policy granted on each resource & make sure that you understand the inheritance
* User principle of least privilege when granting roles
* Audit policies in Cloud audit logs: ‘setiampolicy’
* Audit membership of groups used in policies
Granting roles to Google groups instead of individuals
* Update group membership instead of changing Group IAM policy
* Audit membership of groups used in policies
* Control the ownership of the Google Group used in IAM policies
Service Accounts
* Be very careful granting serviceAccountUser role, as it provides access to all the resources that the service account has access to.
* When you create a service account, give it a display name that identifies its purpose
* Establish a naming convention for service accounts
* Establish key rotation policies and methods
* Audit with service.account.keys.list() method
Cloud Identity-Aware Proxy (Cloud IAP)
Enforces access control policies for applications and resources:
* Identity-based access control
* Central authorization layers for applications access by HTTPs
Cloud IAM is applied after authentication
So you can use application-level access control, instead of relying on network-level firewalls.

### 2.09. Lab Intro: Cloud IAM

### 2.10. Getting Started With GCP And Qwiklabs

### 2.11. Lab: Cloud IAM


Lab: Cloud IAM
Grant and revoke roles to change access, using Cloud IAM
Username 1: create a bucket and upload a file
Navigation Menu -> Storage -> Cloud Storage -> Create bucket
Upload file in the bucket
Username 2: Check Cloud Storage bucket via CGP Console
Success
Remove project viewer role from username2
Navigation Menu -> IAM & Admin -> IAM ->  find username2 -> edit -> remove access -> save
Username 2: Check Cloud Storage bucket via CGP Console
List of buckets cannot be loaded
Add storage access
Navigation Menu -> IAM & Admin -> IAM -> add:
* New members: username 2
* Select a role: Storage Object Viewer
* Save
Username 2: check Cloud Storage via Cloud Shell
$ gsutil ls gs://bucket_name
Success
Setup service account user
Navigation Menu -> IAM & Admin -> Service accounts -> Create a service account:
* Service account name: read-bucket-objects
* Service account permissions: Storage Object Viewer
* Create


Navigation Menu -> IAM & Admin -> IAM-> Select read-bucket-objects@...iam.gserviceaccount.com:
* New members: altostrat.com
* Role: service.account.user
* Save


Navigation Menu -> IAM & Admin -> IAM-> Add:
* New members: altostrat.com
* Role: compute.instance.admin(v1)
* Save
Create new VM using the service account:
* Service Account: read-bucket-objects
* Create
SSH into VM and check permissions
$ gloud compute instances list
Error as compute.intance.admin(v1) does not allow to list instances


$gsutil cp gs://bucket_name/sample.txt .
Success, as Storage Object Viewer allows to view and copy items


$gsutil cp sample.txt gs://bucket_name/sample2.txt
Error, as the user does not have Storage Object Create permissions.

### 2.12. Lab Review: Cloud IAM

### 2.13. Review

## 3. Storage and Database Services

Storage and Database Services
Overview
Every application needs to store data. Whether it is business data, media to be streamed or sensor data from devices. 


From application point technology stores and retrieves the data, whether it is a database or object store is less important than whether that service supports application requirements for efficiently storing and retrieving the data given its characteristics.

Storage and database services
Type:
	Object
	Relational
	Non-Relational
	Warehouse
	Name:
	Cloud Storage
	Cloud SQL
	Cloud Spanner
	Cloud Firestore
	Cloud Bigtable
	BigQuery
	Good For:
	Binary / object data
	Web Frameworks
	RBBMS+scale, HA, HTAP
	Hierarchical, mobile, web
	Heavy read + write, events
	Enterprise data warehouse
	Such as:
	Images, media serving, backups
	CMS, eCommerce
	User metadata, Ad/Fin/Mar Tech
	User profiles, game state
	AdTech, financial, IoT
	Analytics, dashboards
	  

Storage and database decision chart
  

Scope
Infrastructure track:
* Service differentiators
* When to consider using each system
* Setup and connect to a service


Data engineering track:
* How to use a DB system
* Design, organization, structure, schema, and use for an application
* Details about how to service stores and retrieves structured data
Cloud Storage
GCPs object storage service offers worldwide storage/retrieval of any amount of data at any time.
Use cases
* Website content
* Distributing large data objects to users via direct download
* Storing data for archiving and disaster recovery


* Scalable to exabytes
* Time to the first byte in milliseconds
* Very high availability across all storage classes
* Single API across storage classes
Storage classes
* Regional
   * Data-intensive computations
   * data governance
* Multi-Regional
   * Website content
   * Interactive workloads
* Nearline
   * Backup
   * Long-tail multimedia
* Coldline
   * Archiving
   * Disaster recovery
Cloud Storage overview
Buckets:
* Naming requirements
* Cannot be nested


Objects:
* Inherit storage class of bucket, when created 
* No minimum size, unlimited storage


Access:
* Gsutil
* APIs (JSON API, XML API)
Changing storage class
* The default class is applied to new objects
* The regional bucket can never be changed to Multi-Regional
* The multi-Regional bucket can never be changed to Regional
* Objects can be moved from bucket to bucket
* Object Lifecycle Management (OLM) can manage the classes of objects
Access control
Project -> Bucket -> Object


* Cloud IAM- Cloud Identity and Access Management
* ACL - Access Control Lists (max 100 entries per bucket)
   * Scope - who has access, examples:
      * collab@gmail.com, 
      * allUsers (anyone on the internet), 
      * allAuthenticatedUsers (anyone authenticated with google account)
   * Permission - what level of access (owner, write, read)
* Signed URL - Signed and Timed Cryptographic Key
   * “Valet key” access to buckets and objects via ticket
   * The ticket is a cryptographically signed URL
   * Time-limited
   * Operations specified in the ticket: HTTP GET, PUT, DELETE (not POST)
   * Any user with URL can invoke permitted operations
   * Example using private account key and gsutil: gsutil signurl -d 10m path/to/privatekey.p12 gs://bucket/object
* Signed Policy Document - Control File Upload Policy
  

Cloud Storage Features
* Customer supplied encryption key
* Object Lifecycle Management (OLM) - specify actions to be performed on objects that meet certain rules:
   * Object inspection occurs in async batches
   * Changes can take up to 24 hrs to apply
   * Examples:
      * Downgrade storage class on objects older than 1 year
      * Delete objects created before specific data
      * Keep only 3 most recent versions 
* Object Versioning
   * Objects are immutable
   * Object versioning allows maintaining a history of modification of objects
   * Lists archived versions of an object, restore an object to an older state, or delete a version
* Directory sync.
* Object Change notification - can be used to notify an application, when an object is updated or added to a bucket, Cloud Pub/Sub Notifications for Cloud Storage
  

* Data Import
* Strong consistency


  

Lab: Cloud Storage
Preparation, create a new Cloud Storage bucket
Navigation menu -> Storage -> Cloud Storage -> Create:
Project ID = bucket ID, 
Choose a default storage class: multiregional
ACL: set object-level and bucket-level permissions
Create


$ export BUCKET_NAME_1=”Project ID”
Preparation, copy setup.html
$ curl https://hadoop.apache.org/docs/current/\
hadoop-project-dist/hadoop-commom/\
ClusterSetup.html > setup.html


$ cp setup.html setup2.html
$ cp setup.html setup3.html
Access Control List
Get default acls
$ cp setup.html gs://BUCKET_NAME_1/
$ gsutil acl get gs://BUCKET_NAME_1/setup.html > acl.txt
$ cat acl.txt


Set permissions to private
$ gsutil acl set private gs://BUCKET_NAME_1/setup.html
$ gsutil acl get gs://BUCKET_NAME_1/setup.html > acl2.txt
$ cat acl2.txt


Make file publicly readable
$ gsutil acl ch -u AllUsers:R gs://BUCKET_NAME_1/setup.html
$ gsutil acl get gs://BUCKET_NAME_1/setup.html > acl3.txt
$ cat acl3.txt


Remove setup.html from cloud shell and download it from bucket
$ rm setup.html
$ ls -l
$ gsutil cp gs://BUCKET_NAME_1/setup.html setup.html
$ ls -l
Create a customer-supplied encryption key
Create .boto file
$ gsutil config -n


Create encryption key
$ python3 -c 'import base64; import os; print(base64.encodebytes(os.urandom(32)))'


Paste encryption key in boto file
$ nano .boto
Uncomment encryption_key
Add encryption key after “=” sign


Upload setup2.html and setup3.html to bucet
$ gsutil cp setup2.html gs://BUCKET_NAME_1/
$ gsutil cp setup3.html gs://BUCKET_NAME_1/


In GCP Console You will see that files are provided with Customer-supplied key


Delete local files and download them from bucket
$ rm setup*
$ gsutil cp gs://BUCKET_NAME_1/ ./
$ cat setup2.html
Rotate encryption keys
$ nano .boto
encryption key -> decryption key
$ python3 -c 'import base64; import os; print(base64.encodebytes(os.urandom(32)))'


Output -> encryption key


$ gsutil rewrite -k gs://$BUCKET_NAME_1/setup2.html


Comment out description_key1 in .boto file


$ gsutil cp gs://$BUCKET_NAME_1/setup2.html recover2.html


$ gsutil cp gs://$BUCKET_NAME_1/setup3.html recover3.html
Error, no decryption key matches object setup3.html
View current lifecycle policy
$ gsutil lifecycle get gs://BUCKET_NAME_1/


Define policy:
$ nano life.json


{
  “rule”:
  [
    {
    “action”: {“type”: “Delete”},
    “Condition”: {“age: 31”}
    }    
  ]
}


Set policy
$ gsutil lifecycle set life.json gs://BUCKET_NAME_1/
Enable versioning
Check versioning
$ gsutil versioning get gs://BUCKET_NAME_1/


Enable versioning
$ gsutil versioning set on gs://BUCKET_NAME_1/


Modify and save the file locally 
Copy updated file to bucket
$ gsutil cp -v setup.html gs://BUCKET_NAME_1


List all the saved version
$ gsutil ls -a gs://BUCKET_NAME_1/


Recover
export VERSION_NAME=<Enter Version name herr>
$ gsutil cp VERSION_NAME recovered.txt


Sync directory to a bucket
$ gsutil rsync -r ./firstlevel gs://BUCKET_NAME_1/firstlevel


View dir in bucket
$ gsutil ls -r gs://BUCKET_NAME_1/firstlevel
Cloud SQL
Should you build your DB solution or use a managed service? 
Cloud SQL is a fully managed DB service (MySQL or PostgreSQL)
* Patches and updates automatically applied
* You administer MySQL users
* Cloud SQL supports many clients:
   * gcloud sql
   * Application Engine, G Suite scripts
   * Applications and tools:
      * SQL workbench, Toad
      * External apps using standard MySQL drivers
Cloud SQL services
* Replica services
* Backups services
* import/export
* Scaling:
   * Up: machine capacity
   * Out: replicas
Connecting to a Cloud SQL instance
  

Choosing Cloud SQL
  

Lab: Cloud SQL


* Configured a Cloud SQL server,  
* learned how to connect an application to it via
   * a proxy over an external connection.
   * a Private IP link that offers performance and security benefits
 
  

You can only connect via Private IP, if:
* The application and Cloud SQL are co-located in the same region and 
* are part of the same VPC network.
 
If you can host your application in the same region and VPC connected network as your Cloud SQL, you can leverage a more secure and performant configuration using Private IP.
 
By using Private IP, you will increase performance by reducing latency and minimize the attack surface of your Cloud SQL instance because you can communicate with it exclusively over internal IPs.
 
If an application is hosted in another:
* region
* VPC
* project
use a Secure Proxy instead.
Create a Cloud SQL database
Two VM instances are created by default:
* wordpress-europe-proxy – for proxy for Cloud SQL
* wordpress-us-private-ip - for Private IP instances.


Navigation menu -> Storage -> SQL -> Create Instance -> Choose MySQL
* Instance ID: wordpress-db
* Root password: password
* Region/Zone: us-central1/Any
* Configuration Options -> Set Connectivity:
   * Private IP -> Enable Service Networking API
   * Allocate and Connect
* Create
 
Databases -> Create a database:
* Database name: wordpress
Create
Configure a proxy on a virtual machine
SSH in wordpress-europe-proxy
 
Download cloud SQL proxy and make it executable
$ wget https://gl.google.com/cloudsql/cloud_sql_proxy.linux.amd4 -O cloud_sql_proxy && chmod +x cloud_sql_proxy
 
To start the proxy, you need the connection name of Cloud SQL instance:
Navigation menu ->  Storage -> SQL -> wordpress-db -> Instance connection name -> copy
 
In SSH define SQL_CONNECTION variable:
$ export SQL_CONNECTION=”Instance connection name”
 
Activate proxy connection to SQL database and send the process to the background
$ ./cloud_sql_proxy -instance=$SQL_CONNECTION=tcp:3306 &
Ready for new connections


Connect to Cloud SQL via Proxy
  

 
Find machine's external IP address
$ curl -H "Metadata-Flavor: Google" http://169.254.169.254/computeMetadata/v1/instance/network-interfaces/0/access-configs/0/external-ip && echo
 
Run the Installation, specifying:
Username        root
Password        [ROOT_PASSWORD]
Database Host        127.0.0.1*
* you are using 127.0.0.1, localhost as the Database IP because the proxy you initiated listens on this address and redirects that traffic to your SQL server securely.
Connect to Cloud SQL via internal IP
  

 
Navigation menu -> SQL -> Click wordpress-db -> Copy the Private IP address of the Cloud SQL server


Copy the external IP address of wordpress-us-private-ip, paste it in a browser window, and press ENTER
Username             root
Password             [ROOT_PASSWORD]
Database Host  [SQL_PRIVATE_IP]
Cloud Spanner
Cloud SQL with a horizontal scale.


Cloud Spanner combines the benefits of relational DB structure with a non-relational horizontal scale
* Scale to petabytes
* Strong consistency
* High availability
* Used for financial/inventory apps
* Regional/multi-regional uptime 99,99%/99,999%
Cloud Spanner Characteristics
  

Choosing Cloud Spanner
  

Cloud Firestore
Highly scalable no-SQL document database. Cloud Firestore is next-gen Cloud Datastore.


* Simplifies storing, syncing and querying data
* Mobile, web, IoT at a global scale
* Live sync and offline support
* Security features
* ACID transactions (if any fail, whole fail)
* Multi-region replication
* Powerful query engine


Cloud Firestore is the next generation of Cloud Datastore
* Datastore mode (new server projects):
   * Compatible with Datastore applications
   * Strong consistency
   * No Entity group limit
* Native mode (new mobile/web apps):
   * Strong, consistent storage layer
   * Collection and document data model
   * Real-time updates
   * Mobile and Web client libraries
Choosing Cloud Firestore
  



Cloud Bigtable
No SQL big data database service
* Petabyte-scale
* Consistent sub-10ms latency
* Seamless scalability for throughput
* Learns and adjusts to access patterns
* Ideal for Ad Tech, Fintech, and IoT
* Storage engine for ML applications
* Easy Integration with open source big data tools


Cloud Bigtable storage model
  

Cloud Bigtable access patterns
  



Choosing Cloud Bigtable
  



Cloud Memorystore
Fully managed Redis service
* In-Memory data store service
* Focus on building great apps
* High availability, failover, patching, and monitoring
* Sub-millisecond latency
* Instances up to 300 GB
* Network throughput of 12 Gbps
* Easy Lift-and-Shift
Resource management
Overview
Resource management. As resources in GCP are billable, managing resources means controlling costs. 
Cloud Resource Management
Resource Manager lets you hierarchically manage resources.
GCP -> Organization -> Folders -> Projects -> Resources
 
Identity and Access Management (Top -> Bottom approach)
* child policies cannot restrict access granted at the parent level
 
Billing and Resource Monitoring (Bottom -> Top approach)
* A resource belongs to one and only one project (rate of use or time, nr of items or feature use)
* Project is associated with one bolling account
* The organization contains all billing accounts 
Organization node is the root node for GCP resources
 
The project accumulates the consumption of all its resources
Track resource and quota usage:
* Enable billing
* Manage permissions and credentials
* Enable services and APIs
Projects use three identifying attributes:
* Project name or
* Project Number or
* Project ID
 
Resource Hierarchy
Resources are global, regional or zonal
 
Global:
* Images
* Snapshots
* Networks
 
Regional:
* External IP address
 
Zonal:
* Instances
* Disks
Quotas
All resources are subject to project quotas or limits
 
How many resources per project:
5 VPC networks/project
 
How many resources per region:
24 CPUs region/project
 
Ho quickly you can make an API request in a project: rate limit:
5 admin actions/second
 
To increase: Quotas page in GCP Console or a support ticket
Why quotas?
* Prevent runaway consumption in case of error or malicious attack
* Prevent billing spikes/surprises
* Forces sizing consideration and periodic review
Labels and names 
Labels are a utility for organizing GCP resources
Projects and folders provide levels of segregation for resources, but what if you want more granularity? That is where labels and names come in.
 
Labels are a utility for organizing GCP resources, which are attached to resources: VM, disk, snapshot, image.
Each resource can have up to 64 labels, for example:
* Team or cost Center
* Components
* Environment or stage
* Owner or contact
* State
Labels vs. Tags
Labels are a way to organize resources across GCP
User-defined stings in key-value format
Propagated through billing
 
Tags are applied to instances only
User-defined strings
Tags are primarily used for networking (applying forewall rules) 
Billing
Consumption of all resources under a project accumulates into one billing account.
Budgets and email alerts
Setting a budget lets you track how your spending is growing towards that amount.
 
* First, you specify a budget name and select the project this budget applies to.
* Then you can set the budget at a specific amount, or match it to a previous month spent.
* Then you can set budget alerts, which sent emails to billing admins after the spend exceeds the alert amount.
* In addition to email, you can use Cloud Pub/Sub notifications to receive spend updates. You can even create a Cloud Function that listens to a specific topic to automate cost management.
 
Labels can help you optimize GCP spend
Another way to optimize your GCP spend is to use labels. For example, you could label VM instances that are spread across diff. regions. Label your resources and export billing data to BigQuery to analyze your spending.
 
SELECT
  TO_JSON_STRING(labels) as labels,
  sum(cost) as cost
FROM `project.dataset.table`
GROUP BY labels;
 
Visualize GCP spend with Data Studio
You can even visualize spend over time with Data Studio, which turns data into dashboards, which are easy to read, easy to share, and fully customizable. 
Demo: billing Administration
Navigate to Billing
Navigation Menu -> Billing -> Overview
Navigate to budgets and alerts
Navigation Menu -> Billing -> Budgets & alerts -> Create a budget
Name:
Projects
Type/Target Amount
Threashdolds 50/90/100
Connect a Cloud Pub/Sub topic
Finish
See all of the different charges
Navigation Menu -> Billing -> Transactions
Export data
Navigation Menu -> Billing -> Billing export
BigQuery -> Edit Settings -> create a BigQuery dataset -> go to BigQuery


FileExport -> Bucket Name, Report prefix -> Format (JSON, csv)
Review payment method:
Navigation Menu -> Billing -> Payment method
Review different payment account, payment profiles, payment methods, etc 
Lab: Examining Billing with BigQuery
In this lab, you learn how to perform the following tasks:
* Sign in to BigQuery from the Cloud Console
* Create a dataset
* Create a table
* Import data from a billing CSV file stored in a bucket
* Run complex queries on a larger dataset


You then accessed a shared dataset, containing more than 22K records of billing information and run several queries on that dataset.
Use BigQuery to import data
Navigation Menu -> Big DAta -> BigQuery -> Create dataset
Dataset ID: imported_billing_data
Location: US
Expire: 1 day
Create


Create Table:
Source: Google Cloud Storage
Location: gs://cloud-training/archinfra/export-billing-example.csv
Format: csv
Name: sampleinfotable
Schema: auto-detect
Header rows to skip: 1
Create Table
Examine the data
Table -> Schema to view the schema
Table -> details for nr of rows
Table -> preview to see the data
Compose a query
Query table - auto-populates with data


SELECT * FROM projectid
Where cost > 0
run
Resource Monitoring
Stackdriver Overview
Stackdriver dynamically discovers cloud resources and application services, based on deep integration with GCP and AWS. 


* Integrated monitoring, logging, diagnostics
* Manage across platforms
   * GCP and AWS
   * Dynamic discovery of GCP with smart defaults
   * Open-source agents and integrations
* Access to powerful data and analytics tools
* Collaboration with third-party software
Stackdriver:
* Monitoring
* Logging
* Error reporting
* Trace
* Debugger 
Monitoring
Monitoring is at the base of SRE, which is a discipline, which applies aspects of software engineering to operations to create scalable and reliable software systems.
Monitoring
* Dynamic config and intelligent defaults
* Platform, system and application metrics
* Ingests data: metrics, events, metadata
* Generates insights through dashboards, charts, alerts
* Uptime/health checks – uptime checks test the availability of your public services
* Dashboards – dashboards visualize utilization and network traffic
* Alerts - alerting policies can notify you of certain conditions
* Condition
* Notification
* Documentation
* Name of policy
Workspace is the root entity that holds monitoring and configuration information:
* Hosts – Hosting project Z (1st monitored project):
   * Monitoring
   * Dashboards
   * Uptime
   * Configuration
* Workspace Z:
   * Monitors: GCP project A
   * Monitors: GCP project B
   * Monitors: GCP project C <- AWS Connector <- AWS Account Nr1
 


Installing monitoring agent
* Compute Engine
* EC2 instances


$ curl –O https://repo.stackdriver.com/stack-install.sh  
$ sudo bash stack-install.sh --write-gcm
Lab: Resource Monitoring
This lab provisions resources for you
Navigation Menu -> Compute -> Compute Engine -> VM instances you will see three VMs up and running
Navigation Menu -> Compute -> Deployment Manager to see deployment for creating these VMs. See config file, template files, etc
Navigate to monitoring and create a dashboard
Navigation Menu -> Operations -> Monitoring
The workspace is being built for you.


Dashboards -> Create Dashboard:
* Dashboard name: My Dashboard
* Confirm


Add Chart:
* Chart Title: Instance CPU utilization
* Resource Type: GCE VM Instance
* Metric: CPU utilization, Received bytes
* Save


Metrics Explorer allows us to examine Resource Metrics w/o having us create a chart on a dashboard.
Add Alerting Policy
Alerting policies are really important because they allow communication, based on certain metrics.
Alerting -> Create a policy:
* Name: Test
* Add Condition:
   * Resource Type: GCE VM Instance
   * Metric: CPU utilization
   * The condition triggers if: Any time series violates
   * Condition: is above
   * Threshold: 20
   * For: 1 minute
   * Add
* Add Condition:
   * Resource Type: GCE VM Instance
   * Metric: Network traffic
   * The condition triggers if: Any time series violates
   * Condition: is above
   * Threshold: 1000 B
   * For: 1 minute
   * Add
* Triggers when: ANY condition is met
* Add notification channel:
   * Email:
      * Email address: xxx@xxx.xxx
      * Add
* Documentation: Enter optional documentation
* Save
Create group
Groups allow us to combine specific resources and then allow us to create dashboards, alerts, and uptime checks on specific groups.
Create Group:
* Name: VM instances
* Operator: contains
* Value: nginx
* Create
Create uptime checks
Create an uptime check:
* Title: My uptime Check
* Check Type: Http
* Applies to: group
* Group name: VM instances
* Check every: 1 minute
* Save
Logging
* Platform, systems and applications logs
* API to write logs
* 30-day retention
* Log search/view/filter
* Log-based metrics
* Monitoring alerts can be set on log events
* Data can be exported to:
* Cloud Storage,
* BigQuery – analyze logs in BigQuery and visualize in Data Studio (for example, top IP addresses that have exchanged traffic to a web-server. To either relocate a web server or deny access to IP addresses)
* Cloud Pub/Sub – to stream logs to apps or endpoints
Installing a logging agent
* Compute Engine
* EC2 instances


$ curl –sSO https://dl.google.com/cloudagents/install-logging-agent.sh  
$ sudo bash install-logging-agent.sh
4.5 Error Reporting
Aggregates and displays errors for running cloud services
* Error notifications
* Errors dashboards
* Go, Java, Node.js PHP, Python and Ruby
* Compute Engine, EC2, Application Engine
4.6 Tracing
Tracing system:
* Displays data in near real-time
* Latency report
* Per URL latency sampling


Collects latency sampling
* Application Engine
* Google HTTPs load balancers
* Applications instrumented with Stackdriver Trace SDKs
4.7 Debugging
* Inspect an application without stopping it or slowing it down significantly (adds 10 ms to requests)
* Debug snapshots:
   * Capture call stack and local variables of a running application
* Debug log points:
   * Inject logging into a service w/o stopping it
* JAva, Python, Go, Node.js and Ruby










4.8 Lab: Error Reporting and Debugging
* Launch a Google App Engine application
* Introduce an error into the application
* Explore Cloud Error Reporting
* Use Cloud Debugger to identify the error in the code
* Fix the bug and monitor in Cloud Operations
Start Cloud Shell and d-load hello world application
$ gsutil gsutil cp gs://cloud-training/archinfra/gae-hello/* .
$ dev_appserver.py $(pwd)
Web Preview -> Preview on port 8080






Deploy application to app engine
$ gcloud app deploy app.yaml
$ gcloud app browse
Embed a bug
$ cat main.py
$ sed -i -e 's/webapp2/webapp22/' main.py
Redeploy the application to App Engine
$ gcloud app deploy app.yaml –quiet
$ gcloud app browse
Stackdriver
Navigation menu ->  Operations -> Error Reporting
Click the Error name
Stack trace sample -> Parsed