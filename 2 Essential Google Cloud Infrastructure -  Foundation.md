# 2 Essential Google Cloud Infrastructure -  Foundation

## Table of contents

1. Introduction
2. Introduction to GCP
3. Virtual Networks
4. Virtual Machines
5. Course Resources

## 2. Introduction to GCP

### 2.1. Overview

### 2.2. Using GCP

Four ways to interact with GCP:

- GCP Console - Web user interface - console.cloud.google.com
- SDK - Command line interface - gcloud compute instances list
  - Cloud Shell - temporary VM with 5 GB persistent disk with Cloud SDK preinstalled
  - gcloud - for working with Compute Engine and many Google Cloud services
  - gsutil - for working with Cloud Storage
  - kubectl - for working with Google Kubernetes Engine and Kubernetes
  - bq - for working with BigQuery
  - cloudshell - to work with Cloud Shell utility
- REST-based API - For custom applications (in: get, post, put, delete, out: JSON) -  OAuth 2
- Mobile App - iOS & android- manage VMs, manage App Engine, manage billing

### 2.3. Lab Intro: Console and Cloud Shell

### 2.4. Getting Started With GCP And Qwiklabs

### 2.5. Lab: Working with GCP Cloud Console and Cloud Shell

```sh
gsutil mb gs://<BUCKET_NAME>
gsutil cp ‘my file.txt' gs://[BUCKET_NAME]
```

```sh
# Create a persistent state in Cloud shell
# Every time you close Cloud Shell and reopen it, a new VM is allocated, and
# the environment variable you just set disappears.

INFRACLASS_REGION=europe-west1
echo $INFRACLASS_REGION

mkdir infraclass
touch infraclass/config
echo INFRACLASS_REGION=$INFRACLASS_REGION >> ~/infraclass/config
INFRACLASS_PROJECT_ID=qwiklabs-gcp-01-6b3d5788ca14
echo INFRACLASS_PROJECT_ID=$INFRACLASS_PROJECT_ID >> ~/infraclass/config

echo "source infraclass/config" >> .profile
```

### 2.6. Lab Review: Console and Cloud Shell

### 2.7. Lab Intro: Infrastructure Preview

### 2.8. Lab: Infrastructure Preview

Open Jenkins in the marketplace:
Marketplace -> Jenkins Certified by Bitnami -> Launch -> Deploy
Deployment Manager -> jenkins-1 is being deployed

Visit the site
ssh into service

```sh
sudo /opt/bitnami/ctlscript.sh restart
```

### 2.9. Lab Review: Infrastructure Preview

### 2.10. Demo: Projects

Projects isolate related resources from one another

1. Create a project in the Cloud Console
IAM & Admin -> Manage Resources -> New Project
Enter a project name -> Create

2. Shut down a project in the Cloud Console
IAM & Admin -> Manage Resources -> Delete
Project is deleted 30 days after shut down

3. List active project ID in Cloud Shell
`gcloud config list | grep project`

4. Change project_id in Cloud Shell
`gcloud config set project $PROJECT1_ID`

5. delete project in Cloud Shell
`gcloud project delete $PROJECT1_ID`

### 2.11. Review

## 3. Virtual Networks



Virtual Network = software-defined network in GCP. Thinking about resources as services, instead of hardware, will help you understand the available options.

GCP consists of:
* Regions (specific geographic location, where you can run your resources), divided smaller into zones.
* Points-of-Presence (PoPs)
* Global private network
* Services
Virtual Private Cloud (VPC)
Managed networking functionality for your cloud platform resources.

With GCP you can:
*  provision your resources, 
* connect them, 
* and isolate them from each other 
in a virtual private cloud.

VPC is a comprehensive set of Google-managed networking objects:
* Projects - encompass every single service including networks
* Networks - default, auto or  custom mode
* Subnetworks - allow you to divide or segregate your network
* Regions - represents Google’s data centers
* Zones - represents Google’s data centers
* IP addresses - internal, external, range
* VMs - configuring VM instances from a networking perspective
* Routes
* Firewall rules
Projects, networks, and subnetworks
Project
* Associates objects and services with billing
* Contains networks, that can be shared/peered
Network
* Has no IP address range, but instead are simply a construct of all individual IP addresses and services within the network
* Is global and spans all available regions
* Contains regional subnetworks
* Is available as the default, auto, or custom mode.
Default mode
	Auto mode
	Custom mode
	Every project
One Subnet per region
Default firewall rules
	Default network
One subnet per region
Regional IP allocation
Fixed /20 subnetwork per region
Expandible up to /16
	No default subnets created
Full control of IP ranges
REgional IP allocation
Expandible to any RFC 1918 size

Subnetwork
VMs can be on the same subnet but in different zones.

Four reserver IP addresses in the subnet 
* .0 - reserved for network
* .1 - reserved for subnet’s gateway
* Second to last address - broadcast address (?)
* Last address - broadcast address
  
Demo: expand a subnet
Custom subnet with /29 mask, which provides you with 8 addresses, but from those 4 are reserved by GCP. When you create 5th VM, you will get an error msg - “IP Space is exhausted”, so you need to increase subnet capacity. To do that:
Open either VPC Network in Navigation Menu or click on nic0 -> subnet -> edit /23 save.


IP addresses
Internal IP:
* Allocated from subnet range to VMs by DHCP
* DHCP leave is renewed every 24 hours
* VM name + IP is registered with network-scope DNS


External IP:
* Assigned from the pool (ephemeral)
* Reserved (static)
* VM does not know external IP, it is mapped to the internal IP








Demo: internal and external IP
Compute Engine -> Create -> management, security, disc, networking (...) -> Networking:
* Tags
* Hostname
* Network Interfaces:
   * Network
   * Subnet 
      * /20 = over 4 000 IP addresses
      * /23 = 500 (?) IP addresses
      * /29 = 8 IP addresses
   * Internal IP
      * Ephemeral auto
      * Ephemeral custom
      * Reserve static internal IP
   * External IP
      * None - if instances do not need to have an IP address
      * Ephemeral
      * Create an IP address
   * IP Forwarding
Mapping IP address
External IPs are mapped to internal IPs. The external IP is mapped to VMs internal IP by VPC.


$ ifconfig only shows the internal IP address


DNS resolution for internal addresses


Each instance has a hostname that can be resolved to an internal IP address:
* The hostname is the same as instance name
* FQDN is [hostname].[zone].c.[project-id].internal
   * Example: my-server.us.cental1-a.c.guestbook-151617.internal


Name resolution is handled by internal DNS resolver:
* Provided as part of Compute Engine (169.254.169.254)
* Configured for use on instance via DHCP
* Provided answer for internal/external addresses


DNS resolution for external addresses
* Instances with external IP addresses can allow connections from hosts outside the project
   * Users connect directly using external IP address
* DNS records for external address ca be published using existing DNS servers
* DNS zones can be hosted using Cloud DNS


Cloud DNS
* Google’s DNS server
* Translate domain names into IP addresses
* Low latency
* High availability
* UI, command line, or API
Alias IP ranges
Let you assign a range of IP addresses as aliases to a VM’s network interface, using alias IP ranges.
Useful, if you have multiple services running on VM and you want to assign a different IP address to each service. You can configure multiple IP addresses, representing containers or applications hosted in a VM, without having to define a separate network interface.
You just draw the alias IP range from the local subnets primary or secondary CIDR ranges.
  

Routes and firewall rules
A route is a mapping of an IP range to a destination.


Every network has:
* Routes, that let instances in a network send traffic directly to each other, even across subnets
* A default route that directs packets to destinations that outside the network
Firewall rules must also allow the packet.


Routes map traffic to destination networks
* Apply to traffic egressing a VM
* Forward traffic to the most specific route
* Are created when subnets are created
* Enable VMs on the same network to communicate
* The destination is in CIDR notation
* Traffic is ONLY delivered if it also matches a firewall rule


.  
  



Firewall rules protect your VM instances from unapproved connections
* VPC network functions as a distributed firewall
* Firewall rules are applied to the network as a whole
* Connections are allowed or denied at the instance level
* Firewall rules are stateful
* Implied deny all ingress (inbound connections) and allow all egress (outbound connections)


  

  

  

Pricing
Response to ingress = Egress = charge
  

  

  

Lab: VPC Networking
In this lab, you create an auto-mode VPC network, with firewall rules and two VM instances, then you convert the VPC network from auto-mode to custom-mode network, and create other custom-mode networks. + explore connectivity across networks.
Explore the default network
Navigation Menu -> Networking -> VPC Network, Routes, Firewall rules
Every Project has a default network unless your Org. policy prevents it.
Delete existing Firewall rules, VPC Networks & Routes
Select all -> Delete
Without VPC network you will not be able to create any VM instance, containers or App Engine apps
No local network available
Create auto-mode network
Networking -> VPC Network -> Create
* Add VPC Network name: mynetwork
* Subnet creation mode: Automatic
* Firewall rules: select all to apply
Create
Create 2 VM instances in Console
One called mynet-us-vm in us-central1-c, other called mynet-eu-vm in europe-west1-c
Click on nic0, to see that instances are part of auto-network created earlier
Check connectivity b/w instances
Ssh in one of the machines and ping other machine’s IP address. 
Instances are in 2 separate regions but in the same VPC network/subnet, so should be able to ping these addresses 


$ ping -c 3 vm_ip_address
$ ping -c 3 mynet-eu-vm
$ ping -c 3 external_ip_address
Convert auto-mode to custom-mode
Networking -> VPC Network -> mynetwork -> Edit -> subnet creation mode: Custom
Create another VPC Network using Console
Networking -> VPC Network -> Create:
* Add name: managementnet
* Subnet creation mode: custom
* Add subnet name: managementsubnet-us
* Add region: us-cental1
* Add IP address range: 10.130.0.0/20
* Click on the equivalent command line, to see Cloud Shell commands
* Create
Create another VPC Networks using Cloud Shell
$ gcloud compute networks create privatenet --subnet-mode=custom
$ gcloud compute networks subnets create privatesubnet-us --network=privatenet --region=us-central1 --range=172.16.0.0/24
$ gcloud compute networks subnets create privatesubnet-eu --network=privatenet --region=europe-west1 --range=172.20.0.0/20
List networks/subnets in Cloud Shell
$ gcloud compute networks list
$ gcloud compute networks subnets list --sort-by=NETWORK
Create Firewall rule in Console
Networking -> VPC Network -> Firewall Rules -> Create firewall rule:
Add Name: managementnet-allows-icmp-ssh-rdp
Select network: namagementnet
Targets: all instances in the network
Source IP Range: 0.0.0.0/0 to select all addresses
Allow protocols and ports:
* tcp: 22, 3389 
* Other protocols: icmp
Create Firewall rule in Cloud Shell
$ gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-range=0.0.0.0/0
List firewall rules in Cloud shell
$ gcloud compute firewall-rules list --sort-by=NETWORK
Create some more VM instances, 1st in console, 2nd in Cloud Shell
vm1 name: nanagement-us-vm, network = managementnet


$ gcloud compute instances create privatenet-us-vm --zone=us-central1-c --machine-type=f1-micro --subnet=privatesubnet-us
Lis VM instances in Cloud Shell
$ gcloud compute instances list --sort-by=ZONEw
Check connectivity b/w machines
* external ping works
* internal ping does not work as machines are in different networks
Common network designs
Increases availability with multiple zones
For increased availability: two VMs are in different zones but in the same sub-network.
  

Globalization with multiple regions
Different VMs in different regions, combined not with sub-network, but with Global Load Balancer
  

Cloud NAT provides internet access to private instances
Internal IP addresses only, whenever possible.


Cloud NAT or managed  Network Address Translation service. Provision VM machines without public IP addresses, while allowing access to the internet in a controlled manner. Cloud NAT gateway implements outbound net, not inbound net.


  





Private Google access to Google API and Services
To allow VM instances that only have internal IP to access to reach external IP addresses of Google’s APIs and services. For example, you need to enable Private Google access to access Storage Bucket contents
Google Private Access has no effect on instances with external P addresses.
  



Lab: Implement Private Google Access and Cloud NAT
Created an instance with no external IP address
Accessed it using Cloud IAP
Enable Private Google Access
Configure NAT Gateway
Check that VM can access Google API and Services and other external addresses


VMs w/o external IP are isolated from the external network. Using cloud NAT these instances can access the internet.
Create a VPC network
Navigation menu -> networking -> VPC network -> VPC Networks -> Create VPC Network
* Name: privatenet
* Subnet creation mode: custom
* Subnet: privatenet-us
* Region: us-central1
* IP address range: 10.130.0.0/20
* Private Google Access: off
Done, Create
Create a Firewall rule
To allow ssh to the instance
Navigation menu -> networking -> VPC network -> Firewall rules -> Create Firewall rule
* Name: privatenet-allow-ssh
* Network: privatenet
* Targets: all instances in the network
* Source IP ranges: 25.235.240.0/20 # we are gonna give it a very specific range. And that is because we are using the Cloud IAP tunnel.  And because of that, we can limit the CIDR range.
* Specific  protocols and ports: tcp: 22
Create
Create VM instance
Navigation menu -> Compute -> Compute Engine -> VM instances -> Create
* Name: vm-internal
* Region: us-central1
* Zone: us-cetral1-c
* Network interfaces: privatenet
* External IP: None
Done, Create
Ssh into Instance, check connectivity to the internet
As simple ssh would not work for us, we gonna use Cloud Shell to IAP tunnel
Open Cloud Shell -> Continue
$ gcloud config set project [PROJECT_ID]
$ gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
$ ping -c 3 www.google.com 
No connection
$ sudo apt-get update
No connection
$ exit
Create Bucket in Cloud Storage, copy the file into it
Navigation Menu -> Storage -> Storage -> Browser -> Create Bucket
* Name: Project ID
Create


$ gsutil cp gs://cloud-training/gcpret/private/access.svg gs://Project ID
Check access Cloud Storage bucket from Cloud Shell
$ gsutil cp gs://Project ID/*.scg .
success
Check access Cloud Storage bucket from VM
$ gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
$ gsutil cp gs://Project ID/*.scg . 
Fail
Enable Private Google access for privatenet-us subnet
Navigation menu -> networking -> VPC network -> VPC Networks -> privatenet -> privatenet-us -> Edit
* Private Google access: On
Save
Check access Cloud Storage bucket from VM
$ gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
$ gsutil cp gs://Project ID/*.scg . 
Success
Configure Cloud Gateway to access the internet
Navigation menu -> Networking -> Network Services -> Cloud NAT -> Get Started
* Gateway Name: nat-config
* VPC Network: privatenet
* Region: us-central1
* Cloud Router: create a new router
   * Name: nat-router
   * Create
* NAT mapping: Allows you to choose subnets to map to NAT gateway
Create
Check VM access to the internet
$ sudo apt-get update
Success
Virtual Machines
Compute Engine
VMs are the most common infrastructure component and consist of: 
* vCPU,
* Memory
* Disk Storage
* IP address






	Compute Engine
	Kubernetes Engine
	App Engine Standard
	App Engine Flex
	Cloud Functions
	Language Support
	Any
	Any
	Python
Node.js
Go
Java
PHP
	Python
Node.js
Go
Java
PHP
Ruby
.NET
Custom Runtimes
	Python
Node.js
Go
	Usage Model
	IaaS
	IaaS, PaaS
	PaaS
	PaaS
	Microservices Architecture
	 Scaling
	Server Auto-Scaling
	Cluster
	

Autoscaling managed servers
	Serverless
	Primary use case
	General Workloads
	Container Workloads
	Scalable web applications
Mobile backend applications
	Lightweight Event Actions
	



Compute Engine is a GCP service to create VMs.


Predefined or custom machine types:
* vCPU (Cores) and Memory (RAM)
* Persistent disks: HDD, SSD, and local SSD
* Networking
* Linux/Windows


Compute Engine features:
* Availability policies
   * Live Migrate 
   * Auto Restart
* 2:
   * Per second billing
   * Sustained use discounts
   * Committed use discounts
* Preemptible
   * Up to 80% discounts
   * No SLA
* Global Load balancing:
   * Multiple regions for availability
* Machine rightsizing:
   * Recommendation engine for optimums machine size
   * Stsckdriver statistics
   * New recommendation 24 hrs after VM create or resize
* 6:
   * Instance metadata
   * Startup scripts


Several machine types:
* Network throughput scales 2 Gbps per vCPU
* Theoretical max of 32 Gbps with 16 vCPU or 100 Gbps for T4 or V100 GPUs
A vCPU is equal to 1 hardware hyper-thread


Networking:
Robust networking features:
* default/custom networks
* inbound/outbound firewall rules:
   * IP based
   * instance/group tags
* Regional HTTPs load-balancing
* Network-load balancing
   * Does not require pre-warming
* Global and multi-regional subnetworks
Demo: Create a VM
Navigation menu -> Compute -> Compute Engine -> VM instances -> Create
Command-line with parameters you selected
VM instances -> Columns, more columns available
VM access and lifecycle
The creator or VM instance has full root privileges for that instance.


Linux: SSH
* SSH = secure shell
* SSH from GCP Console or Cloud Shell via Cloud SDK
* SSH from a computer or third-party client and generate key-pair
* Requires firewall rule to allow tcp:22


Window: RDP
* RDP = Remote Desktop Protocol
* RDP clients
* Powershell terminal
* Requires setting the Windows password
* Requires firewall rule to allow tcp:3389






VM Lifecycle
* Provisioning:
   * when you define all the properties of the instance and click create, instance enters provisioning state, resources are being reserved, the instance is not running
   * vCPUs + Memory
   * Root disk / Persistent Disk
   * Additional Disks
* Staging:
   * Resources have been acquired, and the instance is prepared for launch
   * IP addresses Virtual Private Cloud
      * Internal
      * external
   * System Image Cloud Storage
   * Boot
* Running
   * After the instance starts running, 
   * Startup script
   * Access SSH/RDP
   * Modify/ Use 
      * Live Migrate your VM to another host, in the same zone
      * Move VM to another zone, take a Snapshop of the persistent disk, export the system image, reconfigure metadata
* Stopping:
   * To upgrade your machine by adding more CPU
   * Shutdown script
* Terminated:
   * Delete
   * Availability policy


  



Changing VMs state from running


	methods
	Shutdown script time
	state
	reset
	console, $ gcloud, API, OS
	no
	Remains running
	restart
	console, $ gcloud, API, OS
	no
	Terminated -> running
	reboot
	OS: sudo reboot
	~90 sec 
	running -> running
	stop
	console, $ gcloud, API
	~90 sec 
	running -> terminated
	shutdown
	OS: sudo shutdown
	~90 sec 
	running -> terminated
	delete
	console, $ gcloud, API
	~90 sec 
	running -> N/A
	preemption
	automatic
	~30 sec 
	N/A
	

Availability policy: automatic changes
Called scheduling options in SDK/API
   * Automatic Restart
      * Automatic VM restart due to crash or maintenance event 
* On host maintenance
   * Determines whether the host is love-migrated or terminated due to maintenance event
* Live Migration
   * During the maintenance event, VM is migrated to different hardware without interruption
   * Metadata indicates the occurrence of live migration
Stopped (terminated) VM
No charge for stopped VM
* Charges for attached disks an IPs


Actions:
* Change the machine type
* add/remove attached disks, change auto-delete settings
* Modify instance tags
* Modify custom CM or project-wide metadata
* remove/set new static IP
* Modify VMs availability policy
* Cannot change the image of a stopped VM
Lab: creating VMs
Navigation menu -> Compute -> Compute Engine -> VM instances -> Create -> Create VM
VM instance details
* CPU platform
* Firewall rules
* Network Tags
* Availability policies
* Logs -> Stackdriver logging
* etc.
Monitoring
To see Linux machine parameters
$ free -h # for available Memory
$ sudo dmidecode -t 17 # additional information about memory
$ nproc # number of processors
$ lscpu # additional information about CPUs
Compute Options
3 ways how to create a VM:
* Console
* $ gcloud
* API 
Machine Types


Predefined machine type: Ratio of GB of Memory per vCPU
* Standard: 3.75 GB per vCPU; 
* High-memory: 6.5 GB per vCPU; 
* High-CPU: 0.9 GB per vCPU;
* Memory-optimized: > 14 GB per vCPU
* Compute-optimized: > highest performance per vCPU (3.8 Ghz)
* Shared-Core: < 1 vCPU


Custom machine type: (mode expensive)
* you specify Memory & vCPUs
* Nr. of vCPUs = 1 or even
* Default Memory 0.9 GB - 6.5 GB pe vCPU, use Extend Memory to change that
* Total MEmory must be a multiple of 256 MB


When to select Custom:
* requirements s fit b/w the predefined types
* Need more memory or more vCPUs
Compute Pricing
Per-second billing, minimum of 1 minute
* vCPUs, GPUs and GB of Memory


Resource-based pricing
* Each vCPU and each GB on Mem is billed separately


Discounts (types cannot be combined):
* Sustained use (for running VMs for a specific length of the month, up to 30% off, if the instance is run for the whole month)
* Committed use 
* Preemptible VM instances 
   * up to 80% off
   * VM might be terminated at any time, 30 sec. terminate warning for shutdown script
   * No live-migrate, no auto restart
   * Monitoring + load balances
   * Recommendation Engine
* Notifies for underutilized instances


Free usage limits
Special Compute Config
Sole-tenant nodes physically isolate workloads
  

Shielded VMs offer verifiable integrity
* Secure Boot
* Virtual trusted platform module (vTPM)
* Integrity monitoring
* Requires a shielded image!
Images
What is an image
* Boot loader
* Operating system
* File system structure
* Software
* Customization


Images
* Public base images
   * Google, third-party vendors, community, Premium images (p)
   * Linux (Ubuntu, Debian, RHEL(p), etc)
   * Windows (Windows Server 2019 (p), 2016 (p), 2012 (p), SQL Server(p))
* Custom images
   * Create new images from VM, pre-configured and software
   * Import from an on-prem, workstation or another cloud service
   * Management features: image sharing, image family, depreciation
Disk Options
Boot disk
* VM comes with a single root persistent disk
* Image is loaded onto root disk during first boot
   * Bootable: you can attach to a VM and oot from it
   * Durable: can survive VM terminate
* Some OS images are customized for Compute Engine
* Can survive VM deletion if “Delete boot disk when an instance is deleted” is disabled
Persistent disk
Networking storage appearing as a block device
* Attached to a VM through the network interface
* Durable: storage, can survive VM terminate
* Bootable: you can attach to a VM and boot from it
* Snapshots: incremental backups
* Performance: Scales with size


Features:
* HDD or SSD
* Disk-resizing: even running and attached
* Can be attached in read-only mode to multiple VMs
* Encryption keys
Local SSDs are physically attached to a VM
* More IOPS, lower latency, higher throughput
* 375 GB disk up to 8 per VM, total 3 TB
* Data survives at reset, but not at VM stop or terminate
* VM-specific, cannot be reattached to a different VM
RAM disk
* tmpfs
* Faster than local disk, slower than memory
   * Use when your application expects a file system structure and cannot directly store its data in memory
   * Fast scratch disk, or fast cache
* Very volatile, erase on stop or restart
* May need a larger machine type if RAM was sized for the application
* Use persistent disks to cack up RAM disk data
Summary of disk options


	PD HDD
	PD SSD
	Local SSD
	RAM
	Data redundancy
	Y
	Y
	N
	N
	Encryption at rest
	Y
	Y
	Y
	N/A
	Snapshotting
	Y
	Y
	N
	N
	Bootable
	Y
	Y
	N
	N
	USe Case
	General, bulk file storage
	Very random IOPS
	High IOPS, low latency
	Low latency and risk of data loss
	

Max Nr. of persistent disks
Machine Type
	Disk Nr. limit
	Shared-core
	16
	Standard
	128
	High-Memory
	High-CPU
	Memory-Optimized
	Compute-Optimized
	

Persistent disk management differences
* A Single file system is best
* Resize (grow) disks
* Resize file system
* Built-in snapshot service
* Automatic encryption
Common Compute Engine Actions
Metadata and scripts
Every VM instance stores its metadata in a metadata server, which is useful with startup and shutdown scripts.
 
startup-script-url=URL
shutdown-script-url=URL
Move an instance to a new zone, automatically (within a region)
* $ gcloud compute instance move
* Update references to VM, not automatic
Move an instance to a new zone, manually (b/w regions)
1. Snapshot all PD on the source VM
2. Create new PD in the destination zone, restored from snapshots
3. Create a new VM in destination zone and attach new persistent disks
4. Assign static IP to new VM
5. Update references to VM
6. Delete snapshot, original PD, and original VM
Snapshot, back up critical data
  

Snapshot, migrate data b/w zones
  



Snapshot: transfer to SSD to improve performance
  

Persistent disk snapshots 
* Snapshot is not available for local SSD
* Creates an incremental backup to Cloud Storage
   * Not visible in your buckets, managed by the snapshot service
   * Consider cron jobs for periodic incremental backup
* Snapshots can be restored to new persistent disks
   * A new disk can be in another region or zone in the same project
   * Basis of VM migration: “moving” a VM to a new zone
      * Snapshot does not back up VM metadata, tags, etc


Snapshots are different from custom images, which are used to create instances or configure instance templates. Spanshorts are useful for periodic backup of the data on your persistent disks. Snapshots are incremental and automatically compressed, so you can create regular snapshots on a PD faster and at a much lower cost than if you regularly created a full image of a disk.


Resize persistent disk
  



Lab: working with VMs
Create a VM instance
Navigation menu -> Compute -> Compute Engine -> VM instances -> Create:
* Name: mc-server
* Region/Zone: us-central1/us-central1-a
* Access scopes: set access for each API
   * Storage: Read Write #to allow VM instance to write to a Cloud Storage bucket
* Disks -> Additional Disks -> Add new disk:
   * Name: minecraft-disk
   * Type: SSD persistent disk
   * Size (GB): 50
   * Done
* Networking-> Network tags: minecraft-server # allow us to create specific firewall rules
* Networking -> Network interface -> External IP -> Create IP ddress -> Name:  mc-server-ip
* Create
Ssh and prepare data disk (create dir, format and mount disk)
Ssh


$ sudo mkdir -p /home/minecraft


$ sudo mkfs.ext4 -F -E lazy_itable_init=0,\
lazy_journal_init=0,discard \
/dev/disk/by-id/google-minecraft-disk


$ sudo mount -o discard,defaults /dev/disk/by-id/google-minecraft-disk /home/minecraft
Install and run the application
$ sudo apt-get update
$ sudo apt-get install -y default-jre-headless
$ cd /home/minecraft
$ sudo wget https://launcher.mojang.com/v1/…./server.jar
$ nano EULA.txt
  eula=true
$ sudo java -Xmx1024M -Xms1024M -jar server.jar nogui
Add Minecraft server port to firewall rules
Navigation Menu -> Networking -> VPC Network -> Firewall Rules -> Create Firewall rule
* Name: minecraft-rule
* Target tags: minecraft-server
* IP ranges: 0.0.0.0/0
* Specified protocols and ports: tcp: 25565
* Create
Create backup script
SSH
Create a Cloud Storage bucket
$ export YOUR_BUCKET_NAME=Project-ID
$ gsutil mb gs://$YOUR_BUCKET_NAME-minecraft-backup
$ cd /home/minecraft
$ nano backup.sh
   script code
$ sudo chmod 755 backup.sh
$ ./backup.sh
Shcedule backup script
$ sudo crontab -e
0 */4 * * * /home/minecraft.bacup.sh # to run every 4 hours
Perform maintenance
Stop VM instance -> Click on instance -> Custom metadata
* startup-script-url: https://storage.googleapis.com/cloud-training/archinfra/mcserver/startup.sh
* shutdown-script-url: https://storage.googleapis.com/cloud-training/archinfra/mcserver/shutdow.sh
* save


$ cat startup.sh
#!/bin/bash
mount /dev/disk/by-id/google-minecraft-disk /home/minecraft
cd /home/minecraft
sudo screen -S mcs java -Xmx1024M -Xms1024M -jar server.jar nogui


$ cat shutdown.sh
#!/bin/bash
sudo screen -r -X stuff '/stop\n'