# Preparing for the Google Cloud Associate Cloud Engineer Exam

## 0 About the Associate Cloud Engineer Certification

### Devising a study strategy

1 Understand the scope of the exam you plan to take

Associate Cloud Engineer (ACE):

* Build
* Deploy
* Manage

ACE exam test:

* Setting up  a cloud solution environment
* Planning and configuring a cloud solution
* Deploying and implementing a cloud solution
* Ensuring successful operation of a cloud solution
* Configuring access and security

### Devising a study strategy, cont1

2 Gather your list of resources
Exam guide: <https://cloud.google.com/certification/guides/cloud-engineer>

### Devising a study strategy, cont2

3 Find out know you know and make note of what you do not
Use exam guide for that

4 put it all together into a plan you can commit to!

### Are you ready

45 min or less, 80% of practice exam

## 1 Setting up a cloud solution environment

### Welcome to Module 2

1 Assess your skill level
2 Update the plan
3 Create a list uf resources

### 1.1 About cloud projects and accounts, pt1

* Creating projects
* Assign user to predefined IAM role
* Link users to G Suite identities
* Enabling APIs withing projects
* Provisioning one or more Stackdriver accounts

* Creating projects
  * Resource Hierarchy basics
  * GCP services are associated with project
  * Creating project: ID, name (mutable), number
  * Folders group project and policies

* Assign user to predefined IAM role
  * Permission hierarchy
  * Understanding roles in GCP
  * IAM primitive roles, to a project
  * IAM predefined roles, to a resource
  * predefined > primitive

* Link users to G Suite identities
  * Managing your GCP admin users
  * Gmail accounts
  * G Suite
  * Cloud Identity

* Enabling APIs withing projects
  * RESTful APIs Enable through GCP Console
  * APIs Explorer

* Provisioning one or more Stackdriver accounts
  * Stackdriver
  * Build-in monitoring (Compute, Kubernetes, SQL, App Engine, etc)
  * Stackdriver logs (Admin log 400 days, Data log 30 days)

### 1.2 Billing management overview

* Creating one or more billing accounts
* Linking projects to a billing account
* Establishing billing budgets and alerts
* Setting up billing exports to estimate daily/monthly charges

### 1.3 Command line interface

* Cloud Shell on GCP Console
* Google Cloud SDK

### Lab: Tour Qwiklabs and the Google Cloud Platform [ACE]

* Learn about the Qwiklabs platform and identify key features of a lab environment.
* Learn about (and possibly purchase) Qwiklabs credits and launch an instance of a lab.
* Learn how to access the GCP console with specific credentials.
* Learn about GCP projects and identify common misconceptions that surround them.
* Learn how to use the GCP navigation menu to identify types of GCP services.
* Learn about primitive roles and use the Cloud IAM service to inspect actions available to specific users.
* Learn about Cloud Shell and run commands that use the gcloud toolkit.
* Learn about the API library and examine its chief features.
* Use tools that are pre-installed in Cloud Shell and run commands like touch, nano, and cat to create, edit, and output the content of files.

This is an introductory level lab and the first lab you should take if you're unfamiliar with GCP or Qwiklabs. If you are already experienced with Qwiklabs and the Google Cloud Platform Console, check out the following labs:

* Getting Started with Cloud Shell and gcloud
* Creating a Virtual Machine

## 2 Planning and configuring a cloud solution

### 2.1 Budgeting and planning with the Pricing Calculator

#### GCP Pricing calculator

### 2.2 Planning and configuring Compute resources

#### Compute engine vs. App Engine vs. Kubernetes Engine

| Option | Use when you need | Typical use cases
| --- |  --- |  --- |
App Engine, Flexible, zero-ops platform for building apps | focus on writing code, develop velocity, minimize operational overhead | websites, apps, gaming backends, IoT apps
Compute Engine, VMs running in data centers | control, OS level changes, lift-and-shift, Custom VM images | workload requiring specific OS or config, oon-prem apps -> cloud
Kubernetes Engine, container orchestration system | no dependency on specific OS, manage containers in Prod | containers, cloud-native, hybrid

### 2.3 Planning and configuring data storage

#### Comparing data storage and database options

| | Cloud SQL | Cloud Spanner | Cloud Datastore | Cloud Bigtable | Cloud Storage | BigQuery |
| --- |  --- |  --- |--- |  --- |  --- |--- |--- |
| | Relational | Relational | Non-relational | Non-relational | Object | Warehouse |
| Good for: | Web frameworks |  RDBMS + scale, HA, HTAP | Hierarchical, mobile, web | Heavy read + write | events | Binary or object data | Enterprise data warehouse |
| Such as: | CMS, eCommerce | User metadata, Ad/Fin/MarTech | User profiles, game state | AdTech, financial, IoT | Images, Media serving, backups | Analytics, dashboards |

Relational = store information in strictured columns and rows and data is retrieved using SQL.

Non-relational = store data in much less structured format, which is sometimes referred to as a documents.
Because it is less rigid structuring of a data, the format of incoming data can be changed over time, perhaps by adding additional information, occasionally to newer records, without affecting the integrity of any older data, still using previous formats.

Object = images, etc binary media
Warehouse scale database, SQL + workloads

#### Data storage use cases

| Option | Use when you need | typical use cases |
|--- | --- | --- |
| Cloud SQL | Fully managed MySQL and Postgre SQL database service |  web frameworks, structured data, OLTP workloads |
| CLoud spanner | Mission critical, relational database service with transactional consistency, global scale and high availability | Adtech, financial services, global supply chain, retail |
| Cloud Bigtable | A Scalable fully managed noSQ: wide-column database that is suitable for both low latency single point lookups and precalculated analytics | IoT, finance, adtech, monitoring, geospatial datasets, graphs |
|  BigQuery | A SCalable fully manages enterprise data warehouse (EDW) with SQL and fast ad hoc queries | OLAP workloads up to petabyte scale, big data exploration and processing |

### 2.4 Planning and configuring network resources

* Differentiating load balancing options - Load Balancing is when you have two ore more identical servers or server clusters, that have been cerated so that if the load becomes too great or if one ore more servers should fail, the remainder can assist with or take over handling the load. This is one way to create applications and services that are highly available. Load balancing allows multiple servers or clusters of servers to function as a single computing resource. Load balancers can also be configured to add or remove these servers or server clusters from the system to better meet demand. This is known as autoscaling.
* Identifying resource locations in a network for availability
* Configuring Cloud DNS

#### Load Balancing overview

INSERT PICTURE!!!

#### Google VPC offers a suite of load balancing options

| Global HTTP(S) | Global SSL Proxy | Global TCP Proxy | Regional Network TCP/UPD | Regional Internal TCP/UDP |
| --- | --- | --- | --- | --- |
| Layer 7 load balancing based on load | Layer 4 load balancing of non-HTTPS SSL traffic based on load | Layer 4 load balancing of non-SSL TCP traffic | Load balancing of any traffic (TCP, UDP) | Load Balancing of traffic inside a VPC |
| Can route diff. URLs to diff. backends | Supported on specific port numbers | Supported on specific port numbers | Supported on any port number | Use for internal tiers of multi-tier apps |

#### Global vs. Regional Load Balancing

* Global (for globally distributed users and instances)
  * Global HTTP(S)
  * Global SSL Proxy
  * Global TCP Proxy
* REgional (for regionally concentrated users and instances)
  * Regional Network TCP/UPD
  * Regional Internal TCP/UDP

#### External vs. internal

* External (traffic coming in from GCP network):
  * Global HTTP(S)
  * Global SSL Proxy
  * Global TCP Proxy
  * Regional Network TCP/UPD
* Internal (traffic within GCP network)
  * Regional Internal TCP/UDP

### Lab: Set Up Network and HTTP Load Balancers

In this hands-on lab, you'll learn the differences between a network load balancer and a HTTP load balancer. This lab takes you through the setup of the following load balancers:

* L3 Network Load Balancer
* L7 HTTP(s) Load Balancer

#### What you'll do

* Setup a network load balancer.
* Setup a HTTP(s) load balancer.
* Get hands-on experience learning the differences between network load balancers and HTTP load balancers.

#### 0 setup

<https://cloud.google.com/sdk/gcloud>

$ gcloud auth list
$ gcloud config list project
$ gcloud config set compute/zone us-central1-a
$ gcloud config set compute/region us-central1

#### 1 Create multiple web server instances

To simulate serving from a cluster of machines, create a simple cluster of Nginx web servers to serve static content using Instance Templates and Managed Instance Groups:

* Instance Templates define the look of every virtual machine in the cluster (disk, CPUs, memory, etc)
* Managed Instance Groups instantiate a number of virtual machine instances using the Instance Template

To create the Nginx web server clusters, create the following:

* A startup script to be used by every virtual machine instance to setup Nginx server upon startup
* An instance template to use the startup script
* A target pool
* A managed instance group using the instance template

##### 1.1 create a startup script

```Shell
cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF
```

##### 1.2 Create an instance template

```Shell
gcloud compute instance-templates create nginx-template \
         --metadata-from-file startup-script=startup.sh
```

##### 1.3 Create a target pool

A target pool allows a single access point to all the instances in a group and is necessary for load balancing in the future steps

```Shell
gcloud compute target-pools create nginx-pool
```

##### 1.4 Create a managed instance group using the instance template

```Shell
gcloud compute instance-groups managed create nginx-group \
         --base-instance-name nginx \
         --size 2 \
         --template nginx-template \
         --target-pool nginx-pool
```

##### 1.5 List the compute engine instances

```Shell
gcloud compute instances list
```

##### 1.6 configure a firewall

```Shell
gcloud compute firewall-rules create www-firewall --allow tcp:80
```

#### 2 Create a Network Load Balancer

Network load balancing allows you to balance the load of your systems based on incoming IP protocol data, such as address, port, and protocol type.
You also get some options that are not available, with HTTP(S) load balancing.
For example, you can load balance additional TCP/UDP-based protocols such as SMTP traffic.

##### 2.1 Create an L3 network load balancer

```Shell
gcloud compute forwarding-rules create nginx-lb \
         --region us-central1 \
         --ports=80 \
         --target-pool nginx-pool
```

##### 2.2 List all Google Compute Engine forwarding rules

```Shell
gcloud compute forwarding-rules list
```

#### 3 Create a HTTP(s) Load Balancer

HTTP(S) load balancing provides global load balancing for HTTP(S) requests destined for your instances.
You can configure URL rules that route some URLs to one set of instances and route other URLs to other instances.
Requests are always routed to the instance group that is closest to the user, provided that group has enough capacity and is appropriate for the request.
 If the closest group does not have enough capacity, the request is sent to the closest group that does have capacity.

##### 3.1 create a health check

```Shell
gcloud compute http-health-checks create http-basic-check
```

##### 3.2  Define an HTTP service and map a port name to the relevant port for the instance group

```Shell
gcloud compute instance-groups managed \
       set-named-ports nginx-group \
       --named-ports http:80
```

##### 3.3 Create a backend service

```Shell
gcloud compute backend-services create nginx-backend \
      --protocol HTTP --http-health-checks http-basic-check --global
```

##### 3.4 Add the instance group into the backend service

```Shell
gcloud compute backend-services add-backend nginx-backend \
    --instance-group nginx-group \
    --instance-group-zone us-central1-a \
    --global
```

##### 3.5 Create a default URL map that directs all incoming requests to all your instances

```Shell
gcloud compute url-maps create web-map \
    --default-service nginx-backend
```

##### 3.6 Create a target HTTP proxy

```Shell
gcloud compute target-http-proxies create http-lb-proxy \
    --url-map web-map
```

##### 3.7 Create a global forwarding rule to handle and route incoming requests

```Shell
gcloud compute forwarding-rules create http-content-rule \
        --global \
        --target-http-proxy http-lb-proxy \
        --ports 80
```

##### 3.8 Verify global forwarding rule

```Shell
gcloud compute forwarding-rules list
```

## 3 Deploying and implementing a cloud solution

### 3.1 Deploying and implementing compute resources

* Launching a compute instance using GCP Console and Cloud SDK
* Creating an autoscaled managed instance group using an instance template
* Generating/uploading a custom SSH key for instances
* Configuring a VM for Stackdriver monitoring and logging
* Assessing compute quotas and requesting increases
* Installing the Stackdriver Agent for monitoring and logging

#### Managed instance groups

* autohealing
* regional (multi-zone)
* load-balancing
* Autoscaling
* Auto-updating
* stateless serving
* Batch workload

### 3.2 Deploying and implementing Kubernetes resources

* deploying a Kubernetes Engine cluster
* Deploying a container app to Kubernetes Engine using pods
* Configuring Kubernetes Engine application monitoring and logging

#### What are containers

Scales as PaaS, but gives you nearly the same flexibility as IaaS. With this abstraction, your code is ultra portable, and you can treat OS and hardware as a block-box.

#### Containers as a tool for microservices

If you want to scale, for example a webserver, you can do so in seconds and deploy dozens or hundreds of them, depending on the size of your workload, on a single host.
Now, that is a simple example of one containers running the whole application on a single host.
You more likely want to build your application using lots of containers, each performing its own function as a microservices.

#### Kubernetes manages your containers

If you build application this way, and connect them with network connections, you can make them modular, deploy easily and scale independently across a group of hosts.
The host could then scale up and down, and start and stop containers as demand for you app changes or as hosts fail. A tool that helps you do that well is Kubernetes. Kubernetes makes ir easy to orchestrate many containers on many hosts, scale them as microservices and deploy rollouts and rollbacks.

#### lets have a closer look at 1) Kubernetes Engine clusters and 2) why you might want to deploy containers using pods

```Shell
gcloud container clusters create k1
```

At its highest level, Kubernetes is a set of APIs that you can use to deploy containers on a set of nodes called the cluster.

The system is divided in to:

* a set of master components that run as a control plane
* and a set of nodes that run containers.

in Kubernetes, a node represents a computing instance, like a machine. In GCP nodes are VMs running in Compute Engine.

You can describe a set of apps and how they should interact with each other and Kubernetes figures out how to make that happen.

#### Kubernetes Pods

Then you deploy containers on nodes using a wrapper of one or more containers, called a pod.
A pod is the smallest unit in Kubernetes that you create or deploy.
A pod represents a running process under cluster, as either a component of your cluster or an entire app.
Generally you have one container per pod, but if you have multiple containers with hard dependency, you can package them into a single pod and share networking and storage.
The pod provides a unique network IP and a set of ports for your containers and options, that govern how containers should run.
Containers inside a pod can communicate with one another using localhost and ports that remain fixed as containers are started and stopped on different nodes.

#### Deployment

A Deployment represent a group of replicas of the same pod and keeps the pods running even when nodes they run on fail. It could represent a component of an application or an entire application.

To see the running pods, run the command:

```Shell
kubectl get pods
```

### 3.3 Deploying App Engine and Cloud Function resources

* Deploying an application to App Engine (scaling configuration, versions and traffic splitting)
* Deploying a Cloud function, that receives Google cloud events (Cloud Pub/Sub events, Cloud Storage object change notification events)

#### App Engine Standard

* Easily deploy
* Autoscale
* Free daily quota
* usage-based pricing
* SDK
* Requirements:
  * Specific versions of Java, Python, PHP, and Go
Constraints:
  *no writing to local files
  * all requests timeout after 60sec
  * limits on 3rd party software

#### Cloud Functions

* Lightweight, event based, async compute solution
* Create single-purpose functions that respond to events w/o a server or runtime
* Event examples:
  * new instance cerates
  * file added to Cloud Storage
* Written in Javascript, Python, Go

### 3.4 Deploying and implementing data solutions

* Initializing data systems with products (Cloud SQL, Cloud Spanner, Cloud Datastore, Cloud Bigtable, BigQuery, Cloud Pub/Sub, Cloud Dataproc, Cloud Storage)
* Loading data (Command line upload, API trf, Import/export, load data from Cloud Storage, streaming data to Cloud Pub/Sub)

#### Initializing data systems with products

| Cloud SQL | Cloud Spanner | Cloud Datastore | Cloud Bigtable | Cloud Storage | BigQuery |
| --- | --- | --- | --- | --- | --- |
| Relational |  Relational | Non-relational | Non-relational | Object | Warehouse |

### 3.5 Deploying and implementing networking resources

* Creating a VPC with subnets (automode vs. custom-mode VPC, Shared VPC)
* Launching a Compute engine instances with custom network configuration (internal only IP address, Google private access, Static external/private IP, network tags)
* Creating ingress and egress firewall rules for a VPC (IP subnets, Tags, Service accounts)
* Creating a VPN b/w a Google VPC and an external network using Cloud VPN
* Creating a load balancer to distribute application network traffic to an application (Global HTTP, Global SSL, Regional Internal)

#### Creating a VPC with subnets

* VPC Networks are global, subnets are regional
* Each VPC network is contained in a GCP project
* automode (automatic subnets) vs. custom-mode VPC (manual subnets)

### DEMO: Creating an automode VPC network with a subnet demo

Navigation Menu -> Networking -> VPC Network -> VPC Networks
Create VPC network
Name: nynetwork
Subnets: automatic
Select firewall rule
Dynamic routing mode: regional vs global
Create

### DEMO: Creating a custom VPC network with a subnet demo

Navigation Menu -> Networking -> VPC Network -> VPC Networks
Create VPC network
Name: nynetwork-custom
Subnets: Custom
New subnet: specify name, region, IP range, Done
Select firewall rule
Dynamic routing mode: regional vs global
Create

### 3.6 Deploying a solution with Cloud Launcher (Marketplace!)

Marketplace -> Deployment Manager -> Compute Engine

### 3.7 Deploying an application using Deployment Manager

* Developing Deployment Manager templates to automate deployment of an application
* Launching a deployment Manager template to provision GCP resources and configure an application automatically

### LAB: Deployment Manager - Full Production

1. Install and configure an advanced deployment using Deployment Manager sample templates.
2. Enable Cloud monitoring.
3. Configure Cloud Monitoring Uptime Checks and notifications.
4. Configure a Cloud Monitoring dashboard with two charts, one showing CPU usage and the other ingress traffic
5. Perform a load test and simulate a service outage.

#### 1 Setup

$ gcloud auth list
$ gcloud config list project

##### Create a virtual environment

$ sudo apt-get update
$ sudo apt-get install virtualenv
$ virtualenv -p python3 venv
$ source venv/bin/activate

#### 2 Clone the Deployment Manager Sample Templates

$ mkdir ~/dmsamples
$ cd ~/dmsamples
$ git clone <https://github.com/GoogleCloudPlatform/deploymentmanager-samples.git>

#### 3 Explore the Sample Files

$ cd ~/dmsamples/deploymentmanager-samples/examples/v2
$ cd nodejs/python

<https://cdn.qwiklabs.com/40UqXM4XpfPlZ%2BOQWaVadL5q1cNctNoC2OHvAS3pxsw%3D>

frontend.py includes frontend.py.schema, which creates an instance template based on container_instance_template.py.

* This template is used to create a managed instance group and an autoscaler. The template also creates:
* a network load balancer that has a forwarding rule with a single public IP address.
* A target pool that refers to the managed instance group.
* A health check attached to the target pool.

nodejs.py includes nodejs.py.schema, which brings the frontend and backend templates together.

* Note that the frontend is frontend.py.
* The backend is /common/python/container_vm.py.
* This is a VM running a Docker container with MySQL, so it doesn't require a custom template.

#### 4 Customize the Deployment

##### 4.1 Specify the zone

$ gcloud compute zones list
$ nano nodejs.yaml
ZONE_TO_RUN -> us-east1-d

##### 4.2 Modify the maximum number of instances

nano nodejs.py
'maxSize': 20 -> 'maxSize': 4

#### 5 Run the Application

##### 5.1 Deploy the application

$ gcloud deployment-manager deployments create advanced-configuration --config nodejs.yaml

#### 6 Verify that the application is operational

You can see the instances in the Compute Engine part of Console (Navigation menu > Compute Engine > VM).

##### 6.1 Find the global load balancer forwarding rule IP address

$ gcloud compute forwarding-rules list

http://<your IP address>:8080
http://<your forwarding IP address>:8080/?msg=<enter_a_message>
http://<your IP address>:8080

#### 6.2 Create a Monitoring workspace

Navigation menu > Monitoring

#### 7 Configure an uptime check and alert policy in Cloud Monitoring

##### 7.1 Configure an uptime check

Cloud Monitoring > Uptime Checks > Create Uptime Check

* Title: Check1
* Check Type: TCP
* Resource Type: URL
* Hostname: <your forwarding address>
* Port: 8080
* Response content contains the text: <leave blank>
* Check every: 1 minute
Test -> Save

#### 8 Configure an alerting policy and notification

Alerting > Create Policy
Name Test
Add Condition
Find resource type and metric: GCE VM Instance
Metrics:  CPU usage & CPU utilization
Condition:  is above

## 4 Ensuring successful operation of a cloud solution

## 5 Configuring access and security
