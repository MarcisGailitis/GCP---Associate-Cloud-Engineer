# GCP Fundamentals - Core Infrastructure

## Table of contents

1. Introducing Google Cloud Platform
2. Getting Started with Google Cloud Platform
3. Virtual Machines in the Cloud
4. Storage in the Cloud
5. Containers in the Cloud
6. Applications in the Cloud
7. Developing, Deploying, and Monitoring in the Cloud
8. Big Data and Machine Learning in the Cloud
9. Summary and Review

## 1. Introducing Google Cloud Platform

### 1.1. What's new?

- Improved coverage of Kubernetes Engine
- Added explanation of Anthos, GCP platform for the hybrid cloud, build on Kubernetes
- GCP new regions

### 1.2. Welcome to GCP Fundamentals

GCP offers four main types of services:

- Compute (this course focuses on)
- Storage (this course focuses on)
- Big Data
- Machine learning

### 1.3. What is cloud computing?

- On-demand self-service - no human interaction is needed to get resources
- Broad network access - access from anywhere
- Resource pooling - provider shares resources for customers
- Rapid elasticity - get more resources as needed
- Measured service - pay only for what you consume

### 1.4. How did we get here?

Physical/colo (user configures, manages and maintains) -> Virtualized ((user configures, provider manages and maintaines) -> Serverless (fully automated)

### 1.5. Every company is an IT/data company

### 1.6. GCP computing architectures

IaaS | Hybrid | PaaS | Serverless | Automated Elastic resources
:---: | :---: | :---: | :---: | :---:
Compute engine | Kubernetes Engine | App Engine | Cloud Functions | Managed Services

### 1.7. The Google network

Carries up to 40% of all Internet traffic.

### 1.8. GCP regions and zones

Organization of GCP:

Multi-Region | Region | Zone
--- | --- | ---
Europe | Europe-west2 | Europe-west2-a, Europe-west2-b, Europe-west2-b

Multi-region = Google Cloud Storage allows to place data within Europe multi-region.
Region = Independent geographical areas. At least 15 regions for GCP. Under 5ms b/w zones in the same region.
Zone = Deployment Area for GCP resources. Not strictly a data center, as it may not correspond to the physical building.

### 1.9. Environmental responsibility

All data centers use 2% of the world's electricity.

### 1.10. Google offers customer-friendly pricing

- Sub-hour billing - for compute, data processing and other services
- Discounts for sustainable use - automatically applied to virtual machine use over 25% a month
- Discounts for committed use - pay less for steady, long-term workloads
- Discounts for pre-emptive use - pay less for interruptible workloads
- Custom VM instance types - pay only for the resources you need for your application

### 1.11. Open APIs

- GCP is compatible with open-source services
- GCP uses Open-source for a rich ecosystem
- Multi-vendor-friendly technologies

### 1.12. Why choose Google Cloud Platform

- Compute:
  - Compute Engine
  - Kubernetes Engine
  - App Engine
  - Cloud Functions
- Storage:
  - Bigtable
  - Cloud Storage
  - Cloud SQL
  - Cloud Spanner
  - Cloud Datastore
- Networking:
- Bigdata:
  - Big Query
  - Pub/Sub
  - Data Flow
  - Data Proc
  - Data lab
- Machine learning:
  - Natural Language API
  - Vision API
  - Machine Learning
  - Speech API
  - Translate API

### 1.13. Multi-layered security approach

Security is designed into Google’s technical infrastructure.

- Operational security - intrusion detection systems, employee U2F use
- Internet communication - Google Front End, designed-in Denial of Service protection
- Storage services - encryption at rest
- User Identity - central identity service, which support U2F
- Service Deployment - Encryption of inter-service communication
- Hardware Infrastructure - Hardware design and provenance

### 1.14. Budgets and Billing

- Budgets & alerts, per billing account or per GCP project
- Billing export, export to BigQuery or Cloud Storage
- Reports
- Quotas, for resource consumption (rate quota, allocation quotas)

## 2. Getting Started with Google Cloud Platform

### 2.1. Introduction

You use Projects to organize workloads in GCP.
You use IAM to control who can do what. You use several interfaces to connect.
Projects are the main way to group related resources, with common business objectives. The Principle of Least Privilege, to have as few privileges as possible to do their jobs.

Customer managed vs. Google Managed security for the principle of least privilege.
On Premise -> IaaS -> PaaS -> Managed services.

Four ways to interact with GCPs management layer:

- Web-based console
- command-line tools,
- APIs
- Mobile App

### 2.2. The Google Cloud Platform resource hierarchy

<!-- TODO add resource hierarchy image -->

All the resources you use (VMs, Cloud Storage buckets, tables in BigQuery) are organized in projects. Projects might be organized in folders.
Resources -> Projects -> Folders (Nested Folders) -> Organization Node.

Projects, Folders, and Org Node are all places, where policies can be applied. Policies are inherited downwards in the hierarchy.

Projects are the basis for enabling and using GCPs services, like managing APIs, enabling Billing, adding/removing collaborators, and enabling other GCPs services:

- Project ID: first-cloud-steps-277609, assigned by you, immutable, globally unique
- Project Name: First Cloud Steps, assigned by you, mutable, not unique
- Project number: 29064997144, assigned by GCP, immutable, globally unique

Folders offer flexible management:

- Folders group projects under and organization
- Folders can contain projects, other folders or both
- Use folders to assign policies

Organization Node:

- The organization node organizes projects
- The organization node is the root node for google cloud resources

IAM resource hierarchy:

- A policy is set on a resource
- Resources inherit policies from parent
- Less restrictive parent policy overrides a more restrictive resource policy

### 2.3. Identity and Access Management (IAM)

IAM policy has a who part, can do what part and on which resource part

- Who = Google acc, Google group, Service acc, Gsuite, Cloud identity domain
- Can do what = IAM role:
  - primitive (Owner, Editor, Viewer, Billing)
  - predefined (apply to a particular GCP service in a project, like an Instance Admin role (list, delete, get, start, strop, set)), custom
- On which resource

- Service Account for server-to-server authentication

### 2.4. IAM roles

IAM roles:

- Compute Engine’s InstanceAdmin Role: (start, stop, list, get, delete, setMachine type)
- Custom role: Compute Engine’s InstanceOperator Role: (start, stop, list, get)
- Service Account for server-to-server interactions

### 2.5. Interacting with Google Cloud Platform

For ways to interact with GCP:

- Cloud Platform Console, web-based administrative interface, offers Cloud Shell
- Cloud Shell (a command-line interface to GCP, accessible from a web browser) 
- SDK (gcloud, gsutil (Cloud Storage), bq (BigQuery))
- Cloud Console Mobile App (managed machines + dashboards)
- REST APIs (JSON, OAuth2)

### 2.6. Cloud Marketplace (formerly Cloud Launcher)

### 2.7. Demonstration:Getting Started with Cloud Launcher

### 2.8. Getting started with GCP and Qwiklabs

### 2.9. GCP Fundamentals: Getting Started with Cloud Marketplace




GCP APIs Explorer
Cloud Marketplace (formerly Cloud Launcher)
GCP Cloud Launcher:
* Marketplace with pre-packaged solutions for VMs
* No additional charge for open source software
Lab: GCP Fundamentals: Getting Started with Cloud Marketplace


3) VMs in the cloud
Intro
Compute Engine & VMs.
Virtual Private Cloud (VPC) Network
* VPC network is contained in a GCP project
* You can provision GCP resources, connect them or isolate them
* VPC Networks are global, subnets are regional.
* Load balancing
Compute Engine
Compute Engine offers managed virtual machines:
* Select the CPU, GPU, TPU, boot image, disk, startup script.
* Snapshots as backups
* Autoscaling
* Load Balancing with VPCs
VPC capabilities
VPC Network capabilities:
* Routing tables, to forward traffic from one instance to another within the network, across subnets
* Use its firewall to control what network is allowed
* Shared VPC to share a network with other GCP projects
* VPC Peering to interconnect networks in GCP projects
* Cloud Load Balancing:
   * Single anycast IP address
   * Traffic fees over the Google backbone from the closest point of presence to the user
   * Backends are selected based on load
   * Only healthy backends receive traffic
   * No prewarming
   * Global HTTP/S, Global SSL, Global TCP, Regional, Regional Internal (load balancing of traffic inside a VPC)
* Cloud DNS
* Cloud CDN (Content Delivery Network) for lower latency
   * Use Google’s globally distributed edge caches to cache content close to your users
* Interconnect options:
   * VPN
   * Direct Peering
   * Carrier Peering
   * Dedicated Interconnect
Lab: GCP Fundamentals: Getting Started with Cloud Compute
List available zones in GCP
gcloud compute zones list | grep us-central1
Add default
gcloud config set compute/zone us-central1-a
Create a new VM instance
gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"
Retrieves webpage
curl http://localhost/
curl http://my-vm-1/
4) Storage in the Cloud
Into to GCP Storage Options
Different applications and workloads require different storage database solutions:
* Video to be stream
* Sensor data from devices
* Customer account balance
* Game data


You can store data on your VMs persistent disk. But, GCP has other storage options, for structured, unstructured, transactional, and relational data


Cloud Storage, Cloud SQL, Cloud Spanner, Cloud Datastore, and Google Bigtable. You might want to use one or several options for these services to get the job done.
Cloud Storage
Object storage is not the same as file storage, where you manage your data as a hierarchy of folders.
Object storage is not the same as block storage, where the OS manages data as chunks of disk.
Instead, object storage means you say to your storage ‘here, keep these bytes I give you’, and the storage lets you address it with a unique key.
Often these unique keys are in a form of URLs, which means object storage interacts nicely with web technologies. 


Cloud storage works just like that, just better! Fully manages scalable service, so no need to provision capacity ahead of time.


Use Cloud Storage for:
* website content, 
* storing data for archival and disaster recovery, or 
* distributing large data objects to end-users via direct download.
 
Cloud storage is not a file system, each object has a URL. It’s ok to use file/folder informally to describe objects, but it is not a file system!


Cloud Storage is comprised of buckets, you create/configure/use them to hold objects. Objects are immutable, so you create new versions of them if you modify them.


Cloud Storage always encrypts your data on the server-side.


Cloud Storage: buckets & objects


Bucket attributes
	Bucket Content (objects)
	globally unique name
	Files (in a flat namespace)
	storage class
	

	geographic location (multi-region, region)
	

	IAM or Access Control Lists
	Access Contol Lists
	versioning setting (on/off)
	

	lifecycle management rules 
(delete older than 1 yr, keep only 3 last versions)
	

	

Access Contol Lists (ACL) = Scope (users), Permissions
Cloud Storage Interactions
Storage classes:
* Multi-regional - content storage and delivery
* Regional – in-region analytics, transcoding
* Nearline – long-tail, backup, accessed at most 1x month
* Coldline – Archive, disaster, accessed at most 1x year
 
Data to Cloud Storage:
* Gsutil or drag/drop in GCP Console
* Storage Transfer Service (online) – scheduled, managed batch transfers from other cloud Provider, different region, HTTPs endpoint
* Transfer Appliance (offline) – rackable appliances to ship data
 
Other GCP services <-> Cloud Storage:
* BigQuery – Import/Export tables
* App Engine – Object storage, logs and Datastore backups
* Compute Engine – Startup Scripts, images, and general object storage
* Cloud SQL – Import/Export tabless
Google Cloud Bigtable
Fully managed NoSQL, “big data” or “wide-column” database service.
SQL = same columns for each row. NoSQL = not all rows have the same nr of columns.


Cloud Bigtable:
* Billions of rows,
* thousands of columns,
* petabytes of data
* “Persistent hash table”
* Accessed using HBase API
* Native compatibility with big data, Hadoop ecosystem
Why Cloud Bigtable vs. Apache HBase installation on Compute Engine’s VM?
* Managed, scalable storage
* GCP handles administration tasks, like upgrades and restarts
* IAM access
* Encryption
* Powers Google’s search, Maps, Gmail
 
Bigtable access:
* Application API, for example, HBase REST Server
* Streaming, Spark Streaming
* Batch processing, Hadoop MapReduce, Spark
Google Cloud SQL and Google Cloud Spanner
Cloud SQL
Relational Database Services use database schema for data consistency and correctness
Database Transactions (all/nothing, either all changes made or none of them)


Takes a lot of time to set up, maintain, manage, administer


MySQL, PostgreSQL as a Service (PaaS)
Cloud SQL vs. MySQL on a virtual machine:
* Automatic replication
* Managed Backups
* Vertical/Horizontal scaling
* Google Security
* Encryption
* Accessible by other GCP instances (App Engine, Compute Engine, external applications)
Cloud Spanner
* For horizontal scalability
* Use Spammer when you have outgrown relational database, transactional consistency or want to consolidate databases
Google Cloud Datastore
* Highly scalable NoSQL database choice
* Designed for application backends
* Supports transactions
* Includes free daily quota


Usecases:
* To store structured data from App Engine Apps
* Cloud Datastore for integration point b/w App Engine and Compute Engine
Comparing Storage Options 
  
  

Lab:  Cloud Storage and Cloud SQL
Create a Compute Engine instance –> startup script
apt-get update
apt-get install apach2 php php-mysql-y
service apache2 restart
Create a new bucket in  Cloud Shell:
gsutil mb -l US gs://$DEVSHELL_PROJECT_ID
gsutil mb -l location name


location = multiregional/regional
name = globally unique name, like gcp_project_id, Cloud Shell variable for project_id gs://$DEVSHELL_PROJECT_ID
Access Control List of the object you just created so that it is readable by everyone


gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
5) Containers in the Cloud
Containers, Kubernetes, and Kubernetes Engine


IaaS
	

	Paas
	Compute Engine
	Kubernetes Engine
	App Engine
	Servers, file systems, networking
	

	Present runtimes, managed services
	

Kubernetes Engine is like IaaS in that it saves you infrastructure shores.
Kubernetes Engine is like PaaS in that it was built with developers in mind.
Containers
IaaS let you share compute resources with others by virtualizing the hardware. Each VM has its own instance of an OS of your choice, and you can build and run applications on it, with access to memory, file systems, networking, etc. BUT flexibility comes with a cost, as the smallest unit of compute engine is VM, and with the application, there is also OS (GB ins size), and it can take minutes to boot up. So the scaling might be an issue, as it will contain the unneeded OS.


In PaaS, you get access to the family of services that apps need. So all you do is write your code in self-contained workloads, that use these services and include any dependent libraries. As demand for app increases, platform scales your app seamlessly, BUT you give up control of underlying server architecture.


That is where containers come in. 
Independent scalability of workloads like you get in PaaS and an abstraction layer of the OS and hardware like you get in an IaaS. Containers get you an invisible box around your code and it's dependencies, with limited access to underlying file system and hardware.
A process is an instance of a running program. The container starts as quickly as a new process. Compare that to booting a new instance of an OS. All you need on each host is OS that supports containers and container runtime.
You are virtualizing the OS rather than the hardware. The environment scales like PaaS, it gives you almost the same flexibility as IaaS.


The container abstraction makes your code very portable, you can treat the OS and hardware as a black box.
So you can move your code from dev to staging to prod. env. or from laptop to the cloud without changing or rebuilding it.


If you want to scale a web server you can do so in seconds and deploy hundreds of them on a single host.
You want to deploy your application using lots of containers, each performing their own function using a microservices pattern. The units of code in those containers can communicate with each other over some network. In this case, you have made an application modular. They deploy easily and scale independently. And the Host can scale up and down, start and stop containers, as demand for your app changes or as hosts fail and are replaced. A tool that does these tasks well is Kubernetes.


Kubernetes makes it easy to orchestrate many containers on many hosts, scale them, roll out new versions of them, or roll back changes.
How to build containers using Docker
GCP offers Cloud Build, a managed service for building containers.


app.py
from flask import Flask
app=Flask(__name__)


@app.route("/")
def hello():
    return "Hello World\n"


@app.route("/version")
def version():
    return "Helloworld 1.0\n"


if __name__ == "__main__":
    app.run(host='0.0.0.0')


requirements.txt
Flask==0.12
uwsgi==2.0.15


Dockerfile
FROM Ubuntu:18.10
RUN apt-get update -y && \
         apt-get install -y python3-pip python3-dev
COPY requirements.txt /app/requirements.txt
WORKDIR /app
RUN pip3 install -r requirements.txt
COPY . /app #copy the files that make up our application
ENTRYPOINT ["python3", "app.py"] #how to run it


Build and run
$ docker build -t py-server #builds the container and stores on a local system as a runnable image.
$ docker run -d py-server #to run the image


In the real world, you would upload the container to a container registry system, like GCP Container Registry, and share or download it from there.
Introduction to Kubernetes and GKE
I'll show you where Kubernetes comes in. Kubernetes is an open-source orchestrator for containers, so you can better manage and scale your applications. Kubernetes offers an API that let's control it's operations, through several utilities. Kubernetes lets you deploy containers on a set of nodes called a cluster. 
What is a  cluster?
Master + nodes (VMs)
Cluster is a set of:
*  master components that control the system as a whole and
* a set of nodes that run containers. 
In Kubernetes a node represents a computing instance, in GCP a node is a VM, running in Compute Engine.
Using Kubernetes you can describe a set of applications and how they should interact with each other and Kubernetes figures out how to make it happen.
Kubernetes makes it easy to run containerized applications.
How do you get a Kubernetes cluster?
You can always build one yourself on your hardware, but then you would have to maintain it, which might not be the best use of your time. GCP offers the Kubernetes Engine as a managed service. You can create a Kubernetes cluster with Kubernetes Engine using the GCP console or the gcloud command. GKE clusters can be customized and they support different machine types, numbers of nodes, and network settings.


$ gcloud container clusters create k1 
You will have a cluster called k1, complete, configured, and ready to go. 
How do you add nodes to a Kubernetes cluster?
What is a pod?
Whenever Kubernetes deploys a container or related containers it does so in a pod abstraction. A pod is the smallest deployable unit in Kubernetes. Think of a pod as a running process on your cluster, it could be one component of your application or even an entire application. If you have multiple containers per pod, they will automatically share networking, share disk storage. Each pod in Kubernetes gets a unique IP address and set of ports for your containers. As containers in pods can communicate with each other using the localhost network interface they do not care about on which nods they are deployed on.
One way to run a container in a pod in Kubernetes is to use the kubectl run command.


$ kubectl run nginx --image=ngix:1.15.7 
Starts a deployment with a container running in a pod. The container running inside a pod is an image of an nginx server. Kubectl fetches the image & version from a Container Registry.
So, what is a deployment?
Deployment represents a group of replicas of the same pod. It keeps your pods running, even if a node on which some of them run on fails.
You can use a deployment to contain a component of your application or even the entire application. To see the running pods run:
$ kubectl get pods


By default pods in a deployment are only accessible inside your cluster. To make pods in your deployment publicly available you can connect a load balancer to it, but running a kubectl expose command:
$ kubectl expose deployments nginx --port=80 --type=LoadBalancer
Kubernetes then creates a service with a fixed IP address for your pods.


Service is the fundamental way Kubernetes represents load balancing. You requested Kubernetes to attach an external load balancer with a public IP address to your service so that others outside the cluster can access it.
In GKE this kind of load balancer is a Network LoadBalancer. This is one of the managed load balancing services that compute engine makes available to VMs. GKE makes it easy to use it with containers.
Everyone accessing the public IP will be routed to a pod behind the service.
So, what is a service?
A service group a set of pods together and provides a stable endpoint for them.


Why do you need a service? Why not just use the pod's IP address directly?
Yes, but it would make a management problem. As deployments create and destroy pods, pods get their IP addresses, but those IP addresses don't remain stable over time. Services provide that stable endpoint you need.


$ kubectl get services 
shows you public IP address


$ kubectl scale nginx --replicas=3
to scale deployment, three web servers in this case, all behind a service
$ kubectl autoscale nginx --min=10 --max=15 --cpu=80 
to autoscale based on CPU usage.
Declarative mode
So far just imperative commands (like, expose, and scale), but the real strength of Kubernetes comes, when you work in a declarative way. Instead of issuing commands, you provide a configuration file that tells Kubernetes what you want your desired state to look like, and Kubernetes figures out how to do it.
These configuration files then become your management tools, to make a change you edit a file and then present the changed version to Kubernetes. And save them in a version control system to keep track of the changes you made to your infrastructure.


$kubectl get pods -l "app=nginx" -o yaml


apiVersion: v1
kind: Deployment
metadata:
     name: nginx
     labels:
         app: nginx
spec:
    replicas: 3
    selector: # so your deployment knows how to group-specific pods as replicas
        matchLabels:
            app: nginx #it works because all those pods share a label, the app is tagged as nginx
    template:
        metadata:
            labels:
                app: nginx
        spec:
            containers:
                - name: nginx
                  image: nginx: 1.15.7
                  ports:
                  - containerPort: 80


$ kubectl apply -f nginx-deployment.yaml #to use the updated config file
$ kubectl get replica sets #to view your replicas and their state
$ kubectl get pods # status of pods
$ kubectl get deployments #status of deployment
$ kubectl get services
How to update the version of your application?
spec: 
  # …
  replicas: 5
  strategy:
    rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
  type: RollingUpdate


One attribute of deployment is its update strategy. A rolling update will create pods of the new version, one by one, waiting for each new version pod to become available, before destroying one of the old version pods. Rolling updates are a quick way to push out your application while spearing users of experiencing downtime.
Introduction to hybrid and multi-cloud Computing (Anthos)
Containers on hybrid-cloud and multi-cloud architecture.


Typical on-premises distributed system architecture.
  



Containers have become a popular way of breaking these applications down into micro-services so that they can be easily maintained and expanded. Traditionally, these systems have been hosted on-premises, somewhere in the company's network or company's data center (server update, installation, network changes, configuration, etc. might require from several months to years). Costly to upgrade, as upgrades are expensive and short lifecycle of the server.


Modern distribution systems allow a more agile approach to managing your computing resources. It allows:
* Move only some of your compute workload to the Cloud
* Move at your own pace
* Take advantage of clouds scalability and lower costs
* Add specialized services to your compute resources stack (machine learning, content caching, data analysis, long term storage, IoT, etc.)


Anthos is Google's modern solution for hybrid and multi-cloud systems and service management.
* Kubernetes and Google Kubernetes Engine on-prem create the foundation
* On-premises and cloud environments stay in sync.
* A rich set of tools is probably fixed for:
   * Managing services on-premises and in the Cloud
   * Monitoring systems and services
   * Migrating applications from VMs into your clusters
   * Maintaining consistent policies across all clusters, whether on-premises or in the Cloud


GKE for production-ready apps
  

First, let's look at GKE on the Cloud. 
GKE is a managed prod. ready env. for deploying containerized applications.  Includes:
* Auto node repair
* Auto upgrade
* Autoscaling
Uses regional clusters, for high availability with multiple masters. Node storage replication across multiple zones (currently 3).
GKE On-Prem is turn-key production-grade Kubernetes
  

GKE counterpart on the on-premises network is GKE On-Prem. 
* Provides an easy upgrade path to the latest Kubernetes releases, that have been validated and tested by Google. 
* Provides access to container services on GCP, such as Cloud Build, Container Registry, Audit Logging, and more. 
* Integrates with Istio and Marketplace Solutions.
* Ensures consistent Kubernetes version across cloud and on-premise environments.
Marketplace applications are available to all clusters
  

Both GKE and GKE On-Prem integrates with GCP Marketplace so that all of the clusters on the network have access to the same repository of containerized applications. 
This allows you to use the same configurations on both sides of the network, reducing the time spent developing applications. Wite once, replicate anywhere.
Service Meshes make apps more secure & observable
  

Enterprise applications might use 100s of microservices to handle computing workloads. Keeping track of all of these services and monitoring their health can quickly become a challenge. Anthos Service Mesh and Istio Open Source Service Meshes take all of these work of managing and securing your microservices. These service mesh layers can communicate across hybrid-network using Cloud Interconnect to sync and pass heir data.
Stackdriver logging and Monitoring watch all sides
  

Stackdriver is built-in logging and monitoring solution for GCP. Stackdriver offers a fully managed logging, metrics collection, monitoring, dashboarding, and alerting solution, that watches all sides of your hybrid or multi-cloud network. 


Stackdriver is the ideal solution for customers wanting a single, easy to configure, powerful, cloud-based solution that gives you a single dashboard to monitor all of your environments. 
Configuration Manager is the single source of truth
  

Lastly, Anthos Configuration Management provides a single source of truth for your cluster’s configuration. 
That source of truth is kept in a Policy Repository, which is actually a Git repository. 
In this illustration, this repository happens to be located on-premises, but it can also be hosted in the cloud.
The Anthos configuration Management agents use the Policy Repository to enforce configurations locally in each environment, managing the complexity across environments. Anthos Configuration Management also provides the ability to deploy code changes with a single repository commit. And the option to implement configuration inheritance, by using namespaces.
Lab: getting started with Kubernetes Engine
Enable APIs:
Open APIs and Services -> Dashboard:
* Google Kubernetes Engine API -> Enable
* Google Container Registry API -> Enable
Start a cluster:
$ export MY_ZONE=us-central1-a
$ gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
$ kubectl --version


Remember that Kubernetes Engines nodes are VMs so you can go to Compute Engine to view them.
You can also view the Cluster in Kubernetes Engine -> Kubernetes clusters
Run a web server on the cluster:
The deployment consists of a single pod. 


$ kubectl run nginx --image-nginx:1.10.0


Confirm that it is running:
$ kubectl get pods
Expose the deployment:
$ kubectl expose deployment nginx --port 80 --type LoadBalancer
View the new service:
$ kubectl get services
Scale-up deployment 
(you would scale up if the load was rising):
$ kubectl scale deployment nginx --replicas 3
Now let’s look at new number of pods:
$ kubectl get pods
6) Applications in the cloud
Intro to App Engine
We have discussed two GCP products that provide compute infrastructure for applications:
Compute Engine &Kubernetes Engine, where you choose infrastructure of which your application runs (VMs for Compute, Containers for Kubernetes).
If you do not want to focus on infrastructure at all, but want to focus on your code, use App Engine for that.


PaaS = Platform as a Service. The App Engine manages the hardware and networking infrastructure, required to run the code. To deploy an application on App Engine, you just add your code to App Engine and App Engine service takes care of the rest.


Built-in services:
* NoSQL database
* In-memory caching
* Load balancing
* Health-checks
* Logging
* Way to authenticate users


App Engine will automatically scale your application in response to the amount of traffic it receives. You only pay for those resources you use, no servers to maintain.


App Engine is especially suited for applications, where the workload is highly variable or unpredictable, like web apps and mobile backend.


App Engine offers two environments:
* Standard
* flexible
App Engine Standard Environment, sanbox
Standard env is the simpler one
* Simpler deployment
* fine-grain autoscale
* Free daily quota, Low utilization applications might be able to run at no charge.
* Usage-based pricing


App Engine SDK for development (locally), testing (locally), and deployment.


Use App Engine’s standard environment runtime, provided by Google, to run your code:
* Java
* Python
* PHP
* Go


If your application is in other languages, you will have to use App Engine’s flexible environment.
If your application is not in sandbox mode (software independent of hardware, OS, or physical location, no writing to the local file system, you will have to write to the DB system instead, no third-party software (aka. no libraries)), you will have to use App Engine’s flexible environment.


1. Develop and test locally, using SDK
2. Deploy to App Engine using SDK
3. Eech App Engine application runs in a GCP project
4. App Engine automatically scales & load balances them
5. Application can make calls to services using APIs:
   1. NoSQL to make data persistent
   2. caching of the data using memcached
   3. searching, logging, user logging
   4. ability to launch actions, not triggered by user requests, like:
      1. task queues and 
      2. task scheduler
App Engine Flexible Environment, Container
If the standard environment is not for you, but you want to have the benefits of App Engine, use a flexible environment, where instead of a sandbox you can specify the container you App Engine runs in.
You application runs inside a Docker Container on Google Compute Engine’s VM. 
App Engine manages these Compute Engine VMs for you:
* health checked, 
* healed 
* you choose in which geographic location they are run in
* critical, backwards compatible updates are automatically applied
So you can just focus on your code.


App Engine flexible environment use standard runtime, can access App Engine services, like:
* Datastore
* Memcased
* Task queues
* and so on,


  

  

Google Cloud Endpoints and Apigee Edge
  

Application Programming Interface (API) - Developers structure their code in a clean, well defined interface, that abstracts away needless details, and then they document that interface. That is an API. In order to add/remove features to an API as cleanly as possible, developers version their APIs. 


Two API management tools in GCP. 
Cloud Endpoint 
helps you create and maintain an API.
* Distributed API management through an API console
* Expose your API using RESTful interface
* Control access and validate calls with JSON Web Tokens and Google API key
* Generate client libraries
Supported platforms:
* App Engine flexible
* Kubernetes Engine
* Compute Engine
Apigee Edge
Helps you secure and monetize APIs
* A platform for making APIS available to your customers and partners
* Contains analytics, monetization and developer portal
Lab: Getting Started with App Engine


$ cloud config set project "project id"
Clone simple App Engine application from GitHub
$ git clone https://github.com/GoogleCloudPlatform/appengine-guestbook-python
$ cd appengine-guestbook-python
Examine app.yaml
$ cat app.yaml
Specifies the runtime as well as handlers and required libraries. Handlers = URLs on which it responds
Run the application locally in Cloud Shell, 
using build it app engine SDK
$ dev_appserver.py ./app.yaml
Use Cloud Shell built-in preview
Press on “web preview”
Deploy application to App Engine service
$ gcloud application deploy ./index.yaml ./app.yaml
Select region
Confirm deployment
App Engine files are stored in Cloud Storage
Refresh App Engine Dashboard to see deployed application 
Click on link
Application Engine -> Settings -> Disable application
To disable
7) Developing, Deploying and Monitoring in the Cloud
Development in the Cloud
You also have tools that are tightly integrated with GCP, and in this module, we will explain them.


Cloud Source Repository
Fully featured Git repository hosted on GCP


* code is kept private for the GCP project 
* uses IAM permissions to protect it 
* not to have to maintain git instance by yourself. 
Cloud Functions
Create single-purpose functions that respond to events without a server of runtime
* Image processing - user uploads an image, you process it and save it
* Write code in JavaScript, execute in node.js environment 
* Pay-per-use
* Cloud Functions can trigger events in Cloud Storage, Cloud Pub/Sub, or on HTTP call.
Deployment: Infrastructure as a Service
Deployment Manager
Setting up your environment in GCP can entail many steps:
* Setting up compute instances
* Setting up network and storage
* Keeping track of their configurations


You can do this all by hand, imperative way, but there is a better way for that, by using a template and specifying how the environment should look like in a declarative way.


* Provides repeatable deployments
* Create a .yaml template, describing your environment and use Deployment Manager to create resources
* You can store and version control your deployment manager templates in Cloud Source Repositories
* Infrastructure management service that automates the creation and management of your GCP resources for you.
Monitoring: Proactive Instrumentation
You cannot run your application stably without monitoring it. 
Monitoring lets you figure out if the changes you made were good or bad.
It lets you respond with information rather than panic when your application is down.


Stackdriver is a GCPs tool for monitoring, logging, and diagnostics:
* Monitoring
* Logging
* Debug
* Error Reporting
* Trace


Stackdriver gives you access to many kinds of signals from your:
* Infrastructure platforms, 
* VMS, 
* containers, 
* middle-were and 
* application tier, 
* logs, metrics, and traces


It gives you insight into your application’s health, performance, and availability, so if issues occur, you can fix them faster.


Core components of Stackdriver:
* Monitoring
   * Platform, system and application metrics
   * Uptime/health checks
   * Dashboards and alerts
* Logging
   * Platform, system and application logs
   * Log search, view, filter, and export
   * Log-based metrics
* Trace
   * Latency reporting and sampling
   * pre-URL latency and statistics
* Error Reporting
   * Error notifications
   * Error dashboard
* Debugger
   * Debug applications
* Profiler
   * Continuous profiling of CPU and memory consumption
Lab: Getting Started with Deployment Manager and Stackdriver
Define a variable with preferred GCP zone
$ export MY_ZONE=us-central1-f
$ echo $DEVSHELL_PROJECT_ID


Create .yaml file
$ nano deploy.yaml
resources:
- name: my-vm
  type: compute.v1.instance
  properties:
    zone: ZONE
    machineType: zones/ZONE/machineTypes/n1-standard-1
    metadata: 
      items:
      - key: startup-script
        value: “apt-get-update”
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams: 
        sourceImage: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-9-strecch-v201709$
    networkInterfaces:
    - network: https://www.googleapis.com/compute/v1/projects/PROJCECT_ID/global/networks/defaults
      accessConfig:
      - name: External NAT
        type: ONE_TO_ONE_NAT


substitute Zone and Project_ID in .yaml file
$ sed -i -e ‘s/PROJECT_ID/’$DEVSHELL_PROJECT_ID/ deploy.yaml
$ sed -i -e ‘s/ZONE/’$MY_ZONE/ deploy.yaml


Build a deployment from template
$ gcloud deployment-manager create my-first-depl --config deploy.yaml
Verify status
$gcloud deployment-manager deployments list
Check deployment in console
Compute Engine -> VM instances -> my-vm is created
Make a change to template’s startup script
        value: “apt-get-update; apt-get install nginx-light -y”
Update a deployment from template
$ gcloud deployment-manager deployments update my-first-depl --config deploy.yaml
Add CPU load to VM instance
ssh into VM machine
$ dd if=/dev/urandom | gzip -9 >> /dev/null & 
Set up Stackdriver Monitoring
Stackdriver -> Monitoring
Create Account -> Continue 
Launch Monitoring -> Continue with trial
Big Data and Machine Learning in the Cloud
Intro to Big Data and Machine Learning
In the future, every company will be a data company! Making the fastest and best use of data is a critical source of competitive advantage.


GCP provides a way for everybody to take advantage of Google investments in infrastructure and data processing innovation. GCP has automated the complexity of building and maintaining data and analytics systems.
Google Cloud Big Data Platform
Google Cloud’s big data services are fully managed and scalable. Integrated, serverless platform.
Serverless = You do not have to worry about provisioning compute instances to run your jobs. The services are fully managed, and you pay only for the resources you consume.
Integrated platform = GCP data services work together to help you create custom solutions




  

Cloud Dataproc
Managed Hadoop MapReduce, Spark, Pig, and Hive service. 
Cloud Dataflow
Stream and batch processing; unified and simplified pipelines
BigQuery
Analytics database; stream data at 100 000 rows per second
Cloud Pub/Sub
Scalable and flexible enterprise messaging
Cloud Datalab
Interactive data exploration
Cloud Dataproc
Apache Hadoop is an open-source framework for big data. Based on the MapReduce programming model:
* Map function runs in parallel with a massive data set, to produce intermediate results, 
* reduce function builds a final result set based on those intermediate results. 
* Hadoop is used informally to encompass Apache Hadoop itself and it’s related projects, such as Spark, Pig, and Hive.


* Cloud Dataproc is a fast, easy, managed way to run Hadoop and its projects on GCP. 
* Run-on Compute Engine’s VMs.
* You can scale it up or down
* Monitor your cluster using Stackdriver
* Pay only for hardware (VMs) used while running the cluster
* Preemptible instances (80% cheaper)
* Spark and Spark SQL to perform data mining on cluster
Cloud Dataflow
Dataproc is great when you have a dataset of known size. Or when you want to manage cluster size yourself.
But what is your data shows up in real-time? Or it is in unpredictable size or rate?
That is where Cloud Dataflow is a good choice. It is both:
*  a unified programming model and 
* a managed service.


It lets you develop and execute a big range of data processing patterns:
* extract, transform and load, 
* batch computation, and continuous computation.


* You use Dataflow to build data pipelines and the same pipelines work for both batch and streaming data.
* No need to spin up a cluster or to size instances, 
* Cloud Dataflow fully automated the management of whatever processing resources are required
* Cloud Dataflow frees you from operational tasks, like resource management and performance optimization


Dataflow pipeline example:
* Source - reads data from BigQuery table,
* Transform - Processes it in a variety of ways (map + reduce operations)
* Sink - writes output to Cloud Storage


Why use Cloud Dataflow?
* ETL (extract/transfor/load) pipelines to move, filter, enrich, shape data
* Data analysis: batch computation or continuous computation using streaming
* Orchestration: creates pipelines that coordinate services, including external services
* Integrates with GCP services, like Cloud Storage, Cloud Pub/Sub, BigQuery and BigTable
* Open-source Java and Python SDKs
BigQuery
Suppose instead of a dynamic pipeline, your data needs to run more in the way of a vast sea of data. You want to do ad hoc SQL queries on a massive data set. That is what BigQueryis for. 
* It is Google’s fully managed Petabytes scale, low-cost analytics data warehouse.
* No infrastructure to manage, so you can focus on analyzing data to find meaningful insights
* Use familiar SQL
* Pay-as-you-go model


To get data into BigQuery:
* Load it from Cloud Storage
* Load it from Cloud Datastore
* Strem it into BigQuery


Once it is into BigQuery:
* Run super-fast SQL queries 
* against multiple terabytes of data
* Using the processing power of Google’s infrastructure
* You can read/write data in BigQuery via Cloud Dataflow, Hadoop and Spark
Cloud Pub/Sub
Pub = publishers
Sub = Subscribers


Cloud Pub/sub is scalable, reliable messaging
* Support many-to-many asynchronous messaging
* Application components make push/pull subscriptions to topics
* Includes support for offline consumers
Why Pub/Sub
* Building block for data ingestion in Dataflow, IoT, Marketing Analytics
* Foundation for Dataflow streaming
* Push notifications for cloud-based applications
* Connect applications across GCP (push/pull b/w Compute Engine and App Engine)
Cloud Datalab


aka. Jupyter Notebooks,
runs on Compute Engines VM


* An interactive tool for large-scale data exploration, transformation, analysis and visualization
* Integrated, open-source (built on Jupyter Notebooks)
* Analyze data in BigQuery, Compute Engine and Cloud Storage using Python, SQL, and JAvaScript
* Easily deploy models to BigQuery
Google Cloud Machine Learning Platform
ML is one branch of the field of AI. It is a way of solving problems, without explicitly coding the solution. Instead coders built systems that improve themselves over time, through repeated exposure to sample data, which we call training data.


Google machine-learning platform is available as a cloud service, so you can add innovative capabilities to your applications. 
Cloud ML 
The platform provides modern machine-learning services with pre-trained models and a platform to generate your own tailored models.


There is a range is services that stretch from highly general to the pre-customized.


TensorFlow is an open-source software library, that is exceptionally well suited for machine-learning applications like neural networks.
You can run TensorFlow wherever you like, but GCP is an ideal place for it because ML models need lots of computing resources and lots of training data. 
TensorFlow can take advantage of Tensor processing units - hardware devices designed to accelerate ML workloads with TensorFlow. GCP makes them available in the cloud with Compute Engine’s VMs. Each cloud TPU provides up to 180 teraflops of performance (MacBook Pro up to 4 teraflops). 
Why use Cloud ML
* For structured data:
   * Classification and regression
   * Recommendation
   * Anomaly detection
* For unstructured data:
   * Image and video analytics
   * Text analysis
Machine Learning APIs
Cloud Vision API
Analyze Images with a simple Rest API
* Logo detection, label detection, etc
* Gain insight from images
* Detect inappropriate content
* Analyze sentiment
* Extract text


Cloud Natural Language (Speach) API
* Can return text in real-time (80+ languages)
* Highly accurate, even in a noisy environment
* Access from any device
* Uses ML models to reveal the structure and meaning of the text
* Extract information about items mentioned in text documents, news articles and blog posts
Cloud Translation API
* Translate arbitrary strings b/w thousands of language pairs
* Programmatically detect the language of a document
* Support for dozens of languages
Cloud Video Intelligence API
* Annotate the content of videos
* Detect scene changes
* Flag inappropriate content
* Support for a variety of video formats
Lab: Getting Started with BigQuery
Load data from Cloud Storage into BigQuery
Open BigQuery in GCP Console
Create a new dataset, name a dataset, select location
Add a table, use Google Cloud Storage location, select bucket; name table; use Schema
Click on the table name to view schema, preview to see data
Perform a query using BigQuery web user interface
Open Compose Query
select int64_field_6 as hour, count(*) as hitcount from logdata.accesslog
    group by hour
    order by hour
Perform a query using CloudShell or SDK
$ bq query “select string_field_10 as request, count(*) as requestcount from logdata.accesslog group by request order by requestcount desc”
Review