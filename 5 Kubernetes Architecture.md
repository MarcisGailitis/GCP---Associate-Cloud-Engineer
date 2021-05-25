# Architecting with Google Kubernetes Engine - Foundations

## Kubernetes Architecture

### Introduction

How does Kubernetes expect you to tell what to do?
What choices do you have for describing your workloads?

#### Lean how to

* Conceptualize the Kubernetes Architecture
* Deploy a Kubernetes cluster using Google Kubernetes Engine (GKE)
* Deploy Pods to a GKE cluster
* View and manage Kubernetes objects

### Kubernetes Concepts

We will lay-out fundamental components of the Kubernetes operating philosophy. To understand how Kubernetes work, there are two related concepts, that you have to understand:

1 Kubernetes Object model - each thing Kubernetes manages is represented by an object, and you can view and change these object's attributes and state.
2 Principle of declarative management - Kubernetes expects you to tell what the expected state of objects under it's management should be. It will work to bring that state into being and keep it there.

Kubernetes Object - Persistent entity representing the state of something running in the cluster, it's desired state and current state.
Various kinds of objects represent the containerized applications, the resources that are available to them and the policies that affect their behavior.

Kubernetes objects have two important elements:

* Object spec desired state described by us
* Object status - current state described by Kubernetes

#### Kubernetes Control Plane

Kubernetes Control Plane - to refer to the various system processes, that collaborate to make a Kubernetes cluster work.  More later in the module.
Each object is of the certain type or kind, as Kubernetes calls them.

#### Pod

Pod are tee basic building block of the standard Kubernetes model and are the smallest deployable Kubernetes object. Why not the container?
Every running container in a Kubernetes system is in a Pod. A Pod embodies the environment, where the containers live and that environment can accommodate one or more containers. If there is more than one container in a pod, they are tightly coupled and share resources, including networking and storage.

|POD|
| :---: |
|Shared Networking|
|Container1 | Container2 | Container3|
|Shared Storage|

Kubernetes assigns each Pod an unique IP address. Every container within a Pod shares the network name space, including IP address and network ports.
Containers within the same pod can communicate through localhost - 127.0.0.1

A Pod can also specify a set of storage volumes to be shared among its containers.

#### Example: three nginx containers

Declare objects that represent those containers, most probably pods
Kubernetes launches those objects and maintains them
BUT Pods are not self healing, so we might use  more sophisticated kind of object (pod 2.0)?

Lets suppose we have given a Kubernetes a desired state (thee nginx pods, always kept running).
We did this by telling Kubernetes to create and maintain one or more objects, that represent them.
Now Kubernetes compares the desired state from the current state.
Lets imagine that declaration of 3 nginx containers is completely new.
The current state does not match the desired state.
So Kubernetes, specifically its Control Plane wit remedy the situation, so three nginx pods will be launched. And Kubernetes Control Plane will continuously monitor the state of the cluster.

### The Kubernetes Control Plane

Kubernetes Control Plane is a fleet of cooperating processes, that make a Kubernetes cluster work. Even though you will fork directly with only few of these components, it helps to know about them and role, each plays. We will build Kubernetes Cluster part by part, explaining each peace.
After that we will look how a Kubernetes cluster running in GKE is a lot less work to manage than the one you provision yourself.

First and fore most, your cluster needs a computer. One is called master and others are called nodes:

* The job of the nodes is to run pods.
* The job of master is to coordinate the entire cluster.

#### master

We will have  a look at its control components first. Several critical Kubernetes components run on the master.

#### kubeAPIserver

The single component that you interact directly is the kubeAPIserver, this components job is to accept commands that view of change the state of the cluster, including launching pods.
In this specialization you will use kubectl command frequently. This commands job is to connect to kubeAPIserver and communicate with it using Kubernetes API. KubeAPIserver also authenticates incoming requests, determines whether they are authorized, invalid and manages admission control. But it is not just kubectl that talks with kubeAPIserver. Any query or change to the cluster's state must must be addressed to the kubeAPIserver.

#### etcd

etcd is the cluster's database. It's job is to reliably store the state of the cluster. It includes all the cluster's configuration data and more dynamic information, such as what nodes are part of the cluster, what pods should be running and where they should be running. You never interact directly with etcd, instead kubeAPIserver interacts with the database on behalf of the rest of the system.

#### kube-schedule

kube-scheduler is responsible for scheduling pods onto the nodes. To do that it evaluates the requirements of each individual pod and selects which node is the most suitable. But, it does not do the work of actually launching the pods onto the nodes.
Instead, whenever it discovers a pod object that does not yet have an assignment to a node, it chooses a node and writes the name of the node into the pod object. Another componentsof the system is then responsible for then launching the pods.

How does a kube-scheduler decides where to run a pod?
It knows the state of all the nodes and it will also obey the constraints that you define, on where a pod may run, based on hardware, software and policies.
For example: you might specify that a certain pod is only allowed to run on a nodes with certain amount of memory.
For example: you can also define affinity specifications, which cause groups of pods to prefer running on the same node OR anti-affinity specifications, which ensure that pods no not run on the same node. More on this later.

#### kube-controller-manager

kube-controller-manager has a broader job. It continuously monitors the state of the cluster through kubeAPIserver. Whenever the current state of the cluster does not match the desired state, kube-controller-manager will attempt to make changes to achieve the desired state. It is called the controller manager, because many Kubernetes objects are maintained by loops of code, called controllers.These loops of code handle the process of remediation. Controllers will be very useful to you - you will use certain kinds of Kubernetes controllers to manage workloads.

For example: to keep tree nginx pods always running? We can gather them together into a controller object called deployment, that not only keeps them running, but also lets us scale them and bring them together underneath a frontend. More on deployments later in this module.

#### other controllers

Other kinds of controllers have system level responsibilities. For example:

* node-controllers job is to monitor and respond when a node is offline.
* kube-cloud-manager manages controllers that interact with underlying cloud providers. For example: if you manually launched a Kubernetes cluster on GCP, kube-cloud-manager would be responsible for bringing in GCP features, like load-balances and storage volumes, when you needed them.

#### node

Each node runs a small family of control-plane components too.

#### Kubelet

For example: each node runs a Kubelet - a Kubernetes agent on each node. When a kubeAPIserver wants to start a pod on a node, it connects to that nodes Kubelet. Kubelet uses the container runtime to start the pod and monitors its lifecycle, including readiness and liveness probes and reports back to kubeAPIserver.

#### Container runtime

Container runtime - software that knows how to launch a containers from a container image. Kubernetes offer several choices of container runtimes, but the Linux distribution that GKE uses for its nodes, launches containers using Container D, the runtime component of Docker.

#### Kube-proxy

Kube-proxy job is to maintain network connectivity among the pods in the cluster. In open-source Kubernetes it does so using firewall and capabilities of IP tables, which are built-in Linux kernel. Later we will learn how GKE handles pod networking.

### Google Kubernetes Engine Concepts

Setting up a Kubernetes cluster by hand is tons of work. Fortunately, there is an open-source command called kubeamd, that can automate initial setup of a cluster. BUT if a node fails or needs maintenance, human admin had to respond manually.

How does this picture we saw earlier differs for GKE?

#### Master in GKE

GKE manages all of the control-plane components for us. It still exposes an IP address, to which we send all of our Kubernetes API requests. BUT GKE takes responsibility for provisioning ang managing ALL of the master infrastructure behind it.
It also abstracts away having a separate master.The responsibilities of Master are observed by GCP and you are not separately billed for your master.

#### nodes in GKE

In any Kubernetes environment, Kubernetes does not create nodes. Nodes are created externally by cluster admins, who  add them to Kubernetes.

GKE automates this for you by deploying and registering Compute Engine instances as nodes.
You can manage node setting directly from the GCP Console.
You pay per hour of life of your nodes, not counting the master.

Because nodes run on Compute engine, you choose your node machine type, when you create your cluster, default n1-standard-1. You can choose a baseline minimum CPU platform for the nodes or node pool.

#### node pools

You can also select multiple node machine types, by creating multiple node pools. A node pool is a subset of nodes within a cluster that share a configuration, such as their amount of memory or their CPU generation. Node pools also provide an easy way to enure that workloads run on the right hardware within your cluster, you just label them with the desired node pool.

Node pools are GKE feature rather than a Kubernetes feature. You can build a similar feature with open-source Kubernetes, but  you would have to maintain it yourself.

You can enable automatic node upgrade, automatic node repairs and cluster autoscaling at this node pool level.
Some of each node's CPU and memory are needed to run GKE and kubernetes components that let it work as part you your cluster.
So, if you allocate nodes with 15 GB of memory, not quite all of that 15 GB will be available for use by pods.

#### Zonal cluster

* Zone:
  * Cluster:
    * Master
    * Node Pool
      * Node1
      * Node2
      * Node3

By default a cluster launches in a single GCE zone. With three identical nodes all in one node pool. Nr of nodes can be changed, during or after the creation of the cluster. Adding more nodes and deploying multiple replicas will improve applications availability, but only up to a point.

#### Regional Cluster

* Region
  * Zone 1
    * same Cluster
  * Zone 2
    * same Cluster
  * Zone 3
    * same Cluster

Regional clusters have a singular API endpoint for the cluster, however it's masters and nodes are spread out across multiple GCE zones within a region. Regional clusters ensure that the availability of the application is maintained across multiple zones in single region. In addition, the availability of master is also maintained, so both the app and management functionality is maintained.
By default regional cluster is spread across three zones, each containing one master and three nodes.

Regional Cluster can not be converted to zonal cluster and vice-versa.

#### Private cluster

Zonal cluster and Regional cluster can be set as private cluster. The entire cluster is hidden from public internet. Cluster masters could be access by GCP Products, through internal IP address or through authorized networks via external IP address. Authorized networks are IP address ranges, that are trusted to access the master.

### Kubernetes Object Management

All Kubernetes objects are identified by an unique name and unique identifier.

Example: three nginx containers running all the time.
How wo we create Pods for these containers?
Declare three pods objects and specify their state: for each a pod must be created and nginx container must be used.

You define objects you want Kubernetes to create and maintain with manifest files. These are ordinary text files, you may write them in yaml or json.

Yaml ir more human readable and less tedious to edit.
You should ave yaml files in version controlled repositories.

~~~yaml
apiVersion: apps/v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
~~~

apiVersion - describes which Kubernetes API version is used to create the object.
kind - defines the object you want, in this case Pos
metadata - helps to identify the object using name and label.
name - names must be unique for pods, only one object of particular kind can have a particular name at the same time in Kubernetes name space (max. char length = 253).
Each object generated throughout the life of a cluster has a unique ID generated by Kubernetes. No two object will have the same UID.

labels - key-value pairs, with which you tag your objects during of after their creation. Labels help you identify and organize the objects.

~~~yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
    env: dev
    stack: frontend
spec:
  replicas: 3
  selector:
    matchLabels
    app: nginx
~~~

For example:

* key-value
* app: nginx

~~~Shell
kubectl get pods -selector=app=nginx
~~~

#### Pods have a life cycle

So one way to bring three nginx servers into being would be to declare 3 pod objects, each with its own section of YAML.Kubernetes default scheduling algorithm prefers to spread the workload evenly across the nodes available. So we would get three running nodes with one pod in each.

The downsides of executing this manually is at least two:

* If you would want 200 instances of nginx, there should be a better way of defining pod objects.
* Also pods do not heal or repair themselves, and they are not meant to run forever. They are designed to be ephemeral, and disposable.

#### Pods and controller object

For these reasons, there are better ways to manage what you run on Kubernetes than specifying individual pods.
So how do you tell Kubernetes to maintain the desired state of three nginx containers?
We can instead declare a Controller Object, whose job is to manage the state of the pods.

Controller:

* nginx Pod
* nginx Pod
* nginx Pod

Controller Object Type:

* Deployments
* StatefulSet
* DeamonSet
* Job

#### Deployments

Deployments are great choice for long-lived software components, like web-servers, especially when we want to manage them as a group.
When kube-scheduler schedules pods for a deployment, it notifies the kubeAPIserver. These changes are constantly monitored by controllers, especially by the deployment controller. The deployment controller will maintain three nginx pods. If one of those pods fails, the deployment controller will recognize the difference b/w the current state and the desired state and will try to fix it by launching new pod.

Instead of using multiple yaml manifests, or files for each pod, you used a single deployment yaml to launch three replicas of the same container.

#### Deployments ensure that sets of Pods are running

~~~yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
~~~

 A deployment ensures that a defined set of pods is running at any given time. Within it's object spec, you specify how many replica pods you want, how pods should run, which containers should run within these pods, and which volumes should be mounted.

Based on these templates, controllers maintain the pod's desired state within the cluster.

Deployments can do much more than this. You will see that later in the course.

#### Allocating resource quotas

Multiple projects run on a single cluster.
How can I allocate resource quota?
How do you keep everyone's work on cluster organized?

#### Namespace

Kubernetes allows you to abstract the physical cluster into several clusters using namespaces. Namespaces provide scope for naming resources, such as pods, deployments and controllers.

In this example, there are three namespaces in this cluster:

* Namespace Test
* Namespace Stage
* Namespace Prod

You cannot have duplicate object names in the same namespace. You can create 3 pods with the same name nginx Prod in different namespaces.

Namespaces also let's you implement resource quotas across the cluster. These quotas define limits for the resource consumption within a namespace. You are not required to use namespaces for your day-to-day management, you can also use labels. Still namespaces are a valuable tool.
Suppose, you want to spin up a copy of deployment as a quick test. Doing so in a new namespace makes it easy and free of name collisions.
There are thee initial namespaces in a cluster:

* Default - for object with no namespace defined
  * Pods
  * Deployments
* Kube-system - for objects created by Kubernetes systems itself
  * ConfigMap
  * Controllers
  * Secrets
  * Deployments
* Kube-public - objects that are publicly readable to all users

#### Best practice tip: namespace-neutral YAML

~~~bash
kubectl -n demo apply -f mypod.yaml
~~~

~~~yaml
apiVersion: apps/v1
kind: Pod
metadata:
  name: mypod
  namespaces: demo
~~~

### Kubernetes Controller Objects

Pods are cerated and destroyed dynamically. Although pods can communicate using pods assigned IP addresses, these IP addresses ar ephemeral. They are not guaranteed to remain constant, when pods are restarted or when scaling changes which nodes are used to run pods. Imagine you have two sets of pods:

* frontend pods
* backend pods

How will the frontend pods discover an keep track of dynamically changing backend pods? Is is where the concept of Kubernetes cervices comes in. A Kubernetes Service is a static IP address that represents a service of a function in your infrastructure. IT is a network abstraction for a set of pods, that deliver that service and it hides the ephemeral nature of the IP addresses of the individual pods.

In the example a set of backend pods are exposed to the front end pods, using a Kubernetes service. Basically, a service defines a set of pods and assigns a policy, by which you can access those pods. The pods are selected using a label selector.

By the way you can also get a service quickly by asking a Kubernetes to expose a deployment. When you do that, Kubernetes handles selecting the right pods for you. Whenever a service is created, Kubernetes automatically creates endpoints for the selected pods, by creating endpoint resources.

By default, master assigns a virtual IP address, also known as a cluster IP to the service from internal IP tables. With GKE this is assign from clusters VPC network. More about services later. GKE offers other ways your service can be exposed, not just through cluster IPs.

Overall, a service provides durable endpoints for pods. These endpoints can be accessed by exposing the service internally within a cluster, or externally, to the outside word. The option to expose a service internally or externally depends on the service type itself. The frontend pod can reliably access the backend pods internally within the cluster using a service.

A container application can easily write data to read/write layer inside a container, but it is ephemeral. So when a container terminates, whatever was written, will be lost. What is you want to store data permanently or what is you need the storage to be shared b/w tightly coupled containers within a pod? That is why a Kubernetes volume is used for more persistent storage. Kubernetes volume is another abstraction. 

* A Volume is simply a directory, that is accessible to all containers in a Pod.
* Requirements for a Volume are are defined through the Pods specification. This declares how a directory is created, what storage medium should be used and its initial contents.
* You do not want failure of containers or restarts of containers to effect the data within these volumes, and you want your Volume to be shared among multiple containers within a pod. Docker containers have their own file system. Therefore in order to access these volumes, they must e mounted specifically to each container within a pod.
* However, pods themselves are also ephemeral. A failing node or deleted pod could resulted in pods volume being deleted as well. To avoid this, you can configure Volumes using network based storage from outside of your pods, to provide durable storage that is not lost, when a pod or node fails. More about persistent volume later.

### Lab: AK8S-03 Creating a GKE Cluster via GCP Console

* Use the GCP Console to build and manipulate GKE clusters
* Use the GCP Console to deploy a Pod
* Use the GCP Console to examine the cluster and Pods

#### Deploy GKE clusters

##### Use the GCP Console to deploy a GKE cluster

Navigation menu-> Kubernetes Engine -> Clusters
Create cluster
name: standard-cluster-1
zone: us-central1-a
Kubernetes Engine > Clusters

Click the cluster name standard-cluster-1 to view the cluster details
Click the Storage and Nodes tabs

#### Modify GKE clusters

Navigation menu-> Kubernetes Engine -> Clusters
standard-cluster-1
Edit
Node Pools -> default-pool ->  Edit ->  change the number of nodes in the default pool from 3 to 4
Save

#### Deploy a sample workload

Navigation menu ->  Kubernetes Engine -> Workloads
Deploy
Continue, to accept the default container image, nginx.latest
Deploy

#### View details about workloads in the GCP Console

Navigation menu ->  Kubernetes Engine -> Workloads
Kubernetes Engine -> Workloads page -> nginx-1.
Overview ->  Managed Pods ->   of one of the Pods to view the details page for that Pod - provides information on the Pod configuration and resource utilization and the node where the Pod is running
Details -  shows more details about the workload including the Pod specification, number and status of Pod replicas and details about the horizontal Pod autoscaler
Revision History - displays a list of the revisions that have been made to this workload.
Events - lists events associated with this workload
YAML - provides the complete YAML file that defines this components and full configuration of this sample workload

