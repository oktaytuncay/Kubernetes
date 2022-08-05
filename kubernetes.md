## Kubernetes From Scratch to Advance 

## Contact info

Reach out to me at one of the following places!

- **Website at** <a href="https://www.linkedin.com/in/oktay-tuncay-8b147724/" target="_blank">`linkedin.com`</a>
- **Email:** oktaytncy@gmail.com

# Table of Contents

---
- [Core Concepts](#core-concepts)
  * [Docker vs Kubernetes](#docker-vs-kubernetes)
  * [PODs](#pods)
    + [How to Deploy Pods?](#how-to-deploy-pods-)
      - [YAML in Kubernetes](#yaml-in-kubernetes)
      - [Editing Existing Pods](#editing-existing-pods)
  * [Multi-Container Pods](#multi-container-pods)
    + [Init Containers](#init-containers)
    + [Multi-Container Patterns](#multi-container-patterns)
      - [The Sidecar Pattern](#the-sidecar-pattern)
      - [The Adapter Pattern](#the-adapter-pattern)
      - [The Ambassador Pattern](#the-ambassador-pattern)
  * [Kubernetes Replication Controllers](#kubernetes-replication-controllers)
    + [Load Balancing & Scaling](#load-balancing---scaling)
      - [Labels and Selectors](#labels-and-selectors)
    + [Scaling an Application](#scaling-an-application)
      - [Section Useful Commands](#section-useful-commands)
  * [Deployments](#deployments)
      - [Output Commands](#output-commands)
  * [Namespaces Walkthrough](#namespaces-walkthrough)
    + [Resource Quota](#resource-quota)
    + [Imperative commands](#imperative-commands)
      - [POD](#pod)
      - [Deployment](#deployment)
        * [Services in Deployments](#services-in-deployments)
  * [Configuration](#configuration)
    + [Commands & Arguments](#commands---arguments)
      - [Editing PODs and Deployments](#editing-pods-and-deployments)
        * [Edit PODs](#edit-pods)
        * [Edit Deployments](#edit-deployments)
      - [Environment Variables](#environment-variables)
        * [ConfigMaps](#configmaps)
          + [ConfigMap in Pods](#configmap-in-pods)
        * [Secrets](#secrets)
          + [Secrets in Pods](#secrets-in-pods)
  * [Security in Docker and Kubernetes](#security-in-docker-and-kubernetes)
    + [Process Isolation](#process-isolation)
    + [Users](#users)
    + [Security Contexts](#security-contexts)
    + [Service Account](#service-account)
      - [Resource Management](#resource-management)
      - [Taints & Tolerations](#taints---tolerations)
        * [Taint - NoExecute](#taint---noexecute)
      - [Node Selectors](#node-selectors)
      - [Node Affinity](#node-affinity)
    + [Observability](#observability)
      - [Understanding Health Probing](#understanding-health-probing)
      - [Readiness Probes](#readiness-probes)
      - [Liveness Probes](#liveness-probes)
      - [Startup Probe](#startup-probe)
      - [Container Logging](#container-logging)
      - [Monitor and Debug Applications](#monitor-and-debug-applications)
      - [Labels, Selectors and Annotations](#labels--selectors-and-annotations)
        * [Labels and Selectors](#labels-and-selectors-1)
        * [Annotations](#annotations)
  * [Rolling Updates & Rollbacks in Deployments](#rolling-updates---rollbacks-in-deployments)
    + [Rollout and Versioning](#rollout-and-versioning)
    + [Upgrades](#upgrades)
    + [Rollback](#rollback)
      - [Rollout status and history](#rollout-status-and-history)
      - [Wrap-Up](#wrap-up)
  * [Jobs in Kubernetes](#jobs-in-kubernetes)
    + [CronJobs](#cronjobs)
  * [Services & Networking](#services---networking)
    + [Service Types](#service-types)
      - [NodePort](#nodeport)
      - [Cluster IP](#cluster-ip)
      - [Recap](#recap)
  * [Cluster Networking](#cluster-networking)
    + [Ingress Networking](#ingress-networking)
      - [Rewrite-Target option](#rewrite-target-option)
    + [Network Policies](#network-policies)
      - [Developing Network Policies](#developing-network-policies)
  * [State Persistence](#state-persistence)
    + [Volumes](#volumes)
      - [Persistent Volumes](#persistent-volumes)
        * [Persistent Volumes on Oracle Cloud Infrastructure (OCI)](#persistent-volumes-on-oracle-cloud-infrastructure--oci-)
      - [Persistent Volume Claims](#persistent-volume-claims)
    + [Stateful Sets](#stateful-sets)
      - [Headless Services](#headless-services)
      - [Storage in StatefulSets](#storage-in-statefulsets)
    + [Authentication, Authorization and Admission Control](#authentication--authorization-and-admission-control)
      - [Authentication](#authentication)
      - [Security KubeConfig](#security-kubeconfig)
      - [API Groups](#api-groups)
      - [Authorization](#authorization)
        * [Create a Role](#create-a-role)
        * [Cluster Roles](#cluster-roles)
      - [Admission Controllers](#admission-controllers)
        * [Validating and Mutating Admission Controller](#validating-and-mutating-admission-controller)
    + [API Deprecations](#api-deprecations)
        * [Kubectl Convert](#kubectl-convert)
    + [Custom Resource Definitions (CRD)](#custom-resource-definitions--crd-)
      - [Custom Controllers](#custom-controllers)
    + [Deployment Strategies](#deployment-strategies)
      - [Blue/Green Deployment Strategy](#blue-green-deployment-strategy)
      - [Canary Deployment Strategy](#canary-deployment-strategy)
    + [Helm](#helm)
      - [Helm Installation](#helm-installation)
      - [Automation with Helm](#automation-with-helm)
  * [Useful Kubernetes Commands and Tricks](#useful-kubernetes-commands-and-tricks)
      - [Aliases](#aliases)
    + [Useful Commands](#useful-commands)
  * [Lab - Create a PostgreSQL Deployment](#lab-create-a-postgresql-deployment-1)
    + [Create a Namespace](#create-a-namespace)
    + [Create a ConfigMap](#create-a-configmap)
    + [Create PersistentVolumes and PersistentVolumeClaims](#create-persistentvolumes-and-persistentvolumeclaims)
    + [Create a Postgres Deployment](#create-a-postgres-deployment)
    + [Create a NodePort Service](#create-a-nodeport-service)
  + [Tutorial and Useful Links](#tutorial-and-useful-links)
---

# Core Concepts

**Node:** A node is a machine, physical or virtual which Kubernetes installed. And node is a worker machine and that is where containers will be launched by Kubernetes. It was also known as minions in the past.

**Cluster:** A cluster is a set of nodes grouped together.
The master node watches over the Nodes in the cluster and is responsible for the actual orchestration of containers on the worker nodes.

When you install Kubernetes on a system you're actually installing the following components.

- **An Api Server:** API Server acts as the front end for Kubernetes. The users, management devices, and command line interfaces; all talk to the API server to interact with the Kubernetes cluster.

- **An etcd service:** etcd service is a distributed reliable key-value store used by Kubernetes to store all data used to manage the cluster. When you have multiple nodes and multiple masters in your cluster, etcd store all that information on all the nodes in the cluster in a distributed manner, is also responsible for implementing locks within the cluster to ensure that there is no conflict between the masters.

- **A kubelet service:** Kubelet is the agent that runs on each node in the cluster. The agent is responsible for making sure that the containers are running on the nodes as expected.

- **A Container Runtime:** The container runtime is the underlying software that is used to run containers. In our case it happens to be Docker, but there are other options as well.

- **Controllers:** The controllers are the brain behind orchestration. They are responsible for noticing and responding when nodes, conatiners or end points goes down.

- **Schedulers:** The scheduler is responsible for distributing work or containers across multiple nodes. It looks for newly created containers and assigns them to nodes. The controllers make decisions to bring up new containers in such cases.

    ![pic1](images/1.png)

How are these components distributed across different types of servers? The worker node or minion as it is also known is where the containers are hosted. For example Docker containers and to run Docker containers on a system we need container **Container RunTime** installed.  

This does not have to be Docker, there are other container runtime alternatives available such as **rkt** (rocket) or **CRI-O**

The master server has the **kube-apiserver** and that is what makes it a master. Similarly, the worker nodes have to **kubetet** agent that is responsible for interacting with a master to provide health information of the worker node and carry out actions requested by the Master on the worker nodes. 

All the information gathered is stored in a key-value store on the master. The master also has the control manager and the scheduler.

**kubectl (kube control):** The kube control tool is used to deploy and manage applications on a Kubernetes cluster.

**Examples:**

- View information about the cluster
    ```
    kubectl cluster-info
    ```

- List all nodes part of the cluster
    ```
    kubectl get nodes
    ```

## Docker vs Kubernetes

![pic4](images/4.png)

A fundamental difference between Kubernetes and Docker is that Docker is a containerization platform, and Kubernetes is a container orchestrator for container platforms like Docker.

In Docker, we would need to establish network connectivity between containers ourselves using links and networks. 

We would need to create sharable volumes and share them among the containers and need to maintain a map of that as well. 

And most importantly, we will need to monitor the state of the application container, and when it dies, we will manually kill the helper container as it is no longer needed.

When a new container is deployed, we would need to deploy the new helper container as well with a pod.

Kubernetes does all of this for us automatically. We just need to define what containers a pod consists of and the containers in a pod by default will have access to the same storage, the same network, namespace, and the same fate as in they will be created together and destroyed together.

Even if our application didn't happen to be so complex and we could live with a single container, Kubernetes still requires you to create pods.

This is good in the long run as your application is now equipped for architectural changes and scale in the future.

## PODs

Kubernetes does not deploy containers directly on the worker nodes. The containers are encapsulated into a Kubernetes object known as pods. A pod is a single instance of an application and is the smallest object that you can create in the Kubernetes cluster.

![pic2](images/2.png)

What if you need to scale your application? 
We need to add additional instances of your Web application to share the load. 

Where would you pin up additioanl instances? Do we bring up new container instance within the same pod? 
The answer is no.

You need to create a new pod with a new instance of the same application. As you can see below, we have two instances of the Web application running on two separate pods on the same Kubernetes at a node.

So what if the load gets higher and the current node doesn't have enough capacity? 

Well, then you can always deploy additional pods on a new node in the cluster. You will have a new node added to the cluster to expand the cluster physical capacity.

A single pod can have multiple containers, except for the fact that they're usually not multiple containers of the same kind.
If our intention is to scale our application, then we would need to create additional pods.

![pic3](images/3.png)

Sometimes, we might have a scenario with processing a user inputting data, processing a user-uploaded file, etc and you want these helper containers to live alongside your application container.

In that case, you can have both of these containers, part of the same pod, so that when a new application container is created, the helper is also created and when it dies, the helper also dies. Since they are part of the same pod.

The two containers can also communicate with each other directly by referring to each other as localhost since they share the same network space. Plus they can easily share the same storage space as well.

**After this step, Kubernetes must be installed in order to test the commands. You can use the links below for installation.**


<a href="https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/" target="_blank">**`Installation Guide for Linux`**</a>

<a href="https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/" target="_blank">**`Installation Guide for MacOS`**</a>

<a href="https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/" target="_blank">**`Installation Guide for Windows`**</a>

### How to Deploy Pods?

```bash
$ kubectl run nginx --image nginx
pod/nginx created
```

The above command automatically creates a pod and deploys an instance of the Nginx docker image, but where does it get the app image?

The application image and in this case the Nginx image is downloaded from the repository docker hub. 
Docker hub is a public repository where the latest images of various applications are stored.

We could configure Kubernetes to pull the image from the public or a private repository within the organization.

Now that we have a pod created, how do we see the list of pods available. `kubectl get pods` command helps us see the list of pods in our cluster.

```bash
$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          8m57s
```

We created Nginx but how can a user access the Nginx Web server? 

We have not yet made the web server accessible to external users. We can access the node internally but for now, we cannot access the Web server from outside the node.

#### YAML in Kubernetes

Kubernetes uses YAML files as inputs for the creation of objects such as pods, replicasets, deployments, services, etc. 

All of these follow a similar structure. Kubernetes definition file always contains top-level fields.
The `APIversion`, `kind`, `metadata`, and `spec`. These are the top-level or root-level properties. 
These are also required fields, so you must have them in your configuration file.

We don't have to write YAML files from scratch every time, we can create a YAML file without creating the POD, with a command similar to the kubectl command we used in the previous section

**Example:**
```bash
$ kubectl run nginx --image nginx --dry-run=client -o yaml > pod.yaml
```

```properties
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
    labels:
        app: myapp
        type: front-end

spec:
    containers:
        - name: nginx-container
          image: nginx
```

**apiVersion:** The version of the Kubernetes API we are using to create the object. Depending on what we are trying to create, we must use the right API version. Since we're working on pods, we will set the API version as `v1`. Few other possible values for this field are apps/v1-beta extension/v1-beta etc.

**kind:** kind refers to a type of object we are trying to create which in this case happens to be a pod. So, we will set it as `Pod`. Could be a replica set, deployment, or service.

**metadata:** The metadata is data about the object like its name, labels, etc. Unlike the first two where we have specified a string value, this is in the form of a dictionary. 

Under metadata the name is a string value so we can name our pod. `myapp-pod` and the labels are a dictionary, so labels are a dictionary within the metadata dictionary.

And it can have any key-value pairs as we wis
For now, we have added a label app with the value `myapp`. 

Similarly, we could add other labels as we see fit which will us identify these objects at a later point in time.

For example. there are hundreds of pods running a front-end application and hundreds of pods running a backend application or a database. It will be difficult to group these pods once they're deployed.

If we label them now as `front-end`, `backend`, or `database`, we will be able to filter the parts based on this label at a later point in time.

It's important to keep in mind that under metadata we can only specify names or labels or anything else that Kubernetes expects to be under metadata.

We can not add any other property as we wish under metadata. However under the labels we can have any kind of key-value pairs we see fit. 

So it's important to understand what each of these parameters expect.

**spec:** Spec is dictionary has a property under it called `containers`. Containers are a list of an array. The reason this property is on the list is that the pods can have multiple containers with them. In this case, we will only add a single item to the list since we plan to have only a single container in the pod. 

The `-` right before the name indicates that this is the first item on the list. The item in the list is a dictionary so add the name and image property the value for the image is Nginx which is the name of the Docker image in the docker repository.

Once the file is created from the command `kubectl create -f pod-definition.yml` followed by the file name which is pod-definition.yml and Kubernetes creates a pod.

The table below summarizes the Type of Fields.

|field     |type      | 
|----------|----------|
|apiVersion|string    |
|kind      |string    |
|metadata  |dictionary|
|name      |string    |
|labels    |dictionary|
|app       |string    |
|type      |string    |
|spec      |dictionary|
|containers|list      |

To create a pod with the above yaml file;

```bash
$ kubectl create -f pod-definition.yaml
pod/myapp-pod created
```

`kubectl describe` command tells us about the pod; when it was created, what labels are assigned to it, what docker containers are a part of it, and the events associated with that pod.

```bash
$ kubectl describe pod nginx
Name:         nginx
Namespace:    default
Priority:     0
Node:         10.0.10.25/10.0.10.25
Start Time:   Fri, 22 Oct 2021 08:20:40 +0000
Labels:       run=nginx
Annotations:  <none>
Status:       Running
IP:           10.244.1.2
IPs:
  IP:  10.244.1.2
Containers:
  nginx:
    Container ID:   cri-o://bf5109681523311b60c40189b5efeee01114ff238f59861770b16def10f3aa7f
    Image:          nginx
    Image ID:       docker.io/library/nginx@sha256:644a70516a26004c97d0d85c7fe1d0c3a67ea8ab7ddf4aff193d9f301670cf36
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Fri, 22 Oct 2021 08:20:48 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-kd5vg (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-kd5vg:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-kd5vg
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:          <none>
```

**Example:**

We can add anything on the labels that will help you group this section in the future.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: front-end
    costcenter: EMEA
    location: Amsterdam
spec:
    containers:
        - name: nginx-container
          image: nginx
```

Before creating a pod with the above script, if it already exists, it can be listed and deleted with the following commands.

```bash
% kubectl get pods                  
NAME        READY   STATUS    RESTARTS   AGE
nginx       1/1     Running   0          2d19h

% kubectl delete pod nginx
pod "nginx" deleted

% kubectl create -f pod-definition.yaml
pod/myapp-pod created
```

If you have more than 1 image and you want to list all image names, the below command can be used.

```
% kubectl describe pod | grep Image
    Image:          nginx
    Image ID:       docker.io/library/nginx@sha256:644a70516a26004c97d0d85c7fe1d0c3a67ea8ab7ddf4aff193d9f301670cf36
```

To find the nodes where the pods are located

```
% kubectl get pods  -o wide
NAME        READY   STATUS    RESTARTS   AGE     IP           NODE         NOMINATED NODE   READINESS GATES
myapp-pod   1/1     Running   0          4h18m   10.244.1.3   10.0.10.25   <none>           <none>
```

#### Editing Existing Pods

1. If you are given a pod definition file, edit it and use it to create a new pod.

2. If you are not given a pod definition file, you may extract the definition to a file using the below command.

    ```bash
    kubectl get pod <pod-name> -o yaml > pod-definition.yaml
    ```

    Then edit the file to make the necessary changes, delete and re-create the pod.

3. Use the `kubectl edit pod <pod-name>` command to edit pod properties.

## Multi-Container Pods

The question often arises, **<span style="color:red"> Should I deploy my microservices stack to a single Pod with multiple containers, or should I create multiple Pods, each running a single microservice? </span>**

The short answer is to operate a single microservice per Pod. This modus operandi promotes a decentralized, decoupled, and distributed architecture. Furthermore, it helps with rolling out new versions of a microservice without necessarily interrupting other parts of the system.

So what’s the point of running multiple containers in a Pod then? 
There are two common use cases.

Sometimes, you’ll want to initialize your Pod by executing setup scripts, commands, or any other kind of pre-configuration procedure before the application container should start. This logic runs in so-called `initContainers`. 

Or, you'll want to provide helper functions that run alongside the application container to avoid the need to convert logic to application code. For example, you may want to have the log output produced by the application. Containers that run auxiliary logic are called `sidecar` patterns.

### Init Containers

Init containers are always started before the main application containers, which means they have their own lifecycle.

To split up the initialization logic, you can even distribute the work into multiple init containers that are run in the order of definition in the manifest. Of course, initialization logic can fail. If an init container produces an error, the whole Pod is restarted, causing all init containers to run again in sequential order.

For init containers, Kubernetes provides a separate section: spec.initContainers. Init containers are always executed before the main application containers, regardless of the definition order in the manifest.

**Example:**

```properties
apiVersion: v1
kind: Pod
metadata:
  name: business-app
spec:
  initContainers:
  - name: configurer
    image: busybox
    command: ['sh', '-c', 'echo Configuring application... && \
              mkdir -p /usr/shared/app && echo -e "{\"dbConfig\": \
              {\"host\":\"localhost\",\"port\":5432,\"dbName\":\"customers\"}}" \
              > /usr/shared/app/config.json']
    volumeMounts:
    - name: configdir
      mountPath: "/usr/shared/app"
  containers:
  - image: bmuschko/nodejs-read-config:1.0.0
    name: web
    ports:
    - containerPort: 8080
    volumeMounts:
    - name: configdir
      mountPath: "/usr/shared/app"
  volumes:
  - name: configdir
    emptyDir: {}
```

Errors can occur during the execution of init containers. You can always retrieve the logs of an init container by using the `--container` command-line option (or `-c` in its short form)

```bash
kubectl logs business-app -c configurer
```

The lifecycle of an init container is as follows: it starts, runs its logic, and terminates when the work is done. Init containers are not meant to keep running over a longer period of time.

### Multi-Container Patterns

#### The Sidecar Pattern

There are scenarios that call for a different usage pattern. For example, you may want to create a Pod that runs multiple containers continuously alongside one another.

Typically, there are two different categories of containers: the container that runs the application and another container that provides helper functionality to the primary application. 

In Kubernetes, the container providing helper functionality is called a sidecar. 

Among the most commonly used capabilities of a sidecar container are file synchronization, logging, and watcher capabilities. The sidecars are not part of the main traffic or API of the primary application. They usually operate asynchronously and are not involved in the public API.

![pic55](images/55.png)

**Example YAML:**
```properties
apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: logs-vol
      mountPath: /var/log/nginx
  - name: sidecar
    image: busybox
    command: ["sh","-c","while true; do if [ \"$(cat /var/log/nginx/error.log \
              | grep 'error')\" != \"\" ]; then echo 'Error discovered!'; fi; \
              sleep 10; done"]
    volumeMounts:
    - name: logs-vol
      mountPath: /var/log/nginx
  volumes:
  - name: logs-vol
    emptyDir: {}
```

When starting up the Pod, we’ll notice that the overall number of containers will show 2. 

#### The Adapter Pattern

The adapter pattern transforms the output produced by the application to make it consumable in the format needed by another part of the system.

![pic56](images/56.png)

The business application running the main container produces the available disk space and writes it to the file space.txt.

As part of the architecture, we want to consume the file from a third-party monitoring application.

The problem is that the external application requires the information to exclude the timestamp. We could change the logging format to avoid writing the timestamp, but we need to know when the log entry has been written.

This is where the `adapter pattern` can help. A sidecar container executes transformation logic that turns the log entries into the format needed by the external system without having to change application logic.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: adapter
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - 'while true; do echo "$(date) | $(du -sh ~)" >> /var/logs/diskspace.txt; \
       sleep 5; done;'
    image: busybox
    name: app
    volumeMounts:
      - name: config-volume
        mountPath: /var/logs
  - image: busybox
    name: transformer
    args:
    - /bin/sh
    - -c
    - 'sleep 20; while true; do while read LINE; do echo "$LINE" | cut -f2 -d"|" \
       >> $(date +%Y-%m-%d-%H-%M-%S)-transformed.txt; done < \
       /var/logs/diskspace.txt; sleep 20; done;'
    volumeMounts:
    - name: config-volume
      mountPath: /var/logs
  volumes:
  - name: config-volume
    emptyDir: {}
```

#### The Ambassador Pattern

The ambassador pattern provides a proxy for communicating with external services.

Typical responsibilities include retry logic upon a request failure, security concerns like providing authentication or authorization, or monitoring latency or resource usage.

Let's assume, We want to implement rate-limiting functionality for HTTP(S) calls to an external service. For example, the requirements for the rate limiter could say that an application can only make a maximum of 5 calls every 15 minutes.

Instead of coupling the rate-limiting logic to the application code, it will be provided by an ambassador container.

All calls from the business application must be routed through the ambassador container.

![pic57](images/57.png)

## Kubernetes Replication Controllers

![pic5](images/5.png)

Containers are the brain behind Kubernetes. They're the processes that monitor Kubernetes objects and respond accordingly.

One of the most important controllers is the replication controller.

To prevent users from losing access to the application, we would like to have more than one instance or pod running at the same time. That way, If one fails, we still have our application running on the other one.

The replication controller helps us multiple instances of a single pod in the Kubernetes cluster, thus providing high availability.

Even if you have a single pod, the replication controller can help by automatically bringing up a new pod when the existing one fails, thus the replication controller ensures that the specified number of pods are running at all times, even if it's just one or hundred.

Worth to mention that Replication Controller will be deprecated in future, and Replica Set will be the de-facto standard for Pod controlling.

### Load Balancing & Scaling

Another reason we need a replication controller is to create multiple pods to balance the load across them.

For example, we have a single pod serving a set of users, when the number of users increases, we deploy an additional pod to balance the load across the two pods.

If the demand further increases and if we run out of resources on the first node, we could deploy additional nodes across the other nodes in the cluster.

It's important to note that there are two similar terms, `Replication Controller` and `ReplicaSet` both have the same purpose, but they're not the same.

Replication Controller is the older technology that is being replaced by ReplicaSet, which is the new recommended way to set up replication.

**Example:**
The Replication Controller configuration file and the previously used configuration file are very similar to each other.

The extra thing to do here is to move all the contents of the pod definition (metadata and spec parts) to the template section of the file replication controller. They should be children of the template section.

Looking at our file now, we have two metadata sections; one is for the replication controller and another for the pod. And also we have two spec sections one for each.

The replicas parameter is the number that determines how many pods there will be at a time. It is important to note that replicas and template sections are direct children of the spec secti

```properties
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: frontend
spec:
  template:
   ---
```

```properties
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: frontend
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end    
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
```

Using the YAML file, to create a Replication Controller and check its status;

```bash
% kubectl create -f rc-definition.yml 
replicationcontroller/myapp-rc created

% kubectl get replicationcontroller
NAME       DESIRED   CURRENT   READY   AGE
myapp-rc   3         3         3       91s
```

If you would like to see the pods that were created by the replication controller and you will see three pods running. All of them are starting with the name of the replication controller which is myapp-rc, indicating that are all created automatically by the replication controller.

```bash
% kubectl get pods            
NAME             READY   STATUS    RESTARTS   AGE
myapp-rc-gfr2j   1/1     Running   0          54s
myapp-rc-kvq6d   1/1     Running   0          54s
myapp-rc-x4d2k   1/1     Running   0          54s
```

**Replicaset Example:**

It's very similar to replication controller. 
- The `apiVersion` is different from what we had before for the application controller
- The spec section is exactly the same except!
  - ReplicaSet requires a selector definition, the selector section helps the ReplicatSet identify what pods fall under it. It's because ReplicaSet also manages pods that were not created as part of the ReplicaSet creation. For example, there were pods created before the creation of the ReplicaSet that matched labels specified in the selector. The ReplicaSet also takes those pods into consideration when creating the replicas.
  The selector is not a required field in the case of a replication controller, but it's still available. When you skip, it assumes it to be the same as the labels provided in the pod definition file. In the case of ReplicaSet, user input is required for this property and it has to be written in the form of `MatchLabels`.
- A MatchLabel selector simply matches the labels specified under it to the labels on the pod.
- The ReplicaSet selector also provides many other options for matching labels that were not available in the replication controller.

```properties
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replica
  labels: 
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end    
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      type: front-end
```

Using the YAML file, to create a ReplicaSet and check its status;

```bash
% kubectl create -f replicaset-def.yml
replicaset.apps/myapp-replica created

% kubectl get pods                    
NAME                  READY   STATUS    RESTARTS   AGE
myapp-replica-kdl2k   1/1     Running   0          16s
myapp-replica-v5srg   1/1     Running   0          16s
myapp-replica-xnqbz   1/1     Running   0          16s

% kubectl get replicaset
NAME            DESIRED   CURRENT   READY   AGE
myapp-replica   3         3         3       41s
```

#### Labels and Selectors

![pic6](images/6.png)

Why do we label our pods and objects in Kubernetes?

Say we deployed three instances of our front-end Web application as three pods. We would like to create a replication controller or replicaset to ensure that we have three active pods at any time.

In case they were not created, the replicaset will create them for us. The role of the replicaset is to monitor the pods and if any of them were to fail, deploy a new one. The replica set is in fact a process that monitors the pods.

There could be hundreds of other pods in the cluster running different applications. This is where labeling our pods during creation comes in handy.

```properties
metadata:
  name: myapp-pod
  labels:
    tier: front-end
```

```properties
selector:
  matchLabels:
    tier: front-end
```

We could provide labels as a filter for replicaset under the selector section, by using the matchLabels filter we can provide the same label we used when creating the pods.

![pic7](images/7.png)

This way, the replicaset knows which partitions it should monitor
The same conecpt of labels and selectors is used in many other places across Kubernetes.

### Scaling an Application

Say we started with three replicas in the future we decided to scale to six.
There are multiple ways to do it.

1. Update the number of replicas in the definition file to six.

    ```
    replicas: 6
    selector:
      matchLabels:
        type: front-end
    ```

    Then run the `kubectl replace -f replicaset-definition.yml` command to specify the same file and that will update the replica set to have six replicas.

2. Run the `kubectl scale` command used the replicas parameter to provide the new number of replicas and specify the same file as input.

    ```
    kubectl scale --replicas=6 -f replicaset-definition.yml
    ```

    We either input the definition file or provide the replicaset name in the type name format.

    ```
    kubectl scale --replicas=6 replicaset myapp-replicaset
    ```

    However, remember that using the filename as an input will not update the existing number of replicase in the file.

    In other words, the number of replicas in the replicaset-definition.yml file will still be three, even though you scaled your replica set to have six replicas using the kubectl scale command and the file as input.

#### Section Useful Commands

- Create a replicaset 
  > kubectl create -f replicaset-definition.yml

- List of public assets created
  > kubectl get replicaset

- Delete the replicaset and also deletes all underlying PODs
  > kubectl delete replicaset myapp-replicaset

- Replace or update the replica set.
  > kubectl replace -f replicaset-definition.yml

- Scale replicaset simply from the command line without having to modify the file
  > kubectl scale -replicas=6 -f replicaset-definition.yml

- Edit replicaset
  > kubectl edit replicaset

## Deployments

![pic8](images/8.png)

You have a Web server that needs to be deployed in a production environment. You need many instances of running a web server. And also whenever a newer version of application builds become available on the Docker registry and if you upgrade them all at once, this may impact users accessing the application. So, you might want to upgrade them one after the other.

Let's say your upgrade resulted in an error and you want to roll back the changes that were recently carried out.

Another scenario that can be illustrated is, that you would like to make multiple changes to your environment such as upgrading the underlying web server versions, scaling the environment, modifying the resource allocations, etc...

And you would like to apply a pause to your environment, make the changes, and then resumes. So, all the changes are rolled out together.

All of these capabilities are available with the Kubernetes deployments.

Each container is encapsulated in PODs. Multiple set PODs are deployed using replication controllers or replicaset.
And then comes deployment which is a Kubernetes object that comes higher in the hierarchy.

The deployment provides us with the capability to upgrade the underlying instances seamlessly using rolling updates, undo changes, pause and resume changes as required.

Replicaset and deployment definition files are similar. 

A ReplicaSet ensures that a specified number of pod replicas are running at any given time. However, a Deployment is a higher-level concept that manages ReplicaSets and provides declarative updates to Pods along with a lot of other useful features.

Therefore, Deployments are recommended instead of directly using ReplicaSets, unless you require custom update orchestration or don't require updates at all.

```properties
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels: 
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end    
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      type: front-end
```

```bash
% kubectl create -f deployment-definition.yml
deployment.apps/myapp-deployment created

% kubectl get replicaset                  
NAME                          DESIRED   CURRENT   READY   AGE
myapp-deployment-7df67f74c5   3         3         3       6s

% kubectl get pods      
NAME                                READY   STATUS    RESTARTS   AGE
myapp-deployment-7df67f74c5-lnnzq   1/1     Running   0          13s
myapp-deployment-7df67f74c5-ndb8n   1/1     Running   0          12s
myapp-deployment-7df67f74c5-zdfgq   1/1     Running   0          12s
```

Deployment automatically creates a Replica Set and PODs. If we run the commands, we will be able to see the PODs with the name of deployment and replicaset.

So far there has not been much of a difference between replicaset and deployments. Except for the fact that deployment created a new Kubernetes object called deployments.

- To see all created objects at once
  > kubectl get all

  ```bash
  % kubectl get all
  NAME                                    READY   STATUS    RESTARTS   AGE
  pod/myapp-deployment-7df67f74c5-lnnzq   1/1     Running   0          4m52s
  pod/myapp-deployment-7df67f74c5-ndb8n   1/1     Running   0          4m51s
  pod/myapp-deployment-7df67f74c5-zdfgq   1/1     Running   0          4m51s

  NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
  service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5d1h

  NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
  deployment.apps/myapp-deployment   3/3     3            3           4m52s

  NAME                                          DESIRED   CURRENT   READY   AGE
  replicaset.apps/myapp-deployment-7df67f74c5   3         3         3       4m52s
  ```

#### Output Commands

The default output format for all kubectl commands is the human-readable plain-text format. The -o flag allows us to output the details in several different formats.

> kubectl [command] [TYPE] [NAME] -o <output_format>

Here are some of the commonly used formats:

1. `-o json` output a JSON formatted API object.

2. `-o name` print only the resource name and nothing else.

3. `-o wide` output in the plain-text format with any additional information.

4. `-o yaml` output a YAML formatted API object.

**You can check the following links for more details.**

- [Overview](https://kubernetes.io/docs/reference/kubectl/overview/)

- [Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Namespaces Walkthrough

![pic9](images/9.png)

Let's assume there are two boys named Michael to differentiate them from each other, we call them by their last names Jordan and Jackson.
They come from different houses and there are other members in the house. Individuals in the household only address each other by their first names.

For example, the father addresses Michael. However, if the father wishes to address Michael in the other house, he uses his full name. (Michael Jackson)

Someone outside of these houses would also use the full name to refer to anyone in these houses.

Each of these houses has its own set of rules that defines who does what. Each of these houses has its own set of resources that can consume.

These houses correspond to Namespaces in Kubernetes.

We have created objects such as PODs, Deployments and services in our cluster. Whatever we have been doing, we have been doing within a Namespace.

We were inside a house all this while, this namespace is known as the default Namespace and it's created automatically by Kubernetes when the cluster is the first set up.

Kubernetes creates a set of PODs and services for it's internal purpose such as those required by the networking solution, the DNS service, etc.

To isolate these from the user and to prevent accidentally deleting or modifying the services, Kubernetes creates them under another Namespace created at cluster startup named `kube-system`.

A third namespace created by Kubernetes automatically is called `kube-public. This is where resources that should be made available to all users are created.

![pic10](images/10.png)

If you want to use the same cluster in both Dev and Production environment but at the same time isolate the resources between them, you can create a different namespace for each of them.

That way while working in the Dev environment you don't accidentally modify resources in production.

Each of these Namespaces can have it's own set of policies that define who can do what.

We can also assign POD of resources to each of these Namespaces, that way each Namespace is guaranteed a certain amount and does not use more than its allowed limit.

The default Namespace that we've been working on, just like how the members within the house refer to each other by their first names. The resources within a namespace can refer to each other simply by their names.

**Example:**
  - **Namespace:** Default
  - **Objects:**
    - web-pod
    - db-service
    - web-deployment

In this case, the webapp-pod can reach db-service simply using the hostname db-service.

> mysql.connect("db-service")

If required, the webapp-pod can reach service in another Namespace as well. For this, you must append the name of the namespace to the name of the service.

For example for the web-pod in the default namespace to connect to the database in the Dev environment, in this case, the format should be `service.namespace.svc.cluster.local` 

> mysql.connect("db-service.dev.svc.cluster.local")

We're able to do this because when the service is created a DNS entry is added automatically in this format.

Looking closely at the DNS name of the service, the last part `cluster.local` is the default domain name of the Kubernetes cluster `.svc` is the subdomain for service followed by the namespace and then the name of the service itself.

![pic11](images/11.png)

To list pods in another namespace, we need to use the namespace option in the command along with the name of the namespace.

```bash
% kubectl get pods --namespace=kube-system
NAME                                   READY   STATUS    RESTARTS   AGE
coredns-7db99fc7fb-7fj9c               1/1     Running   0          6d11h
coredns-7db99fc7fb-lpjnv               1/1     Running   0          6d11h
coredns-7db99fc7fb-zt4ww               1/1     Running   0          6d11h
csi-oci-node-cxh6l                     1/1     Running   1          6d11h
csi-oci-node-h5jk9                     1/1     Running   0          6d11h
csi-oci-node-hmskj                     1/1     Running   0          6d11h
kube-dns-autoscaler-696f8695ff-rwbdd   1/1     Running   0          6d11h
kube-flannel-ds-gr4f6                  1/1     Running   0          6d11h
kube-flannel-ds-klfxr                  1/1     Running   0          6d11h
kube-flannel-ds-r6dsz                  1/1     Running   0          6d11h
kube-proxy-6lq74                       1/1     Running   0          6d11h
kube-proxy-lp25n                       1/1     Running   0          6d11h
kube-proxy-qm7k2                       1/1     Running   0          6d11h
proxymux-client-lhcjc                  1/1     Running   0          6d11h
proxymux-client-lzfzm                  1/1     Running   0          6d11h
proxymux-client-p5cfh                  1/1     Running   0          6d11h
```

To create a pod in another namespace, we need to use the namespace option.

> kubectl create -f pod-definition.yml --namespace=dev

If we want to make sure that this pod gets created in the dev environment all the time even if we don't specify the namespace in the command line, we can put the namespace definition into the pod definition file under the metadata section.

```properties
metadata:
  name: myapp-pod
  namespace: dev
  ...
```

So, how do we create a new namespace? Like any other object, use a namespace definition file. The API version is v1, kind is the namespace and under metadata specify the name.

```properties
apiVersion: v1
kind: namespace
metadata:
  name: dev
```

```bash
% kubectl create -f namespace-dev.yml
namespace/dev created
```

Another way to create a namespace is by simply running the command kubectl create namespace followed by the name of the namespace.

```bash
% kubectl create namespace prod
namespace/prod created
```

But what if we want to switch to the dev namespace permanently so that we don't have to specify the namespace option anymore?

In that case, use the kubectl config command to set the namespace in the current context to dev.

> kubectl config set-context $(kubectl config current-context) --namespace=dev

```bash
% kubectl create -f pod-definition.yml --namespace=dev
pod/myapp-pod created

% kubectl config set-context $(kubectl config current-context) --namespace=dev
Context "context-cklkyy5l4pa" modified.

% kubectl get pod
NAME        READY   STATUS    RESTARTS   AGE
myapp-pod   1/1     Running   0          54s
```

Similarly, we can switch to the prod namespace the same way. To view pods in all namespaces, we can use the all namespaces option in the command.

```bash
% kubectl get pods --all-namespaces

NAMESPACE     NAME                                   READY   STATUS    RESTARTS   AGE
default       myapp-deployment-7df67f74c5-lnnzq      1/1     Running   0          34h
default       myapp-deployment-7df67f74c5-ndb8n      1/1     Running   0          34h
default       myapp-deployment-7df67f74c5-zdfgq      1/1     Running   0          34h
dev           myapp-pod                              1/1     Running   0          3m24s
kube-system   coredns-7db99fc7fb-7fj9c               1/1     Running   0          6d11h
kube-system   coredns-7db99fc7fb-lpjnv               1/1     Running   0          6d11h
kube-system   coredns-7db99fc7fb-zt4ww               1/1     Running   0          6d11h
kube-system   csi-oci-node-cxh6l                     1/1     Running   1          6d11h
kube-system   csi-oci-node-h5jk9                     1/1     Running   0          6d11h
kube-system   csi-oci-node-hmskj                     1/1     Running   0          6d11h
kube-system   kube-dns-autoscaler-696f8695ff-rwbdd   1/1     Running   0          6d11h
kube-system   kube-flannel-ds-gr4f6                  1/1     Running   0          6d11h
kube-system   kube-flannel-ds-klfxr                  1/1     Running   0          6d11h
kube-system   kube-flannel-ds-r6dsz                  1/1     Running   0          6d11h
kube-system   kube-proxy-6lq74                       1/1     Running   0          6d11h
kube-system   kube-proxy-lp25n                       1/1     Running   0          6d11h
kube-system   kube-proxy-qm7k2                       1/1     Running   0          6d11h
kube-system   proxymux-client-lhcjc                  1/1     Running   0          6d11h
kube-system   proxymux-client-lzfzm                  1/1     Running   0          6d11h
kube-system   proxymux-client-p5cfh                  1/1     Running   0          6d11h
```

Taking a closer look at the command `kubectl config set-context $(kubectl config current-context) --namespace=dev`, this command first identifies the current context and then sets the namespace to the desired one for that current context. Contexts is used to manage multiple clusters in multiple environments from the same management system.

### Resource Quota

To limit resources in a namespace, we can create a resource quota. We first need to specify the namespace for which we want to create a quota and the under spec, we can provide our limits and then create the quota.

```properties
apiVersion: v1
kind: ResourceQuota
metadata
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    request.cpu: "4"
    request.memory: 5Gi
    limits.pu: "10"
    limits.memory: 10Gi
```

> kubectl create -f compute-quota.yaml

### Imperative commands

Although we mostly work declaratively using definition files, we can quickly handle one-time tasks using imperative commands and at the same time easily create a definition template.

By default as soon as the command is run, the resource will be created. If you simply want to test your command use the below option. This will not create the resource, instead, tell you whether the resource can be created and if your command is right.

  - `--dry-run=client`

#### POD

1. Create an NGINX Pod
  - `kubectl run nginx --iname=nginx`

2. Generate Pod manifest yaml file with `-o yaml option`. This command will not create it with the `--dry-run` option. 

  - `kubectl run nginx --image=nginx --dry-run=client -o yaml`

#### Deployment

1. Generate deployment with 4 replicas

  - `kubectl create deployment nginx --iname=nginx --replicas=4`

2. Scale a deployment 

  - `kubectl scale deployment nginx --replicas=5`

3. Alternatife way to generate deployment is to save the yaml definition to a file and modify.

  - `kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml`

  We can save then update the yaml file with the other fields before creating the deployment.

##### Services in Deployments

1. Create a service named redis-service of type ClusterIP to expose pod redis on port 6379. The following command will automatically use the pod's labels as selectors.

  - `kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`

2. Alternatife way of 1. The following command will not use the pods labels as selectors, instead it will assume selectors as **app=redis**

  - `kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml`

3. Create a service named ginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes. The following command will automatically use the pod's labels as selectors but we can not specify the node port. We have to generate a definition file and then add the node port manually before creating the service with the pod.

  - `kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml`

4. Alternatife way of 3. The following command will not use the pods labels as selectors

  - `kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml`

    Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port.

    If you need to specify a node port, create a definition file using the same command and enter the node port manually before creating the service.

 [Reference Link](https://kubernetes.io/docs/reference/kubectl/conventions/)

**Examples:** 

1. Create a new pod called custom-nginx using the nginx image and expose it on container port 8080.

    > kubectl run custom-nginx --image=nginx --port=8080

2. Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80.

    > kubectl run httpd --image=httpd:alpine --port=80 --expose

## Configuration

### Commands & Arguments

To create a simple docker image that sleeps for a given number of seconds, we can named it ubuntu-sleeper and use the following command.

> docker run --name ubuntu-sleeper ubuntu-sleeper

By default, it sleeps for five seconds, but we can override by passing a command line argument.

> docker run --name ubuntu-sleeper ubuntu-sleeper 10

We will now create a pod using this image with additional argument in the pod definition file.

Anything that is appended to the docker run command will go into the `args` property off the pod definition file in the form of an array line below.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      args: ["10"]
```

The docker file has an entry point as well as CMD instructions specified. The entry point is the command that is run at startup and the CMD is the default parameter passed to the command. With the args option in the pod definition file, we override the code CMD instruction in the docker file.

But what if we need to override the entry point? For instance, from sleep to an imaginary sleep 2.0 command.

In the docker world, we can run with the entry point option set to the new command as follows.

> docker run --name ubuntu-sleeper --entrypoint sleep2.0 ubuntu-sleeper 10

The corresponding entry in the pod definition file would be using a command field. The command field corresponds to the entry point instruction in the docker file.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: ["sleep2.0"]
      args: ["10"]
```

To summarize, there are two fields that correspond to instructions in the docker file, 
- The command field overrides the entry point instruction 
- The args field overrides the command instruction in the docker file.

#### Editing PODs and Deployments

##### Edit PODs

We can not edit the specifications of an existing POD other than the below.

- spec.containers[*].image
- spec.initContainers[*].image
- spec.activeDeadlineSeconds
- spec.tolerations

For example, we cannot edit the environment variables, service accounts, or resource limits of a running pod. But if we really want to, we have 2 options.

1. Run the `kubectl edit pod <pod name>` command.

  This command will open the pod specification in an editor (vi editor). Then edit the required properties. When we try to save it, you will be denied.

  This is because we are attempting to edit a field on the pod that is not editable.

  ```bash
    % kubectl edit pod myapp-pod
    error: pods "myapp-pod" is invalid
    A copy of your changes has been stored to "/var/folders/7v/btvm72w972qbg3zcxgfnl80m0000gn/T/kubectl-edit-1351316848.yaml"
    error: Edit cancelled, no valid changes were saved
  ```

  A copy of the file with your changes is saved in a temporary location as shown above.

  We can then delete the existing pod by running the command,

  ```bash
  kubectl delete pod webapp
  ```

  Then create a new pod with your changes using the temporary file,

  ```bash
  kubectl create -f /var/folders/7v/btvm72w972qbg3zcxgfnl80m0000gn/T/kubectl-edit-1351316848.yaml
  ```

2. The second option is to extract the pod definition in YAML format to a file using the command.

  ```bash
  kubectl get pod myapp-pod -o yaml > my-new-pod.yaml
  ```

  Then make the changes to the exported file using an editor (vi editor). Save the changes

  > vi my-new-pod.yaml

  Then delete the existing pod

  > kubectl delete pod myapp-pod

  Then create a new pod with the edited file

  > kubectl create -f my-new-pod.yaml

##### Edit Deployments

With Deployments, we can easily edit any field/property of the POD template.

Since the pod template is a child of the deployment specification, with every change the deployment will automatically delete and create a new pod with the new changes. 

So if you are asked to edit a property of a POD part of deployment we may do that simply by running the command

> kubectl edit deployment my-deployment

#### Environment Variables

When we want to set an environment, we can use the env attribute in the YAML file. Env is an array so every item under the env property starts with a "-" indicating an item in the array.

Each item has a name and a value property.
The name is the name of the environment variable made available with the container and the value is its value.

```properties
spec:
  env:
    - name:  app_colur
      value: blue
```

A direct way of specifying the environment variables using a plain key-value pair format. However, there are other ways of setting the environment variables such as using config maps and secrets.

The difference, in this case, is that instead of specifying value we say `valueFrom` and the specification of conflict maps and secret keys.

```properties
spec:
  env:
    - name:  app_colur
      valueFrom:
        configMapKeyRef:
```

```properties
spec:
  env:
    - name:  app_colur
      value: blue
```

##### ConfigMaps

When we have a lot of pod definition file, It will become difficult to manage the environment data stored within the various files.

We can take this information out of the pod definition file and manage it centrally using configuration maps.

Config maps are used to pass configuration data in the form of key-value pairs in Kubernetes.

When the pod is created, the config map is injected into the pod. So the key-value parents are available as environment variables for the application hosted inside the container in the pod.

There are two phases involved in configuring config maps.

1. Create the ConfigMap
2. Inject them into the pod.

There are two ways of creating a ConfigMap. The imperative way, without using a definition file, and the declarative way by using a config map definition file.

<span style="color:red">Imperative</span>
> kubectl create configmap

<span style="color:green">Declarative</span>
> kubectl create -f config-map.yaml

**<span style="color:red">Imperative Example:</span>**

```bash
kubectl create configmap \
  <config-name> --from-literal=<key>=<value>
```

```bash
kubectl create configmap \
  app-config --from-literal=APP_COLOR=blu
```
If you wish to add addinitiol key value pairs, simply specify the from-literal options multiple times.

```bash
kubectl create configmap \
  app-config --from-literal=APP_COLOR=blu \
  app-config --from-literal=APP_MOD=prod
```

However, this will get complicated when we have too many configuration items.

Another way to input configuration data is through a file.

```bash
kubectl create configmap \
  app-config --from-file=app_config.properties
```

**<span style="color:green">Declarative Example:</span>**

For the declarative approach, we can create a definition file. In the same way as in the pod file; the file has apiVersion, type, and metadata. But here we have data instead of spec. Let's say the file is named config-map.yaml.

```properties
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_MODE: prod
```
> kubectl create -f config-map.yaml

We can create as many ConfigMaps as we need in the same way for various different purposes.

- To list configmaps
  > kubectl get configmaps

- To list the configuration data
  > kubectl describe configmaps

###### ConfigMap in Pods

Below is a simple example pod definition file for the web application. 

```properties
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-colur
  labels:
    name: simple webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
```

To inject an environment variable, add a new property to the container called `envFrom`. The envFrom property is a list so we can pass as many environment variables as required.

Each item in the list corresponds to a configmap item. We can specify the name of the configMap we created earlier.

```properties
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
    envFrom:
      - configMapRef:
          name: app-config
```

The name of the ConfigMapRef should be the same as the name of the config-map we created before. And now we just need to create a pod with the above definition file.

> kubectl create -f pod-definition.yaml

##### Secrets

Let's assume, we have a simple python application that connects to the MySQL database.

```python
import os
from flash import Flask

app = Flask (__name__)

@app.route("/")
def main():

  mysql.connector.connect(host=`mysql`, database=`mysql`, user=`root`, password=`passwd`)

  return render_template('hello.html', color=fetchcolor())

if __name__ == "__main__":
  app.run(host="0.0.0.0", port="8080") 
```

In this code; hostname, username, and password are hard-coded which is not recommended.

One option would be to move these values into a conflict map but the conflict map stores configuration data in plain text format. So, it's definitely not the right place to store a password.

Secrets are used to store sensitive information like passwords or keys. They're similar to conflict maps except that they're stored in an encoded or hashed format as with conflict maps.

There are two steps involved in working with secrets.

1. Create a secret.
2. Injected into the pod.

There are two ways of creating a secret. As in ConfigMap, imperative and declarative.

<span style="color:red">Imperative</span>

```bash
kubectl create secret generic 
  <secret-name> --from-literal=<key>=<value>
```

If you want to add additional key-value pairs, you can add as many --from-literal options as you want.

```bash
kubectl create secret generic \
  app-secret --from-literal=DB_Host=mys \
             --from-literal=DB_user=root \
             --from-literal=DB_Password=paswrd
```

However, this could get complicated when you have too many secrets to pass. Another way, to input, the secret data is through the file. Use the from file option to specify a path to the file that contains the required data.

```bash
kubectl create secret generic \
  <secret-name> --from-file=app_secret.properties
```

The data from this file is read and stored under the name of the file. 

<span style="color:green">Declarative</span>

```bash
kubectl create -f secret-data.yml
```

For the declarative method, we create a definition file just like how we did the conflict map. The file contains apiVersion, type, meta, and data section as below, and let's name the file secret-data.yaml to use it in the next steps.

```properties
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: mysql
  DB_User: root
  DB_Password: paswrd
```

Here we have specified the data in plain text which is not very secure. So while creating a secret with a declarative approach, we must specify the secret values in a hashed format as below.

```properties
data:
  DB_Host: bXlzcWw=
  DB_User: cm9vdA==
  DB_Password: cGFzd3Jk
```

But how do we convert the data from plain text to an encoded format? 
On a Linux host, run the command `echo -n` followed by the exte we're trying to convert.

```bash
% echo -n 'mysql' | base64
bXlzcWw=

% echo -n 'root' | base64
cm9vdA==

% echo -n 'paswrd' | base64
cGFzd3Jk
```

`kubectl get secrets` command lists user-created and Kubernetes-created secrets.

```bash
% kubectl get secrets
NAME                  TYPE                                  DATA   AGE
default-token-pnx9d   kubernetes.io/service-account-token   3      5d14h
```

To view more information, the `kubectl describe secrets` command can be used. This shows the attributes in the secret but hides the value themselves to view the values as well.

When we execute the `kubectl get secret <secret-name> -o yaml` command, we can see the hash value. 

So, how do we decode these hashed values? When we run the following commands, we can get the actual passwords.

```bash
% echo -n 'bXlzcWw=' | base64 --decode
mysql%                                                                        

% echo -n 'cGFzd3Jk' | base64 --decode
paswrd%
```

###### Secrets in Pods

Below is a simple example pod definition file for the web application. 

```properties
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-colur
  labels:
    name: simple webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
```

To inject an environment variable, add a new property to the container called `envFrom`. The envFrom property is a list so we can pass as many environment variables as required.

Each item in the list corresponds to a secret item in the list. We can specify the name of the secret we created earlier.

```properties
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
    envFrom:
      - secretRef:
          name: app-secret
```

The name of the secretRef should be the same as the name of the secret we created before. And now we just need to create a pod with the above definition file.

> kubectl create -f pod-definition.yaml

What we just saw was injection secrets as the environment variables into the pods. There is another way to inject secrets into pods. 

We can inject a single environment variable;

```properties
env:
  - name: DB_Password
    valueFrom:
      secretKeyRef:
        name: app-secret
        key: DB_Passoword
```

Or we can inject the whole secret as files in a volume;

```properties
volumes:
  - name: app-secret-volume
    secret:
      secretName: app-secret
```

If you were to mount the secret as a volume in the pod, each attribute in the secret is created as a file with the value of the secret as it's content.

```bash
% ls /opt/app-secret-volumes
DB_Host       DB_Password         DB_User
```

If we look at the contents of the DB_Password file we see the password in it.

```bash
% cat /opt/app-secret-volumes/DB_Password
paswrd
```

Secrets encode data in base64 format. 
Anyone with the base64 encoded secret can easily decode it. As such the secrets can be considered as not very safe.

The concept of safety of the Secrets is a bit confusing in Kubernetes. The <a href="https://kubernetes.io/docs/concepts/configuration/secret/" target="_blank">**kubernetes documentation**</a> page and a lot of blogs out there refer to secrets as a **safer option** to store sensitive data. 

They are safer than storing in plain text as they reduce the risk of accidentally exposing passwords and other sensitive data. 

It's not the secret itself that is safe, it is the practices around it. 

Secrets are not encrypted, so it is not safer in that sense. 
However, some best practices around using secrets make it safer. As in best practices like:

* Not checking-in secret object definition files to source code repositories.
* <a href="https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/" target="_blank">**Enabling Encryption at Rest**</a> for Secrets so they are stored encrypted in etcd. 

Also the way Kubernetes handles secrets. Such as:

* A secret is only sent to a node if a pod on that node requires it.
* Kubelet stores the secret into a tmpfs so that the secret is not written to disk storage.
* Once the pod that depends on the secret is deleted, kubelet will delete its local copy of the secret data as well.

Read about the <a href="https://kubernetes.io/docs/concepts/configuration/secret/#protections" target="_blank">**Protections**</a> and <a href="https://kubernetes.io/docs/concepts/configuration/secret/#risks" target="_blank">**Risks**</a>  of using secrets.

There are other better ways of handling sensitive data like passwords in Kubernetes, such as using tools like Helm Secrets, HashiCorp Vault.

## Security in Docker and Kubernetes

### Process Isolation

Unlike virtual machines, containers are not completely isolated from their host. Containers and the host of containers share the same kernel. Containers are isolated using Namespace in Linux. The host has a Namespace and the containers have their own Namespace. All the processors run by the containers are in fact run on the host itself, but in their own namespace. As far as the docker container is concerned, it is in its own namespace and it can see its own process only, It can not see anything outside of it or in any other namespace.

Suppose we are running the `docker run ubuntu sleep 3600` command in Docker, when we list the processes from within the docker container, we see the sleep process with a process id of one.

```bash
% ps aux
USER          PID  %CPU %MEM      VSZ    RSS   TT  STAT STARTED      TIME COMMAND
root          1     0.0  0.0      4708   724   ??  S    Mon09AM 156:58.82 sleep 3600
```

For the docker host, all processes of its own as well as those in the child Namespaces are visible as just another process in the system. So, when we list the processes on the host, we see a list of processes including the sleep command but with a different process id.

```bash
root               229   0.7  0.1  4397552  12204   ??  Ss   Mon09AM   1:24.24 /usr/libexec/trustd
root                 1   0.6  0.1  4360104  18148   ??  Ss   Mon09AM   9:24.69 /sbin/launchd
root               146   0.6  0.0  4378072   5128   ??  Ss   Mon09AM   0:26.44 /usr/libexec/diskarbitrationd
oktaytuncay      27808   0.5  2.3 34550456 389200   ??  S    12:58PM   1:12.97 /Applications/Google Chrome.app/Contents/Frameworks/
root              1754   0.0  0.0     4708    724   ??  S    Mon09AM 156:58.82 sleep 3600
```

This is because the processes can have different process ids in different namespaces and that's how docker isolates containers within the system.

### Users

The docker host has a set of users. By default docker processes run within containers as the root user. This can be seen in the output of the command we ran above. If you don't want to run with the root user, the `--user` parameter can be used.

> docker run --user=1017 ubuntu sleep 3600

Another way to enforce user security is to have this defined in the docker image itself at the time of creation. For example, we can use the default ubuntu image and set the user id to 1017 using the user instruction. Then we can build the custom image.

By default, docker runs a container with a limited set of capabilities. And so the process running within the container does not have the privileges to reboot the host or perform operations that can disturb the host or other containers running on the same host. 

If you wish to override this behavior and provide additional privileges than what is available, use the `cap-add` option in the docker run command.

> docker run --cap-add MAC_ADMIN ubuntu

Similarly, we can drop privileges as well using the `cap-drop` option, or in case we wish to run the container with all privileges enabled, then we can use the `privileged flag`

> docker run --cap-drop KILL ubuntu

> docker run --privileged ubuntu

### Security Contexts

When we run a docker container, we have the option to define a set of security standards such as `--user`, `--cap-add`, `--cap-drop`. These can be configured in Kubernetes as well. As we know already Kubernetes containers are encapsulated in pods. We may choose to configure the security settings at a container level or at a pod level.

If we configure it at a pod level, the settings will carry over to all the containers within the pod. If we configure it at both the pod and container, the settings on the container will override the settings on the pod.

The following pod runs an Ubuntu image with the sleep command. To configure the security context in the container, we can add a field called `securityContext` under the spec section.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: web-pod
spec:
  securityContext:
    runAsUser: 1000
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep","3600"]
```

To set the same configuration on the container level, we just need to move the whole section under the container specification as below.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: web-pod
spec:
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep","3600"]
      securityContext:
        runAsUser: 1000
        capabilities:
          add: ["MAC_ADMIN"]
```

To add capabilities use the capabilities option and specify a list of capabilities. Capabilities are only supported at the container level, not at the pod level.

### Service Account

There are two types of accounts in Kubernetes, a user account and a service account. A service account could be an account used by an application to interact with a Kubernetes cluster. For example, a monitoring application like Prometheus uses a service account to pull the Kubernetes API for performance metrics, and an automated built tool like Jenkins uses these service accounts to deploy applications on the Kubernetes cluster.

![pic12](images/12.png)

Let's say we developed a simple Kubernetes dashboard application called *Kubernetes Dashboard*, It's a simple built-in Python application and when deployed all it does is get the list of pods in the Kubernetes cluster by sending a request to Kubernetes. API and displayed on a web page.

In order for this application to query the Kubernetes API, it has to be authenticated. For that, we can use a service account.

**To create a service account:**

  > kubectl create serviceaccount <account_name>

**Example:**

  ```bash
  % kubectl create serviceaccount dashboard-sa
  serviceaccount/dashboard-sa created

  % kubectl get serviceaccount          
  NAME           SECRETS   AGE
  dashboard-sa   1         17s
  default        1         19d

  % kubectl describe serviceaccount dashboard-sa
  Name:                dashboard-sa
  Namespace:           dev
  Labels:              <none>
  Annotations:         <none>
  Image pull secrets:  <none>
  Mountable secrets:   dashboard-sa-token-bkhcx
  Tokens:              dashboard-sa-token-bkhcx
  Events:              <none>
  ```

When the service account is created it also automatically generates a token, the service account token is what should be used by the external application when authenticating to the Kubernetes API.

When a service account is created, it first creates the service account object and then generates a token for the service account. It then creates a secret object and stores that token inside the secret object.

The secret object is then linked to the service account. 

**To view the token viewed the secret object:**

```bash
% kubectl describe secret dashboard-sa-token-bkhcx
Name:         dashboard-sa-token-bkhcx
Namespace:    dev
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: dashboard-sa
              kubernetes.io/service-account.uid: 8e5d5143-cc54-488f-83f1-9d0429696b13

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1285 bytes
namespace:  3 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IndvMlZxVkkzYlY0ZkxhcEF3a1llRHJaSlpmQ0VoMHJXX3FBSmRZTkFndVEifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZXYiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlY3JldC5uYW1lIjoiZGFzaGJvYXJkLXNhLXRva2VuLWJraGN4Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImRhc2hib2FyZC1zYSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjhlNWQ1MTQzLWNjNTQtNDg4Zi04M2YxLTlkMDQyOTY5NmIxMyIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZXY6ZGFzaGJvYXJkLXNhIn0.MGsLrx-DpbtQzOunn4GciVt0tMApcVHaCnZuyau4T9JQaZvIRp1GnKUHH_Bd29H1dp_fCo8Qnw3MDp_U7-H3SATwxFcXdMFqcGygv6tlPzsLueKLt2Ac83bTtn3oNFlQoy3gBqTGQhrHiam6vObVgh3pNP0XPn2lElN7kszrDr-6WwoqcFwigdFFLn-9_HBIbaft1w620rmDQnlAXYq-nycOwjF7Vk__i9wGLfRKM7XccdhLhd21-w4mtYvnPqNvMGtcGryUkogecDj8bkkiTUyPl5GkWPBfsjsjpgZsflhC67H9twbtDUk5pkXGn06iWl-T82l8miruqtGtP1zw2w
```

This token can be used as an authentication token while making a risky call to the Kubernetes. 

For example, in the following example, using the curl command, we could provide a token as an authorization header while making a rest call to the Kubernetes API.

```bash
curl https://192.168.17.115/api -insecure --header "Authorization: Bearer eyJhbG..."
```

What if the 3rd party application is hosted on the Kubernetes cluster itself? we can have the Prometheus application deployed on the Kubernetes cluster itself. 

In that case, this whole process of exporting the service account token and configuring the third-party application to use it can be made simple port automatically mount the service token secret as a volume inside the pod.

That way, the token to access the Kubernetes API is already placed inside the pod and can be easily read by the application, we don't have to provide it manually.

When we look at the list of service accounts, we will see that there is a default service account that exists already for all namespaces in Kubernetes.

```bash
% kubectl get serviceaccount          
NAME           SECRETS   AGE
dashboard-sa   1         26m
default        1         19d
```

A service account named default is automatically created. Each namespace has its own default service account. Whenever a pod is created, the default service account and its token are automatically mounted to that pod as a volume mount.

For example, we have a pod named myapp-pod. We haven't specified any secrets or elements in the definition file.

```bash
% kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
myapp-pod   1/1     Running   0          19d
```

However, when the pod is created, If we look at the details of the pod by running `kubectl describe pod myapp-pod` command, we see that volume is automatically created from the secret named default token. which is in fact the secret containing the token for this default service account. 

```bash
% kubectl describe pod myapp-pod              
...
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-pnx9d (ro)
Conditions:
...
```

The secret token is mounted at location  `/var/run/secrets/kubernetes.io/serviceaccount` inside the pod.

From inside the pod, If we run the following command to list the contents of the directory, we'll see the secret mounted as three separate files.

```bash
% kubectl exec myapp-pod -- ls /var/run/secrets/kubernetes.io/serviceaccount 
ca.crt
namespace
token
```

If we view the contents of that file, we'll see the token to be used for accessing the Kubernetes API.

```bash
% kubectl exec myapp-pod -- cat /var/run/secrets/kubernetes.io/serviceaccount/token
eyJhbGciOiJSUzI1NiIsImtpZCI6IndvMlZxVkkzYlY0ZkxhcEF3a1llRHJaSlpmQ0VoMHJXX3FBSmRZTkFndVEifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZXYiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlY3JldC5uYW1lIjoiZGVmYXVsdC10b2tlbi1wbng5ZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiNDYzMDRjNDItN2VhMC00YWQ1LWFhYmYtYjc3N2MzNzdiNDkxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmRldjpkZWZhdWx0In0.qWqP5YQxEOSoDmJdtM32AtrPhnDW9AdPnEpd83rjAspNmXGOPe9rfolhfqxmY3RCsQza3wF12malZPoNBtLlH5Rb2x5cC20KfEZu_ysZeaKw7FiF0qU0YKQ0WL_r8Ws-fwxGLkFd4y_Pa5qum8Sv2FImmq1I57K4PwrsqmEs3sVJMT1i3lURbpbNNeBBumDZoLRcWkojipFgkRS0op04irI-ru3hXHMOv4MGOw7DF8pjPqpudIfcDVYJF8sD3-nTVImCCd_Vm14V-uBNqIiXcJ1JdwM6Ktmh4NO2Hb-NxbjsM0DtDz6oR6z2brl7RRf_PQsLMRtrSe7-Y8ls9HCJkw%
```

It is important to note that the default service account is very much restricted. It only has permission to run basic common API queries. 

If you'd like to use a different service account, we can modify the pod definition file, include a service account field, and specify the name of the new service account. 

```properties
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
    labels:
        app: myapp
        type: front-end
spec:
    containers:
        - name: nginx-container
          image: nginx
    serviceAccountName: dashboard-sa
```

We can not edit the service account of an existing pod. We must delete/re-create the pod. However, in the case of deployment, we'll be able to edit the service account as any changes to the pod definition file will automatically trigger a new rollout for the deployment.

So the deployment will take care of deleting and creating new pods with the right service account.

So keep in mind that Kubernetes automatically mounts the default service account if we haven't specified it explicitly. We may choose not to mount a service account automatically by setting the `automountServiceAccountToken`

```properties
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
    labels:
        app: myapp
        type: front-end
spec:
    containers:
        - name: nginx-container
          image: nginx
    automountServiceAccountToken: false
```

#### Resource Management

Whenever a pod is placed on a node, it consumes resources available to that node. The scheduler takes into consideration the number of resources required by a pod and available resources on the nodes.

If the node has no sufficient resources, the scheduler avoids placing the pod on that node, instead of placing the pod on one where sufficient resources are available.

If there are no sufficient resources available on any of the nodes, Kubernetes host back-scheduling the pods and we'll see the pod in a pending state.

By default, Kubernetes assumes that a pod or a container within a pod, requires 0.5 CPU and 256 Mb of memory. When the scheduler tries to place the pod on a node, It uses these numbers to identify a node that has a sufficient amount of resources available.

If we know that our application will need more than these numbers, we can modify numbers by specifying them in the pod or deployment definition file as below.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-colur
  labels:
    name: simple webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
    resources:
      requests:
        memory: "1G"
        cpu: 1
```

We can also specify a value as low as 0.1 CPU. 0.1 CPU can also be expressed as 100m. 100m CPU, 100 milliCPU, and 0.1 CPU are all the same. 1m is the lowest possible figure, it cannot be less than that.

1 count of CPU is equivalent to 0.5 OCPU, 1 AWS vCPU, 1 Azure Core, and 1 Hyperthread. 

We could request a higher number of CPUs for the container, provided our nodes are sufficiently funded.

In the docker world, the docker container has no limit on the resources it can consume on a node. Suppose a container starts with a CPU on a node and moment by moment it may demand more resources and consume resources reserved for other processes in the node.

However, we can set a limit for the resource usage on this pod by adding a limited section under the resource section in the definition file.

```properties
    resources:
      requests:
        memory: "1G"
        cpu: 1
      limits:
        memory: "2G"
        cpu: 2
```

When the pod is created, Kubernetes sets new limits for the container. The limits and requests are set for each container within the pod. 

A container can not use more CPU resources than its limit. However, this is not the case with memory. A container can use more memory resources than its limit. So if a pod tries to consume more memory than its limit constantly, the pod will be terminated.

Default numbers can also be changed. To do this, LimitRange must be created as follows. The configuration specifies a default memory request and a default memory limit.

```bash
kubectl create namespace default-mem-example
```

```properties
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512M
    defaultRequest:
      memory: 256M
    type: Container
```

```bash
kubectl apply -f memory-defaults.yaml --namespace=default-mem-example
```

Now if a Container is created in the default-mem-example namespace, and the Container does not specify its own values for memory request and memory limit, the Container is given a default memory request of 256 Mb and a default memory limit of 512 Mb.

#### Taints & Tolerations

Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes. One or more taints are applied to a node; this marks that the node should not accept any pods that do not tolerate the taints.

Taints and tolerations are used to set restrictions on what pods can be scheduled on that node. 

Let's start with the simple cluster with three worker nodes. When the pods are created, the Kubernetes scheduler tries to place these pods on the available worker nodes.

As of now, there are no restrictions or limitations and the scheduler places the pods across all of the nodes to balance them out equally.

![pic13](images/13.png)

So let's say we have dedicated resources on Node 1 for a particular use case or application. So, we would like only those pods that belong to this application to be placed on Node 1.

First, we prevent all pods from being placed on the node by placing a taint on the node. Let's call it blue.

So in this case none of the pods can be placed on Node 1, as none of them can tolerate the taint blue. This solves half of our requirements. No unwanted pods are going to be placed on this node. 

The other half is to ensure that certain pods are placed on this node. For this, we must specify which pods are tolerant of this particular taint. 

In our case, we want to allow only pod D to be placed on this node. Thus, we add toleration to pod D and pod D is tolerated to blue.

So when the scheduler tries to place this pod on Node 1, It goes through and Node 1 can only accept pods that can tolerate the taint blue.

![pic14](images/14.png)

So taints are set on nodes and tolerations are set on pods. So how does it look in the command line?

```bash
kubectl taint nodes node-name key=value:taint-effect
```

- To remove the taint on the node
  ```bash
  kubectl taint nodes node-name key=value:taint-effect-
  ```

If we would like to dedicate the node to pods in application blue, then the key-value pair would be app=blue.

The `taint-effect` defines what would happen to the pods if they do not tolerate the taint. There are three main effects.

- **NoSchedule :** The pods will not be scheduled in the node as described above
- **PreferNoSchedule :** The system will try to avoid placing the pod on the node but that is not guaranteed.
- **NoExecute :** New pods will not be scheduled on the node and existing pods on the node. If any will be evicted if they do not tolerate the taint. These pods may have been scheduled on the node before taint was applied to the node.

**Example :**
```bash
kubectl taint nodes node1 app=blue:Noschedule
```

Tolerations are added to pods. To add toleration to a pod, in the spec section of the pod definition file add a section called tolerations. Move the same values used while creating the taint under the section.

```properties
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
    labels:
        app: myapp
        type: front-end
spec:
    containers:
        - name: nginx-container
          image: nginx
    tolerations:
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"
```

When the pods are created or updated with the new tolerations they are either not scheduled on nodes or evicted from the existing nodes, depending on the effects set.

##### Taint - NoExecute

![pic15](images/15.png)

We had three nodes running some workloads. We do not have any taints or tolerations at this point. So they are scheduled this way.

After scheduling, we then decide to dedicate Node 1 for a special application and as such, we taint the node with the application name and add toleration to the pod that belongs to the application, which happens to be pod D in this case.

While tainting the node we set to taint effect to NoExecute. And as such once the taint on the node takes effect it evicts pod C from the node. Which simply means that the pod is killed.

The pod D continues to run on the node as it has toleration to the taint blue.

Remember taints and tolerations are only meant to restrict nodes from excepting certain pods. In this case Node 1 can only accept pod D but id does not guarantee that pod D will always be placed on Node 1 since there are no taints or restrictions applied on the other two nodes.

So remember taints and tolerations do not tell the pod to get to a particular node. Instead, it tells the node to only accept pods with certain tolerations.

If the requirement is to restrict a pod to certain nodes, this can be achieved with `node affinity`.

So far we have only been referring to the worker nodes. We also have master nodes in the cluster, which is technically just another node that has all the capabilities of hosting a pod plus it runs all the management software.

If you noticed, the scheduler does not schedule any pods on the master node. Why is that?

When the Kubernetes cluster is first set up, a taint is set on the master node. Automatically that prevents any pods from being scheduled on this node. 

We can see this and modify this behavior if needed. However, a best practice is not to deploy application workloads on a master node. 

- To see this taint:

  ```bash
  kubectl describe node kubemaster | grep Taint
  ```

#### Node Selectors

![pic16](images/16.png)

Let's say, we have a three-node cluster and two are smaller nodes with lower hardware resources and one of them is a larger node and configured with higher resources.

We have different kinds of workloads running in the cluster. For workloads that require higher horsepower, we want to forward it to the larger node in case the job requires extra resources.

However, in the current default setup, any pod can go to any nodes. So, if the pod is deployed to pod 2 or 3, it may not receive the resource it needs.

To avoid this, we can set a limitation on the pods so that they only run on particular nodes. There are two ways to do this.

The first is using node selectors, which is a simple and easier method. To limit the pod to run on the larger node, we can add a new property called `nodeSelector` to the spec section and specify the size as `Large`.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  container:
  - name: data-processor
    image: data-processor
  nodeSelector:
    size: Large
```

As we mentioned above, the `size: Large` key-value pairs are the labels assigned to the nodes. The scheduler uses these labels to match and identify the right node to place the pods on.

To use labels in a knows select like the one above, we need to have labeled our nodes before creating this pod. To label the node we can use the following command.

```bash
kubectl label nodes <node-name> <label-key>=<label-value>
```

```bash
kubectl label nodes Node-1 size=Large
```

After this point, When the pod is created it's placed on node-1 as desired.

nodeSelector has served this purpose, but it has some limitations. To achieve our goal here, it was enough to use a single label and selector. But what if our requirement is much more complex?

For instance, we should be able to say something like `place the pod on a large or medium node` or `place the pod on any nodes that are not small`.

We can not achieve this using nodeSelectors. For more complex requirements like this, affinity and anti-Affinity features can help us.

#### Node Affinity

The primary purpose of the node affinity feature is to ensure that pods are hosted on particular nodes.

We can not provide advanced expressions like `or`, `or not` with nodeSelector.

Node affinity feature provides us with advanced capabilities to limit pod placement on specific nodes.

With great power comes great complexity.

The same thing we do in nodeSelector is done in node affinity as follows.

- Placed the pod on the Large node.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  container:
  - name: data-processor
    image: data-processor

  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: ksize
            operator: In
            values:
            - Large
```

If we think our pod could be placed on a large or a medium node, we could simply add the value to the list of values below.

```properties
- matchExpressions:
  - key: size
    operator: In
    values:
      - Large
      - Medium
```

We can use the `not in` operator to say something like the following.

```properties
- matchExpressions:
  - key: size
    operator: NotIn
    values:
      - Small
```

The `exists` operator will simply check if the label size exists on the nodes and we don't need the values section for that as it does not compare the values.

```properties
- matchExpressions:
  - key: size
    operator: Exists
```

There are a number of other operators as well. You can refer to the [documentation](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) for more details.

What if no affinity could not match a node with a given expression?

The type of node affinity defines the behavior of the scheduler with respect to node affinity and the stages in the life cycle of the pod. There are currently two types of node affinity available.

- `requiredDuringSchedulingIgnoredDuringExecution`
- `preferredDuringSchedulingIgnoredDuringExecution`

There is also an additional type of node affinity planned as follows.

- `requiredDuringSchedulingRequiredDuringExecution`

There are two states in the life cycle of a pod when considering node affinity, `DuringScheduling` and `DuringExecution`.

**DuringScheduling :** is the state where a pod does not exist and is created for the first time. We have no doubt that when a pod is created, the affinity rules specified are considered to place the pod on the right node. 

What if the node with matching labels is not available? For example, we forgot to label that node as Large. That is where the type of node affinity used, comes into play.

| |DuringScheduling|
|-|----------------|
|Type 1|Required|
|Type 2|Preferred|

If you select the required type, which is the first row, the scheduler will mandate that the pod be placed on a node with the given affinity rules.

If it cannot find one, the pod will not be scheduled, this type will be used in cases where the placement of the pod is crucial. If a matching node does not exist, the pod will not be scheduled. 

But let's say the pod placement is less important than running the workload itself.

In that case, we could set it to preferred and in cases where a matching node is not found, the scheduler will simply ignore no definitive rules and place the pod on any available node.

This is a way of telling the scheduler, ***try the best to place the pod on the matching node, but if you really cannot find one, just place is anywhere.***

**DuringExecution :** is the state where a pod has been running and a change is made in the environment that affects node affinity, such as a change in the label of a node.

For example, an administrator removed the label we said earlier, `size=Large` from the node. What would happen to the pods that are running on the node?

| |DuringExecution|
|-|----------------|
|Type 1|Ignored|
|Type 2|Ignored|

As we can see above, there are two types of node affinities available today and this value is set to ignore it. 

This means that the pods will continue to run and any changes in node affinity will not affect them once they are scheduled.

### Observability

#### Understanding Health Probing

Even with the best-automated test coverage, it’s nearly impossible to find all bugs before deploying software to a production environment. That’s especially true for failure situations that only occur after operating the software for an extended period of time. It’s not uncommon to see memory leaks, deadlocks, infinite loops, and similar conditions crop up once the application has been put under load by end users.

Proper monitoring can help with identifying those issues; however, you still need to take an action to mitigate the situation. First of all, you’ll likely want to restart the application to prevent further outages. Second, the development team needs to identify the underlying root cause and fix the application’s code.

Kubernetes provides a concept called health probing to automate the detection and correction of such issues. You can configure a container to execute a periodic mini-process that checks for certain conditions.

#### Readiness Probes

A pod has a pod status and some conditions. The pod status tells us where the pod is in its lifecycle. When a pod is first created, it is in a pending state. This is when the scheduler tries to figure out where to place the pod.

If the scheduler can't find the node to place the pod, it remains in a pending state. To find out why it's stuck in a pending state, we can run `kubectl describe pod` command and it can tell us exactly why.

Once the pod is scheduled, it goes into a `ContainerCreating` status where the images required for the application are pulled and the container starts.

Once all the container in a pod starts, it goes into a `Running` state where it continues to be until the program completes successfully or is terminated.

Conditions define the pod's status. It is an array of true or false values that tell the state of the pod. When a pod is scheduled on a node, the `PodScheduled` condition is set to true. When the pod is `Initialized`, its value is set to true.

We know that a pod has multiple containers. When all the containers in the pod are ready, the `ContainersReady` condition is set to True and finally, the pod itself is considered to be `Ready`

- `PodScheduled` **<span style="color:green">True</span>** or <span style="color:red">False</span>
- `Initialized` **<span style="color:green">True</span>** or <span style="color:red">False</span>
- `ContainersReady` **<span style="color:green">True</span>**  or <span style="color:red">False</span>
- `Ready` **<span style="color:green">True</span>**  or <span style="color:red">False</span>

If we try to run an instance of a Jenkins server, we'll notice that it takes about 10 to 15 seconds for the server to initialize before a user can access the WebUI.

Even after the WebUI is initialized, it takes a few seconds for the server to warm up and be ready to serve users. During this wait period, if we look at the state of the pod, it continues to indicate that the pod is ready which is not true. 

So, how does Kubernetes know whether the application inside the container is actually running or not? And why does it matter if the state is reported correctly?

Let's look at a simple scenario where we create a pod and expose it to external users using a service. The service will route traffic to the pod immediately.

The service relies on the pods ready condition to route traffic. By default, Kubernetes assumes that as soon as the container is created, it is ready to serve user traffic. So, it sets the value of the ready condition for each container to True.

But if the application within the container took longer to get ready the service is unaware of it and sends traffic through as the container is already in a ready state causing users to hit a pod that is not yet running a live application.

What we need here is a way to tie the ready condition to the actual state of the application inside the container. As a developer of the application, we know better what it means for the application to be ready.

![pic17](images/17.png)

There are different ways that we can define if an application inside a container is actually ready. We can set up different kinds of HTTP tests to see if the API server responds.

In the case of a database, we may set to see if a particular TCP socket is listening or we may simply execute a command within the container to run a custom script that will exit successfully if the application is ready.

In the pod definition file, we can add a new field called `readinessProbe`, use the `httpGet` option and specify the port and the ready API.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-colur
  labels:
    name: simple webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
    readinessProbe:
      httpGet:
        path: /api/ready
        port: 8080
```

After this definition, when the container is created, Kubernetes does not immediately set the ready condition on the container to True.

Instead, it performs a test to see if the API responds positively. Until then the service does not forward any traffic to the pod as it sees that the pod is not ready.

There are different ways a probe can be configured.

|Usage|Option|
|-----|------|
|HTTP|httpGet with path and the port|
|TCP|tcpSocket with the port|
|Executing Command| exec with the command and options in an array format|

```properties
readinessProbe:
  httpGet:
    path: /api/ready
    port: 8080

readinessProbe:
  tcpSocket:
    port: 1521

readinessProbe:
  exec:
    command:
      - cat
      - /app/is_Ready
```

If we know that our application will take a minimum of say 10 seconds to warm up, we can add an additional delay to the probe. If we would like to specify how often to probe, we can do that using the period seconds option.

By default, if the application is not ready after three attempts, the probe will stop. If we would like to make more attempts, we can use the `failureThreshold` option.

```properties
readinessProbe:
  httpGet:
    path: /api/ready
    port: 8080

  initialDelaySeconds: 10

  periodSeconds: 5

  failureThreshold: 8
```

Finally, let us look at how readiness probes are useful in a multi-pod setup. Say we have a replica set or deployment with multiple pods and a service serving traffic to all the pods.

There are two pods already serving users say we want to add an additional pod and let's say the pod takes a minute to warm up.

Without the readiness probe configured correctly, the service would immediately start routing traffic to the new pod and that will result in service disruption to at least some of the users.

Instead, if the pods were configured with the correct readiness probe, the service will continue to serve traffic only to the older pods and wait until the new pod is ready.

Once ready, the traffic will be routed to the new pod as well, ensuring no users are affected.

#### Liveness Probes

Let's say we run an Nginx image on Docker and it starts to server users but for some reason, WebServer crashes, and the Nginx process and the container exits.

We noticed that when we run the `docker ps -a` command, the status is `Exit (1) 17 seconds ago`.

Since docker is not an orchestration engine, the container continues to stay dead and it will continue to deny requests until we manually create a new container.

When we run the same Web Application with Kubernetes, when the application crashes, Kubernetes tries to restart the container to restore the service to the users.

We can see the count of restarts increase in the output of `kubectl get pods` command.

However, what if the application is not really working but the container continues to stay alive? 

Say, for instance, due to a bug in the code, the application is stuck in an infinite loop. According to Kubernetes, the container runs and therefore assumes the application is up however, users who reach the container cannot use the service.

In that case, the container needs to be restarted or destroyed and a new container is to be brought up. That is where the liveness probe can help us.

A liveness probe can be configured on the container to periodically test whether the application within the container is actually healthy.

If the test fails, the container is considered unhealthy and is destroyed and created.

For a Web application, it could be when the API server is up and running, or for a database, we may test to see if a particular TCP socket is listening or we may simply execute a command to perform a test.

The liveness probe is configured in the definition file, as we did with the readiness probe. The difference is to user liveness instead of readiness.

```properties
livenessProbe:
  httpGet:
    path: /api/ready
    port: 8080
```

Similar to the readiness probe, we have the httpGet option for APIs, tcpSocket for port, and exec for commands and as well as additional options like `initialDelaySeconds` before the test is run, `periodSeconds` to define the frequency, and `failureThreshold`

#### Startup Probe

The purpose of a startup probe is to figure out when an application is fully started. Defining the probe is especially useful for an application that takes a long time to start up. The kubelet puts the readiness and liveness probes on hold while the startup probe is running. A startup probe finishes its operation under one of the following conditions:

1. If it could verify that the application has been started.
2. If the application doesn’t respond within the timeout period.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: startup-pod
spec:
  containers:
  - image: httpd:2.4.46
    name: http-server
    startupProbe:
      tcpSocket:
        port: 80
      initialDelaySeconds: 3
      periodSeconds: 15
```

As you can see in the following terminal output, the describe command can retrieve the configuration of a startup probe as well.

```bash
$ kubectl describe pod startup-pod
...
Containers:
  http-server:
     ...
     Startup:        tcp-socket :80 delay=3s timeout=1s period=15s \
                     #success=1 #failure=3
     ...
```

#### Container Logging

When we run `docker runcodes/event-simulator` in Docker, it generates a random event simulating a web server. These are events streamed to the standard output by the application. 

If we were to run the same command in a detached mode using `-d` option, we would not see the logs. If we wanted to view the logs, we can use the docker logs command followed by the container id. The `-f` option helps us see the live log trail.

> docker logs -f <cont_id>

We can create a pod with the same docker image using the pod definition file.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: event-simulator-pod
spec:
  containers:
  - name: event-simulator
    image: runcodes/event-simulator
```

Once the pod is running, we can view the logs using `kubectl logs -f event-simulator-pod`. `-f` option is for to stream the logs flow just like the Docker command.

```bash
% kubectl logs -f myapp-pod
/docker-entrypoint.sh: Configuration complete; ready for start up
2021/12/02 10:15:41 [notice] 1#1: using the "epoll" event method
2021/12/02 10:15:41 [notice] 1#1: nginx/1.21.4
2021/12/02 10:15:41 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2021/12/02 10:15:41 [notice] 1#1: OS: Linux 5.4.17-2102.205.7.3.el7uek.x86_64
2021/12/02 10:15:41 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2021/12/02 10:15:41 [notice] 1#1: start worker processes
2021/12/02 10:15:41 [notice] 1#1: start worker process 31
2021/12/02 10:15:41 [notice] 1#1: start worker process 32
2021/12/02 10:15:41 [notice] 1#1: start worker process 33
2021/12/02 10:15:41 [notice] 1#1: start worker process 34
```

Kubernetes pods can have multiple docker containers in them. In this case, we can modify the pod definition file to include an additional container. 

If we run kubectl logs command with pod name, which container's logs will be shown? 

If there are multiple containers within a pod, we must specify the name of the container explicitly in the command. Otherwise, It would fail to ask you to specify a name. In this case, we need to specify the name of the container that prints the relevant log messages.

```bash
kubectl logs -f myapp-pod nginx-container
```

This is how the simple logging functionality implemented with Kubernetes works

#### Monitor and Debug Applications

In Kubernetes, we can monitor performance metrics such as CPU, memory, network, and disk utilization, as well as node-level metrics such as the number of nodes in the cluster and how many are healthy.

Pod-level metrics like the number of pods and performance metrics like CPU and memory consumption on each pod.

So we need a solution that will monitor these metrics, store them and provide analytics around this data.

Kubernetes does not come with a full feature built-in monitoring solution. But there are a number of open source solutions available like Metrics Server, Prometheus, Elastic Stack, and Proprietary solutions like Datadog and Dynatrace.

Heapster was one of the original projects that enabled monitoring and analysis features for Kubernetes. A lot of references can be found for reference architectures for monitoring Kubernetes. However, heapster is now deprecated, and a slimmed-down version was formed known as the Metric Server.

We can have one metric server per Kubernetes cluster. The Metrics Server retrieves metrics from each of the Kubernetes nodes and pods, aggregates them, and stores them in memory. 

Note that the metrics server is only an in-memory monitoring solution and does not store the metrics on the disk. And as a result, we can't see historical performance data.

So how to generate metrics for pods in these nodes?

Kubernetes runs an agent on each node called kubelet which is responsible for receiving instructions from the Kubernetes API Master server and running pods on the nodes. 

The kubelet also contains a subcomponent known as the cAdvisor or Container Advisor. cAdvisor is responsible for retrieving performance metrics from pods and exposing them through the kubelet API to meet the metrics available for the metrics server.

If we are using minicube for the local cluster, the below command will enable the metric server.

> minikube plugins metric-enable server

For all other environments, the following command deploys the metric server by cloning the metric server deployment file from the GitHub repository.

```bash
% git clone https://github.com/kubernetes-sigs/metrics-server
```

Then deploying the required components using the kubectl apply command. This command deploys a set of pods, services, and roles to enable the metric server to poll for performance metrics from the nodes in the cluster.

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Verify that the metrics-server deployment is running the desired number of pods with the following command.

```bash
% kubectl get deployment metrics-server -n kube-system

NAME             READY   UP-TO-DATE   AVAILABLE   AGE
metrics-server   1/1     1            1           2m48s
```

A little while later, the metric server will collect and process data. After processing, cluster performance can be viewed by running the following command.

```bash
% kubectl top node

NAME          CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%   
10.0.10.214   42m          1%     2142Mi          6%        
10.0.10.25    27m          0%     2072Mi          6%        
10.0.10.252   46m          1%     2108Mi          6% 
```

The above command provides the CPU and memory consumption of each of the nodes.

The following command can be used to view performance metrics for pods in Kubernetes.

```bash
% kubectl top pod

NAME                                CPU(cores)   MEMORY(bytes)   
myapp-deployment-7df67f74c5-lnnzq   0m           4Mi             
myapp-deployment-7df67f74c5-ndb8n   0m           3Mi             
myapp-deployment-7df67f74c5-zdfgq   0m           3Mi             
myapp-pod
```

#### Labels, Selectors and Annotations

##### Labels and Selectors

Let's say we have a large number of different types and want to filter them by their class, type, or even multiple criteria.
Whatever that classification is, maybe we need the ability to group elements together and filter them according to our needs. And the best way to do that is with labels. And the best way to do that is with labels.

Labels are properties attached to each item. So we add properties to each item for their class, kind, and color. Selectors help us filter these items. For example, when we say `class=bird`, we get a list of birds. And when we say `color=green`, we get the green birds.

We can see labels and selectors used everywhere such as the keywords we tag on YouTube videos or blogs that help us filter and the right content.

So how are labels and selectors used in Kubernetes?
We can create a lot of different types of objects in Kubernetes including pods, services, replica sets, deployments, etc.

Over time we may end up having hundreds or thousands of these objects in our cluster. Then we'll need a way to filter and view different objects by different categories. For instance, group objects by their type, view objects by application, or by their functionality.

Regardless, we can group and select objects using labels and selectors. For each object, attach labels as per our needs like app, function, and more.

Then while selecting specify a condition to filter specific objects for example `app=Prod`.
So how exactly do we specify labels in Kubernetes? 
In a pod definition file, under metadata, we need to create a section called labels. Below that we can add labels in a key-value format like below.
 
```properties
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
  labels:
    app: Prod
    function: front-end
spec:
  containers:
  - name: simple-webapp
    image: simple-webapp
    ports:
      - containerPort: 8080
```

We can add as many labels as we like. 
Once the pod is created we can use the following command to select the labeled pod.

```bash
kubectl get pods --selector app=Prod
```

Kubernetes objects use labels and selectors internally to connect different objects together. For instance, to create a replica set of three different pods, we first label the pod definition and use a selector on a replica set to group the pods. In a replica set definition file, we see labels defined in two places.

```properties
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: simple-webapp
  labels: 
    app: Prod
    function: front-end
spec:
  replicas: 3
  selector:
    matchLabels:
      app: Prod <-----
  template:
    metadata:
      labels:
        app: Prod <-----
        function: front-end
    spec:
      containers:
      - name: simple-webapp
        image: simple-webapp
```

The labels on the ReplicaSet will be used if we were to configure some other object to discover the ReplicaSet.

In order to connect the ReplicaSet to the pod, we configure the selector field labels defined on the pod and make sure that these labels match the template section correctly.

However, if there may be other pods with the same label and different functions, we can specify both labels to ensure that the correct pods are discovered by ReplicaSet.

On creation, if the labels match the replicas that are created successfully. It works the same for other objects like a service.

When a service is created it uses the selector defined in the service definition file to match the labels set on the pods in the ReplicaSet definition file.

```properties
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector: 
    app: Prod <-----
  ports:
  - protocol: TCP
    port: 8080
    targetPort: 9376
```

```properties
    metadata:
      labels:
        app: Prod <-----
        function: front-end
```

##### Annotations

While labels and selectors are used to group and select objects, annotations are used to record other details for informatory purposes.

For example, tool details like name, version, build, information, etc. Or contact details like phone numbers, email, id, etc. In a word, annotations may be used for some kind of integration purpose.

```properties
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: simple-webapp
  labels: 
    app: Prod
    function: front-end
  annotations:
      buildversion: 1.17
```

## Rolling Updates & Rollbacks in Deployments

### Rollout and Versioning

When we first create a deployment, it triggers a rollout. A new rollout creates a new deployment revision, let's call it revision one.

In the future, when the application is upgraded, meaning when the container version is updated to a new one, a new rollout is triggered and a new deployment revision is created named revision to. This helps us keep track of the changes made to our deployment and enables us to roll back to a previous version of deployment if necessary.

We can see the status of our rollout by running the following command.

```bash
% kubectl rollout status deployment/myapp-deployment
```

We can see the revisions and history of our deployment by running the following command.

```bash
% kubectl rollout history deployment/myapp-deployment
deployment.apps/myapp-deployment 

REVISION  CHANGE-CAUSE
0         <none>
1         <none>
```

There are two types of deployment strategies. 

![pic18](images/18.png)

For example, we have five replicas of our Web application instance deployed. One way to upgrade these to a newer version is to destroy all of these and then create newer versions of application instances, meaning first destroy the five running instances and then deploy five new instances of the new application version.

The problem with this, as you can imagine, is that during the period after the older versions are down and before any newer version is up, the application is down and inaccessible to users.

This strategy is known as the `recreate` strategy and this is not the default deployment strategy.

![pic19](images/19.png)

The second strategy is that we did not destroy all of them at once. Instead, we take down the older version and bring up the newer version, one by one. This way, the application never goes down and the upgrade is seamless. The name of this strategy is the `Rolling Update` strategy.

If we don't specify a strategy while creating a deployment, it'll assume it to be a rolling update. In other words, the rolling update is the default deployment strategy.

So how is the upgrade process done?
After making the necessary changes in the definition file, we run the following command.

```bash
kubectl apply -f deployment-definition.yml
```

A new rollout is triggered and a new version of the deployment is created. But there is another way to do the same thing, We can update the image of our application by using the `set image` command as follows.

```bash
kubectl set image deployment/myapp-deployment \
  nginx=nginx:1.9.1
```

However, it is important to keep in mind that doing this will cause the deployment definition file to have a different configuration. So we must be careful when using the same definition file to make changes in the future.

### Upgrades

![pic20](images/20.png)

When a new deployment is created, it first creates a replica set automatically, which in turn creates the number of pods required to meet the number of replicas. When we upgrade our application, the Kubernetes deployment object creates a new replica set under the hood and starts deploying the containers there. at the same time taking down the pods in the old replica set following a rolling update strategy.

This can be seen when we try to list the replica sets using `kubectl get replicasets` command. 

### Rollback

Say, for instance, once we upgrade our application, we realize something wrong with the new version of the application. Wo we would like to rollback the application.

Kubernetes deployment allows us to rollback to the previous version. To undo a change, we can run the following command.

```bash
kubectl rollout undo deployment/myapp-deployment
```

The deployment will then destroy the pods in the new replica set and bring the older version up. And our application is back to its older format.

#### Rollout status and history

```bash
% kubectl create deployment nginx --image=nginx:1.16
deployment.apps/nginx created

% kubectl rollout status deployment nginx
deployment "nginx" successfully rolled out

% kubectl rollout history deployment nginx

deployment.apps/nginx 
REVISION  CHANGE-CAUSE
1         <none>
```

To check for revision 1, the first version of the deployment was created.

```bash
% kubectl rollout history deployment nginx --revision=1

deployment.apps/nginx with revision #1
Pod Template:
  Labels:	app=nginx
	pod-template-hash=6d4cf56db6
  Containers:
   nginx:
    Image:	nginx:1.16
    Port:	<none>
    Host Port:	<none>
    Environment:	<none>
    Mounts:	<none>
  Volumes:	<none>
```

When looking at the deployment history, there was no information on `CHANGE-CAUSE` field. We can use the `--record` flag to save the command used to create/update a deployment against the revision number.

```bash
% kubectl set image deployment nginx nginx=nginx:1.17 --record
deployment.apps/nginx image updated

% kubectl rollout history deployment nginx
deployment.apps/nginx 
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment nginx nginx=nginx:1.17 --record=true
```

We can now see that the change-cause is recorded for revision 2 of this deployment.

Let's make some more changes. In the example below, we are editing the deployment and changing the image from nginx:1.17 to the latest version of Nginx (nginx:latest) while making use of the `--record` flag.


```bash
% kubectl edit deployments nginx --record 
deployment.apps/nginx edited

% kubectl rollout history deployment nginx
deployment.apps/nginx 

REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment nginx nginx=nginx:1.17 --record=true
3         kubectl edit deployments nginx --record=true
```

To check for revision 3, the latest version of the deployment was created

```bash
% kubectl rollout history deployment --revision=3
deployment.apps/nginx with revision #3
Pod Template:
  Labels:	app=nginx
	pod-template-hash=bb957bbb5
  Annotations:	kubernetes.io/change-cause: kubectl edit deployments nginx --record=true
  Containers:
   nginx:
    Image:	nginx:latest
    Port:	<none>
    Host Port:	<none>
    Environment:	<none>
    Mounts:	<none>
  Volumes:	<none>
```

**To rollback to previous version:**

```bash
% kubectl rollout undo deployment nginx
deployment.apps/nginx rolled back

% kubectl rollout history deployment nginx       
deployment.apps/nginx 
REVISION  CHANGE-CAUSE
1         <none>
3         kubectl edit deployments nginx --record=true
4         kubectl set image deployment nginx nginx=nginx:1.17 --record=true


% kubectl rollout history deployment --revision=4

deployment.apps/nginx with revision #4
Pod Template:
  Labels:	app=nginx
	pod-template-hash=db749865c
  Annotations:	kubernetes.io/change-cause: kubectl set image deployment nginx nginx=nginx:1.17 --record=true
  Containers:
   nginx:
    Image:	nginx:1.17
    Port:	<none>
    Host Port:	<none>
    Environment:	<none>
    Mounts:	<none>
  Volumes:	<none>


% kubectl describe deployments nginx | grep -i image: 
    Image:        nginx:1.17
```

#### Wrap-Up

1. Check if anything exists in the cluster

```bash
% kubectl get all 
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   46d
```

There is nothing except for the default Kubernetes service.

2. Create a new deployment 

```properties
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels: 
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end    
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      type: front-end
```

```bash
% kubectl get all 
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   46d
```

```bash
% kubectl create -f deployment-definition.yml 
deployment.apps/myapp-deployment created
```

3. Check the status of the deployment.

```bash
% kubectl rollout status deployment myapp-deployment
deployment "myapp-deployment" successfully rolled out
```

All new deployments create a new rollout and a rollout is a process of deploying the containers in the backend. Whenever a new rollout is created, a new deployment revision is created.

4. Check the rollout history of the deployment

```bash
% kubectl rollout history deployment myapp-deployment

deployment.apps/myapp-deployment 
REVISION  CHANGE-CAUSE
1         <none>
```

5. Delete the deployment, recreate with the `--record` flag and check the history.

```bash
% kubectl delete deployment myapp-deployment

% kubectl create -f deployment-definition.yml --record

% kubectl rollout history deployment myapp-deployment

deployment.apps/myapp-deployment 
REVISION  CHANGE-CAUSE
1         kubectl create --filename=deployment-definition.yml --record=true
```

6. Change the Nginx version in the deployment-definition.yml file, apply the changes, check the image, check events and check the history.
  - **Before :** *image: nginx*
  - **After :** *image: nginx:1.20* 

```bash
% kubectl apply -f deployment-definition.yml --record
Warning: resource deployments/myapp-deployment is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
deployment.apps/myapp-deployment configured

% kubectl describe deployments myapp-deployment | grep Image
    Image:        nginx:1.20

% kubectl describe deployments myapp-deployment | tail -10
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  14m    deployment-controller  Scaled up replica set myapp-deployment-7df67f74c5 to 3
  Normal  ScalingReplicaSet  4m10s  deployment-controller  Scaled up replica set myapp-deployment-79896f8f68 to 1
  Normal  ScalingReplicaSet  4m3s   deployment-controller  Scaled down replica set myapp-deployment-7df67f74c5 to 2
  Normal  ScalingReplicaSet  4m3s   deployment-controller  Scaled up replica set myapp-deployment-79896f8f68 to 2
  Normal  ScalingReplicaSet  3m55s  deployment-controller  Scaled down replica set myapp-deployment-7df67f74c5 to 1
  Normal  ScalingReplicaSet  3m55s  deployment-controller  Scaled up replica set myapp-deployment-79896f8f68 to 3
  Normal  ScalingReplicaSet  3m47s  deployment-controller  Scaled down replica set myapp-deployment-7df67f74c5 to 0
```

If we look at the flow of the events above, we can see the events that have happened in the background.

We can see that there are two separate replicas mentioned here, one of which is older (in our case it's the opposite but we ignore it) and is scaled down one by one. And at the same time, a new version of replicas with the new version (nginx:1.20) we want is scaled-up one by one.

```bash
% kubectl rollout history deployment myapp-deployment
deployment.apps/myapp-deployment 
REVISION  CHANGE-CAUSE
1         kubectl create --filename=deployment-definition.yml --record=true
2         kubectl apply --filename=deployment-definition.yml --record=true
```

7. Change the image version with the `set image` command, upgrade the Nginx container to 1.20-perl which is another version of the Nginx container, check the history, and check the version of the image.

```bash
% kubectl set image deployment myapp-deployment nginx-container=nginx:1.20-perl --record
deployment.apps/myapp-deployment image updated

% kubectl rollout history deployment myapp-deployment
deployment.apps/myapp-deployment 
REVISION  CHANGE-CAUSE
1         kubectl create --filename=deployment-definition.yml --record=true
2         kubectl apply --filename=deployment-definition.yml --record=true
3         kubectl set image deployment myapp-deployment nginx-container=nginx:1.20-perl --record=true

% kubectl describe deployments | grep Image                            
    Image:        nginx:1.20-perl
```

Let's assume there is a problem with the last change. 

8. Rollback last change that we made, check the history and check the version of the image.

```bash
% kubectl rollout undo deployment myapp-deployment
deployment.apps/myapp-deployment rolled back

% kubectl rollout history deployment myapp-deployment
deployment.apps/myapp-deployment 
REVISION  CHANGE-CAUSE
1         kubectl create --filename=deployment-definition.yml --record=true
3         kubectl set image deployment myapp-deployment nginx-container=nginx:1.20-perl --record=true
4         kubectl create --filename=deployment-definition.yml --record=true

% kubectl describe deployments | grep Image
    Image:        nginx:1.20
```

We see a new revision with revision number 4 as seen above, and we can no longer see revision number 2, and revision 4 has the same content as revision 2. The last revision before this step was 3, we rollback 3, made 2 the current revision, and it became the current revision with number 4.

9. Change/break the Nginx version in the deployment-definition.yml file, apply the changes and check the rollout status.
  - **Before :** *image: nginx:1.20*
  - **After :** *image: nginx:invalid_version* 

```bash
% kubectl apply --filename=deployment-definition.yml --record=true
deployment.apps/myapp-deployment configured

% kubectl rollout status deployment myapp-deployment
Waiting for deployment "myapp-deployment" rollout to finish: 1 out of 3 new replicas have been updated...
```

When we check the status of rollout, we see that it's actually stuck at that point where it says one out of three new applications has been updated.

So we know that at this point in the backend, it's actually trying to bring down the old replicas and bring up new replicas. But it's a

When we check the number of pods in the deployment, we can see that the number of ready, up-to-date, and available pods, which should be 3, is 2.

```bash
% kubectl get deployment myapp-deployment
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
myapp-deployment   2/3     2            2           14h
```

But the recent change that we made is to update to an image that is not actually available, so we run the `kubectl get pods` to see the status pods, we'll notice that two of the old versions which are the actual working versions are running. The new version, which is not actually available, gives an error.

```bash
% kubectl get pods
NAME                                READY   STATUS             RESTARTS   AGE
myapp-deployment-794c669469-cvw7b   0/1     ImagePullBackOff   0          28m
myapp-deployment-794c669469-drhsj   0/1     ImagePullBackOff   0          16m
myapp-deployment-79896f8f68-859nw   1/1     Running            0          10h
myapp-deployment-79896f8f68-ql2rz   1/1     Running            0          10h
```

So what's happened here is, as always, this is considered to be an upgrade and the deployment object tried to terminate one instance from the old working versions, so thereby bringing the total number of running pods to two and it tried to create two pods of the new version. 

However, the new version is not right and that's why we can see that the status of the image is `ImagePullBackOff` which basically means that the deployment is unable to pull the image from the Docker hub. 

And because it couldn't deploy either of the two pods, Kubernetes stopped proactively continuing the upgrade and stopped terminating the old replica set because if it terminated the entire old replica set, the new image would be unhealthy so the user would be affected. So it proactively stops the upgrade and it's now waiting for the new image to be available.

We know that there is an error with the image that we are trying to download, so we are going to undo that recent change by running the following command.

```bash
% kubectl rollout undo deployment myapp-deployment
deployment.apps/myapp-deployment rolled back
```

If we check the status of the pods and the image, we can see that it has been fixed.

```bash
% kubectl describe deployment myapp-deployment | grep Image
    Image:        nginx:1.20

oktaytuncay@oktaytuncay-mac pod % kubectl get deployment myapp-deployment            
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
myapp-deployment   3/3     3            3           14h
```

## Jobs in Kubernetes

There are different types of workloads that a container can serve. For example, performing a computation, processing an image, performing some kind of analytics on a large data set, generating a report and sending email, etc. These are workloads that are meant to live for a short period of time.

Let's first see how such a workload works in docker and then relate the same concept to Kubernetes.

The following command can be used for a simple math operation to add two numbers.

> docker run ubuntu expr 3 + 2

The docker container comes up and performs the requested operation prints the output and exits. When we run the `docker ps` command. we see the container in and exited state. The return code of the operation performed is shown in the bracket as well. In this case, since the task was completed successfully the return code ise zero.

Let's just replicate the same with Kubernetes. We'll create a pod definition file to perform the same operation.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: math-pod
spec:
  containers:
    - name: math-pod
      image: ubuntu
      command: ['expr', '3', '+', '2']
```

When the pod is created, it runs the container performs the computation task and exists and the pod goes into a completed state. 

```bash
% kubectl get pods math-pod
NAME       READY   STATUS      RESTARTS   AGE
math-pod   0/1     Completed   5          3m22s
```

But it then recreates the container in an attempt to leave it running. Again the container performs the required competition task and exists and Kubernetes continues to bring it up again. And this continues to happen until the threshold is reached. So why does that happen?

Kubernetes wants our application to live forever. The default behavior of pods is to attempt to restart the container in an effort to keep it running. 

This behavior is defined by the property restart policy set on the pod which is by default set to always and that is why the pod always recreates the container when it exits. We can override this behavior by setting this property to never or on failure. That way Kubernetes does not restart the container once the job is finished.

We have new use cases for batch processing. We have large data sets that require multiple pods to process the data in parallel. We want to make sure that all pods perform the task assigned to them successfully and then exit. So we need a manager that can create as many pods as we want to get work done and ensure that work gets done successfully.

But we have learned about ReplicaSets helps us create multiple pods. While a ReplicaSet is used to make sure a specified number of pods are running at all times. A job is used to run a set of pods to perform a given task to completion. 

Now let's see how we can create a job. We create a job using a definition file and we started this with a pod definition file as above. To create a job using this, we can use a YAML file like the below. The template section is the same as the spec section in the pod-definition file we used above. We just added the `restartPolicy` parameter additionally.

```properties
apiVersion: batch/v1
kind: Job
metadata:
  name: math-add-job
spec:
  template:
    spec:
      containers:
        - name: math-pod
          image: ubuntu
          command: ['expr', '3', '+', '2']
      
      restartPolicy: Never
```

Once done, we can create the job using the following command.

> kubectl create -f job-definition.yaml 

Once created, we can use the following command to see the newly created job. We now see that the jobs were created and completed successfully.

```bash
% kubectl get jobs
NAME           COMPLETIONS   DURATION   AGE
math-add-job   1/1           9s         99s
```

We can check the status of the pod created with the following command and check the RESTARTS column to confirm that Kubernetes did not restart the pod.

```bash
% kubectl get pods           
NAME                                READY   STATUS      RESTARTS   AGE
math-add-job-8k2x7                  0/1     Completed   0          5m51s
```

But what about the output of the job? The output of a container can be viewed using the logs command.

```bash
% kubectl logs math-add-job-8k2x7
5
```

To delete the job and the pod inside, we can use the delete command.

```bash
% kubectl delete job math-add-job
job.batch "math-add-job" deleted
```

As an example of real-world scenarios, if the job was created to process an image, the processed image stored in a persistent volume would be the output, or if the job was to generate an email report, the email containing the report would be the result of the job.

We just run one instance of the pod in the previous example. To run multiple pods, we set a value for `completions` under the job specification and we set it to 3 to run 3 pods. 

```properties
apiVersion: batch/v1
kind: Job
metadata:
  name: math-add-job
spec:
  completions: 3

  template:
    spec:
      containers:
        - name: math-pod
          image: ubuntu
          command: ['expr', '3', '+', '2']
      
      restartPolicy: Never
```

This time when we created the job we see the completions count is 3 and the successful count is also 3. By default, the pods are created one after the other. The second pod is created only after the first is finished.


```bash
% kubectl get jobs
NAME           COMPLETIONS   DURATION   AGE
math-add-job   3/3           2m9s       2m59s

% kubectl get pods
NAME                                READY   STATUS      RESTARTS   AGE
math-add-job-2hv2s                  0/1     Completed   0          95s
math-add-job-hhln7                  0/1     Completed   0          2m38s
math-add-job-v88km                  0/1     Completed   0          2m41s
```

What if the pods failed? If we had a docker image that either successfully completed or got an error at random, and we were to create a job with that image. 

This job will be successful once and will get an error on the second attempt after it. So a third one is created and that completes successfully and the fourth one fails and so does the fifth and so we have 3 completions.

The job tries to create new pods until it has three successful completions and that completes the job.

Instead of getting the pods created sequentially, we can get them created in parallel. For this, we can add a property called `parallelism` to the job specification.

```properties
...
spec:
  completions: 3
  parallelism: 3
  template:
...
```

We set it to 3 to create three pods in parallel. So the job first creates three pods at once. If, as in the previous example, the desired number of healthy pods cannot be reached in the first three attempts, the steps of creating pods one by one will continue from here on, and this will continue until the desired number of 3 is reached.

### CronJobs

A CronJob is a job that can be scheduled just like crontab in Linux. Say for example we have a job that generates a report and sends an email.
The schedule option takes a cron-like format string where we can specify the time when the job is to be run.

![pic21](images/21.png)

In such a case, the definition file will look like the one below. 

```properties
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: reporting-cron-job
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      completions: 3
      parallelism: 3
      template:
        spec:
          containers:
            - name: math-pod
              image: ubuntu
      
          restartPolicy: Never
```

There are now 3 spec sections one for the cron-job, one for the job, and one for the pod. Once the file is ready we can run the create command below to create a cron-job.

```bash
% kubectl create -f cron-job-definition.yaml
cronjob.batch/reporting-cron-job created
```

```bash
% kubectl get cronjob
NAME                 SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
reporting-cron-job   */1 * * * *   False     1        6s              14s
```

To delete all pods created with cron-job.

```bash
% kubectl get pods -n default --no-headers=true | awk '/reporting-cron-job/{print $1}'| xargs  kubectl delete -n default pod
pod "reporting-cron-job-1639577640-78pt8" deleted
pod "reporting-cron-job-1639577640-lk68d" deleted
pod "reporting-cron-job-1639577640-vz977" deleted
pod "reporting-cron-job-1639577760-jrhgf" deleted
pod "reporting-cron-job-1639577760-ndrl8" deleted
pod "reporting-cron-job-1639577760-pk6jw" deleted
pod "reporting-cron-job-1639577820-7nxv2" deleted
pod "reporting-cron-job-1639577820-lntjp" deleted
...
```

## Services & Networking

Kubernetes services enable communication between various components within and outside of the application. Kubernetes services help us connect applications together with other applications or users. For example, our application may have a particular group of pods to serve the front-end and another group may have another group of pods to serve the back-end. And a third group connecting to an external data source.

![pic22](images/22.png)

It is services that enable connectivity between these groups of pods. Services enable the front-end application to be made available to end users. It helps communication between back-end and front-end pods and helps in establishing connectivity to an external data source.

Let's take a look at one use case of services. Let's assume we deployed our pod having a web application running on it. How do external users access the web page?

First of all, let's look at the existing setup. Kubernetes node has IP address 192.168.1.2 and my laptop is on the same network and its IP address is 192.168.1.10. The internal pod network is in the range of 10.244.0.0 and the pod has an IP 10.244.0.2. Clearly, I can not ping or access the pod at address 10.244.0.2 as it's in a separate network.

![pic23](images/23.png)

So what are the options to access the web page? First, if we were to ssh into the Kubernetes node at 192.168.1.2. From the node, we would be able to access the pods web page by executing a curl command or if the node has a UI, we would fire up a browser and see the web page in a browser following the address http://10.244.0.2. 

But this is from inside the Kubernetes node and that's not what I really want. I want to be able to access the web server from my own laptop without having to ssh into the node and simply by accessing the IP of the Kubernetes node.

So we need something in the middle to help us map requests to the node from our laptop through the node to the pod running the web container. This is where the Kubernetes service comes into play.

The Kubernetes service is an object just like pods, ReplicaSet or deployments that we work with before. One use case is to listen to a port on the node and forward a request on that port to a pod in the node.

This type of service is known as a node port service because the service listens to a port on the node and forward requests to the pods.

### Service Types

The first service type is NodePort where the service makes an internal pod accessible on a port on the node. 

The second is ClusterIP and in this case, the service creates a virtual IP inside the cluster to enable communication between different services such as a set of front-end servers to a set of back-end servers.

The third type is a load balancer which provisions a load balancer for our application in supported Cloud providers. A good example of that will be to distribute load across to different web servers in our front-end tier.

#### NodePort

As mentioned before, a service can help us by mapping a port on the node to a port on the pod. If we look closely at the service, we can see that there are three parts to it.

<p align="center">
  <img src="images/24.png" alt="drawing" width="400"/>
</p>

**TargetPort :** The port on the pod where the actual web server is running 80 and is referred to as the target port because that is where the service forwards the request to. 

**Port :** The second port is the port on the service itself. It is simply referred to as the port. Remember these naming are from the viewpoint of the service. The service is in fact like a virtual server inside the node. Inside the cluster, it has its own IP address, and that IP address is called the cluster IP of the service. 

**NodePort :** And finally, we have the port in the node itself which we use to access the webserver externally and that is known as the NodePort. As we can see it is set to 30008. That is because no ports can only be in a valid range which by default is from 30000 to 32767.

The simple definition file of the NodePort should also be as follows. In the spec section, we have type and ports that type refer to the type of service we are creating. It could be ClusterIP, NodePort, or Load Balancer. 

In the ports section, the only mandatory field is port. If we do not specify a targetPort, it is assumed to be the same as the port, and If we do not specify a NodePort, a free port in the valid range between 30000 and 32767 is automatically allocated. 

The ports section is an array, so notice the `-` below the port section, which indicates the first item in the array. So we can have multiple such port mappings within a single service.

The selector section, just like in a ReplicaSet and Deployment definition files, we provide a list of labels to define which pod it will connect to. For this, we need to look at the pod definition file used to create the pod. We need to pull the labels from the pod definition file and place them under the selector section.

```properties
apiVersion: v1
kind: Service
metadata:
  name: myapp-service

spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:
    app: myapp
    type: front-end
```

After this point, we can now create the service and check the status of the service.

```bash
% kubectl create -f service-definition.yml
service/myapp-service created
```

```bash
% kubectl get service
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP        56d
myapp-service   NodePort    10.96.116.85   <none>        80:30008/TCP   39s
```

```bash
curl http://192.168.1.2:30008
```

We can now use the port to access the web service using curl or a WebServer. 

So, what do we do when we have multiple pods? 

<p align="center">
  <img src="images/25.png" alt="drawing" width="300"/>
</p>

In a production environment, we may have multiple instances of our web application running for high availability and load balancing purposes. In this case, we have multiple similar pods running our web application.

They all have the same labels with a key app and set to a value of myapp. The same label is used as a selector during the creation of the service. So when the service is created, it looks for a matching pod with the label and finds three of them. The service then automatically selects all the three pods as endpoints to forward the external requests coming from the user.

We don't have to do any additional configuration to make this happen. It uses a random algorithm to balance the three different pods. Thus, the service acts as a built-in load balancer to distribute the load across different pods.

And finally, let's look at what happens when the pods are distributed across multiple nodes. 

<p align="center">
  <img src="images/26.png" alt="drawing" width="300"/>
</p>

In this case, we have the web application on pods on separate nodes in the cluster. When we create a service without us having to do any additional configuration, Kubernetes automatically creates a service that spans across all the nodes in the cluster and maps the target port to the same node port on all the nodes in the cluster. 

This way we can access the application using the IP of any node in the cluster. So we can access our application using the same port number 30008 as in our example. In short, the same port will be available to all nodes in the cluster.

In any case, whether it's a single pod on a single node, multiple pods on a single node, or multiple pods on multiple nodes, the service is created exactly the same without us having to take any additional steps during service creation.

When pods are removed or added, the service automatically updates, making it extremely flexible and adaptable.

#### Cluster IP

A full-stack web application typically has different kinds of pods hosting different parts of an application. We may have a number of pods running a front-end WebServer and another set of pods running a back-end server and a set of pods running a key-value store like Redis or another set of pods may be running a persistent database like MySQL.

<p align="center">
  <img src="images/27.png" alt="drawing" width="450"/>
</p>

Web front-end server needs to communicate with back-end servers and backend servers need to communicate with Redis services. So what is the way to establish connectivity between these services or tiers of my application?

As we can see above, the pods all have an IP address assigned to them. But these IPs are not static, these pods can go down at any time and new pods are created. So we can not rely on these IP addresses for internal communication between the application. 

Also what if the first front-end pod on 10.244.0.3 needs to be connected to a back-end service? Which of the three is it going for? And who makes this decision?

A Kubernetes service can help us group the pods together and provide a single interface to access the pods in a group. For example, a service created for the back-end pod will help group all of the back-end pods together and provide a single interface for other pods to access this service. The request is forwarded to one of the pods under the service randomly.

This enables us to easily and effectively deploy a microservices-based application on the Kubernetes cluster. Each layer can scale or move as required without impacting communication between the various services. Each service gets an IP and name assigned to it inside the cluster and that is the name that should be used by other pods to access the service.

This type of service is known as ClusterIP. To create such a service, we can use a definition file like the one below. ClusterIP is the default type so even If we didn't specify, It will automatically assume the type to be ClusterIP.

The targetPort is the port where the back-end is exposed which in this case is 80 and the port is where the service is exposed which is 80 as well.

To link the service to a set of pods we use a selector. We need to copy the labels defined in the pod definition file and move them under the selector.

```properties
apiVersion: v1
kind: Service
metadata: 
  name: back-end

spec:
  type: ClusterIP
  ports:
    - targetPort: 80
      port: 80

  selector:
    app: myapp
    type: back-end
```

```bash
% kubectl create -f cluster-service-definition.yml
service/back-end created
```

```bash
% kubectl get services back-end
NAME       TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
back-end   ClusterIP   10.96.16.84   <none>        80/TCP    21s
```

The service can be accessed by other pods using the Cluster-IP or the Service Name.

#### Recap

**ClusterIP:** Exposes the Service on a cluster-internal IP. Only reachable from within the cluster.

**NodePort:** Exposes the Service on each node’s IP address at a static port. Accessible from outside of the cluster.

A service with ClusterIP can be patched to NodePort online.

```bash
$ kubectl patch service nginx-service -p '{"spec":{"type":"NodePort"} }'
```

**Test:** 
```bash
% kubectl get services
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT
nginx-service   NodePort    10.96.124.64   <none>        9000:30597/TCP   8m28s
```

```bash
% kubectl get nodes
NAME          STATUS   ROLES   AGE     VERSION
10.0.10.167   Ready    node    3d15h   v1.22.5
10.0.10.44    Ready    node    3d15h   v1.22.5
10.0.10.73    Ready    node    3d15h   v1.22.5
```

```bash
$ wget --spider --timeout=1 10.0.10.167:30597
```

## Cluster Networking

### Ingress Networking

We're deploying an application on Kubernetes for a company that has an online store selling products. We build the application into a Docker image and deploy it on the Kubernetes cluster as a pod in a deployment.

Our application needs a database, so we deploy a MySQL database as a pod and create a service of the type ClusterIP called MySQL-service to make it accessible to our application.

The application can work with this setup and make the application accessible to the outside world, we create another service which is to type NodePort, and make our application available on a high port on the nodes in the cluster.

In this example, a port 38080 is allocated for the service. Users can access the application using a URL such as below, which represents any node IP and port 38080.  

- `http://<node-ip:38080>`

That setup works and users are able to access the application. Whenever traffic increases, we increase the number of replicas pods to handle the additional traffic and the service takes care of solving traffic between the pods.

However, if we are talking about a production-level application, we know that there is more to consider than just simply speeding up traffic between pods.

For example, we do not want the users to have to define an IP address every time. As in the example below, we can configure the DNS server to point to the IPs of the nodes.

- `http://my-online-store.com:38080`

If we also don't want our users to have to emember port number either. Then we need to bring in an additional layer between the DNS server and the cluster. This layer will work like a proxy server and proxies requests on port 80 to port 399080 on our nodes. With the help of this definition, users will now be able to access the application with an address like the one below.

- `http://my-online-store.com`

<p align="center">
  <img src="images/28.png" alt="drawing" width="350"/>
</p>

This is if the application is hosted On-Prem data center. What we could do If we were on a Public Cloud environment like Oracle Cloud Infrastructure (OCI). In that case, instead of creating a service of type NodePort for the application, we could set it to type Load Balancer. 

When we do that, Kubernetes will still do whatever it needs to do for a NodePort providing a high port for the service. But in addition to that, Kubernetes also sends a request to OCI to provision a network Load Balancer for the service.

On receiving the request, OCI would then automatically deploy a Load Balancer configured to route traffic to the service ports on all the nodes and return its information to Kubernetes. The Load Balancer has an external IP that can be provided to users to access the application. 

In this case, we said the DNS does point to this IP and users access the application using the URL `http://my-online-store.com`.

Company's business grows and we have new services for our customers. For example, a video streaming service and we want users to be able to access the new video streaming service by going to `http://my-online-store.com/watch`. We also would like to make our old application accessible at `http://my-online-store.com/wear`.

Let's say, our development team developed a new video streaming application as a completely different application as it has nothing to do with the existing one. However, in order to share the same cluster resources, we deploy the new application as a separate deployment within the same cluster. 

We create a service called video-service of type LoadBalancer. Kubernetes provisions port 38282 for this service and also provision a network LoadBalancer on the Cloud. The new Load Balancer has a new IP. 

So how do we route traffic between each of these Load Balancers based on the URL that the user typed? Do we need yet another proxy or Load Balancer that can redirect traffic based on URLs to the different services?

Every time we introduce a new service, we have to reconfigure the Load Balancer and finally, we also need to enable SSL for our application so users can access the application using HTTPS. 

Where do we configure that? It can be done at different levels, either at the application level or at the Load Balancer or proxy server level. We don't want the development team to implement it in their application as they would do it in different ways. We want it to be configured in one place with minimal maintenance.

That's a lot of different configurations and all of this becomes difficult to manage when our application scales. It requires involving different teams, we need to configure the firewall rules for each service and it's expensive as well as for each service, a new cloud-native Load Balancer needs to be provisioned.

All can be done with another Kubernetes definition file that lives with the rest of our application. 

<p align="center">
  <img src="images/29.png" alt="drawing" width="350"/>
</p>

That's where Ingress comes in, Ingress helps our users access the application using a single externally accessible secure URL that we can configure to redirect to different services within our cluster based on the URL path. 

Simply put, think of Ingress as a layer seven Load Balancer built-in to the Kubernetes cluster that can be configured using native Kubernetes primitives just like any other objects in Kubernetes.

Even with Ingress, we still need to expose it to make it accessible outside the cluster, so we still have to either publish it as a NodePort or with a Cloud Native Load Balancer. With this setup, we're going to perform all our load-balancing authentication, SSL, and URL-based routing configuration on the ingress controller.

So, how does it work? Without ingress, how would we do all of this? We would use a reverse proxy or a load balancing solution like Nginx, HAProxy, or traffic. We would deploy them on a Kubernetes cluster and configure them for road traffic to other services.

Ingress is implemented by Kubernetes, with any of the services we listed above (Nginx, HAProxy, or traffic), we need to deploy the solution and then set up a set of rules to configure Ingress. The solution we deploy is called an Ingress controller, and the set of rules we configure is called Ingress resources.

Ingress resources are created using definition files like the ones we use to create pods, deployments, and services.

A Kubernetes cluster does not come with an ingress controller by default. If we set up a cluster, we won't have an ingress controller built into it. So, if we simply create ingress resources and expect them to work, they won't. 

The question is, what are we deploying? There are a number of solutions available for Ingress such as Oracle Load Balancer which is Oracle's layer seven HTTP Load Balancer, GCE which is Google's layer seven HTTP Load Balancer, Nginx, Contour, HAPProxy, traffic, and Istio.

These ingress controllers are not just another Load Balancer or Nginx server. The Load Balancer components are just a part of it. The ingress controllers have additional intelligence built into them to monitor the Kubernetes cluster for new definitions or ingress resources and configure the Nginx server accordingly.

So we start with a deployment definition file named Nginx-ingress-controller with on replica and a simple pod definition template. We'll label it Nginx-ingress, and the image used is Nginx-ingress-controller with the right version.

1. Deployment Configuration File

```properties
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-ingress-controller
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nginx-ingress
  template:
    metadata:
      label:
        name: nginx-ingress
    spec:
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
      args:
        - /nginx-ingress-controller
```

This is a special build of Nginx built specifically to be used as an ingress controller in Kubernetes. So it has its own setup requirements, within the image the Nginx program is stored at location `/nginx-ingress-controller`, so we must pass that as the command to start the Nginx controller service.

Nginx has a number of configuration options such as path the store the logs, keep-alive threshold, SSL settings, session timeouts, etc. In order to decouple these configuration data from the Nginx controller image, we must create a ConfigMap object and pass that in as follows. 

2. ConfigMap Configuration File

```properties
kind: ConfigMap
apiVersion: v1
metadata:
  name: nginx-configuration
```

The ConfigMap needs to have any entries at this point, for this definition to be valid, the `args` section of the previous definition file must be as follows.

1. Deployment Configuration File Additional v1

```properties
    spec:
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
      args:
        - /nginx-ingress-controller
        - --configmap=$(POD_NAMESPACE)/nginx-configuration
```

Creating a ConfigMap makes it easy for us to modify configuration settings in the future. We'll just have to add it to the ConfigMap and not have to worry about modifying the Nginx configuration files.

We must also pass into environment variables that carry the pods, name, and namespace it is deployed to.

1. Deployment Configuration File Additional v2

```properties
    spec:
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
      args:
        - /nginx-ingress-controller
        - --configmap=$(POD_NAMESPACE)/nginx-configuration
      env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
```

The Nginx service requires these to read the configuration data from within the pod. And finally specify the ports used by the Ingress controller, which happen to be 80 and 443.

1. Deployment Configuration File Additional v3

```properties
    spec:
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
      args:
        - /nginx-ingress-controller
        - --configmap=$(POD_NAMESPACE)/nginx-configuration
      env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace

      ports:
        - name: http
          containerPort: 80
        - name: https
          containerPort: 443
```

We then need a service to expose the controller to the external users, so we create a service of the type NodePort with the Nginx ingress label selector tooling the service to the deployment.

3. Service Configuration File

```properties
apiVersion: v1
kind: Service
metadata: 
  name: nginx-ingress
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    name: http
  - port: 443
    targetPort: 443
    protocol: TCP
    name: https
  selector:
    name: nginx-ingress
```

As mentioned before, the ingress controllers have additional intelligence built into them to monitor the Kubernetes cluster for ingress resources and configure the underlying Nginx server when something has changed.

But for the ingress controller, to do this, it requires a service account with the right set of permissions. For that, we create a service account with the correct roles and role bindings as follows.

4. Auth Configuration File

```properties
apiVersion: v1
kind: ServiceAccount
metadata:
  name: nginx-ingress-serviceaccount
```

So to summarize, with a `Deployment` of Nginx ingress image and `Service` to expose it, a `ConfigMap` to feed Nginx configuration data, and a Service account (`Auth`) with the right permissions to access all of these objects. So, we should be ready with an Ingress controller in its simplest form.

The next part of creating ingress resources. Ingress resource is a set of rules and configurations applied to the Ingress controller. We can configure rules to say simply forward all incoming traffic to a single application or route traffic to different applications based on the URL.

So, if a user goes to `www.my-online-store.com/wear` then brought to one of the applications, or if the user visits the `www.my-online-store.com/watch`, then watch the video app, etc. 

Or we could route users based on the domain name itself. For example, the user can visit the wear application via the `wear.my-online-store.com` address or the watch application via the `watch.my-online-store.com` link.

Let us look at how to configure these and a bit more detail. The Ingress resource is created with a Kubernetes definition file. Let's say the name of the file is `ingress-wear.yaml`.

Under the spec we'll have a back-end. and this section defines where the traffic will be routed to. So, if it's a single back-end, then we don't really have any rules. We can simply specify the `serviceName` and `servicePort` of the back-end where service.

5. Ingress Resource Configuration File

```properties
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-wear
spec:
  backend:
    serviceName: wear-service
    servicePort: 80
```

```bash
% kubectl create -f ingress-wear.yaml
ingress.extensions/ingress-wear created
```

```bash
% kubectl get ingress
NAME           CLASS    HOSTS   ADDRESS   PORTS   AGE
ingress-wear   <none>   *                 80      70s
```

The new Ingress is now created and wrote all incoming traffic directly to the wear service. We use rules when we want to route traffic based on different conditions. 

**For example:**
- **Rule 1 :** `www.my-online-store.com`
- **Rule 2 :** `www.wear.my-online-store.com`
- **Rule 3 :** `www.watch.my-online-store.com`
- **Rule 4 :** `Everything Else`

Within each rule, we can handle different pods. For instance, according to rule 1, the `www.my-online-store.com/wear` path will route the traffic to the clothes app. The `www.my-online-store.com/watch` path will route the traffic to the video streaming app. And there will be a third path that routes anything other than the first two paths to the `404 not found`.

Similarly, the second rule handles all traffic from `www.wear.my-online-store.com`, we can also have a path definition within this rule to route traffic to different paths. For example, say we have different applications and services within the clothes section for shopping or returns or support. When a user goes to `www.wear.my-online-store.com/support`, the path will route the traffic to the support app.

By default, it will be the shopping page but if the user changes the URL, it will be different backend services.

A similar structure but different logic will apply to Rule 3 and finally, anything other than the listed will go to the 4th Rule that would simply show a `404 not found error page.

We have rules at the top for each host or domain and inside each rule, we will have different pods to route traffic accordingly.

Let's look at how we configure ingress resources in Kubernetes. We need to define a similar definition file as we defined for ingress-wear. But this time it's different, under the spec section, we need a set of rules.

Now our requirement here is to handle all traffic to `my-online-store.com` and find them by URL path. So we just need a single rule for this, since we are only handling traffic to a single domain name which is `my-online-store.com`.

In the rules section, we have an item that is an HTTP rule that specifies different paths. The paths section is an array of multiple items and these multiple items define a path for each URL. The backend specification will remain the same as in an ingress-wear example. It will have the serviceName and servicePort sections.

Similarly, we'll create a similar back-end entry to the second URL part for the watch-service to route all traffic coming in through the watch URL to the watch-service.

```properties
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-wear-watch
spec:
  rules:
    - http:
        paths:
        - path: /wear
          backend:
            serviceName: wear-service
            servicePort: 80
        - path: /watch
          backend:
            serviceName: watch-service
            servicePort: 80
```

```bash
% kubectl create -f ingress-wear-watch.yaml
ingress.extensions/ingress-wear-watch created
```

By running the describe command, we should see two back-in URLs under the rules and the back-end services are pointing to just as we created them. 

**Note :** `endpoints not found` errors are ignorable as backend services are not created.

```bash
% kubectl describe ingress ingress-wear-watch
Name:             ingress-wear-watch
Namespace:        default
Address:          
Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *           
              /wear    wear-service:80 (<error: endpoints "wear-service" not found>)
              /watch   watch-service:80 (<error: endpoints "watch-service" not found>)
Annotations:  <none>
Events:       <none>
```

If we look closely at the output command above, we can see that there is something about a `Default backend`. If a user tries to access a URL that does not match of these rules, then the user is directed to the service specified as the Default backend. In this case, it happens to be a service named default-http-backend. So we must remember to deploy such a service.

If a user visits the URL `my-online-store.com/listen` or `my-online-store.com/eat` and If we don't have an audio or food delivery service, we may want to show them a nice message instead of an error. We can do this by configuring a default backend service to display that message.

The third type of configuration is using domain names or hostnames. To make this definition, we can create a definition file similar to the ingress. Let's say we have two domains, we need to create two rules for each domain. To split the traffic by domain name, we use the host field.

The host field in each rule matches the value specified by the domain name used in the requested URL and routes traffic to the appropriate backend. 

In the previous case, we did not specify the host field. If we don't specify the host field, it will accept all traffic coming through this particular rule without matching the hostname.

In this case, the definition file should be as follows.

```properties
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-wear-watch
spec:
  rules:
  - host: wear.my-online-store.com
    http:
      paths:
      - backend:
          serviceName: wear-service
          servicePort: 80
  - host: watch.my-online-store.com
    http:
      paths:
      - backend:
          serviceName: watch-service
          servicePort: 80
```

We can also create an Ingress resource from the imperative way like below.

```bash
kubectl create ingress <ingress-name> --rule="host/path=service:port"
```

- **Example:**

  ```bash
  kubectl create ingress ingress-test --rule="wear.my-online-store.com/wear*=wear-service:80"
  ```

#### Rewrite-Target option

The watch app displays the video streaming webpage at 
`http://<watch-service>:<port>/`

The wear app displays the apparel webpage at 
`http://<wear-service>:<port>/`

We must configure Ingress to access the below URLs. When a user visits the URL on the left, her/his request should be forwarded internally to the actual URL on the right. 

|URL for User|Actual URL|
|------------|----------|
|`http://<ingress-service>:<ingress-port>/watch`|`http://<watch-service>:<port>/`|
|`http://<ingress-service>:<ingress-port>/wear`|`http://<wear-service>:<port>/`|

Without the rewrite-target option, this is what would happen

|URL for User|Actual URL|
|------------|----------|
|`http://<ingress-service>:<ingress-port>/watch`|`http://<watch-service>:<port>/watch`|
|`http://<ingress-service>:<ingress-port>/wear`|`http://<wear-service>:<port>/wear`|

To fix that we need to re-write the URL when the request is passed on to the watch or wear applications. We can achieve that with the `rewrite-target` option and this can be done as below.

```properties
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-ingress
  namespace: critical-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /pay
        backend:
          serviceName: pay-service
          servicePort: 8282
```

### Network Policies

Let's assume we have a web-server serving front-end to users, an application serving back-end APIs, and a database server.

<p align="center">
  <img src="images/30.png" alt="drawing" width="300"/>
</p>

The user sends in a request to the web server at port 80. The web server then sends a request to the API server at port 5000 in the back-end. The API server then fetches data from the database server at port 3306 and sends the data back to the user.

So there are two types of traffic here Ingress and Egress. For example, for a web server, the incoming traffic from the users is ingress traffic and the outgoing traffic requests to the app server are egress traffic.

If we list the rules required for it to work, we should have the required ingress rules as follows.

|Service  |Ingress Port|Egress Port|
|---------|------------|-----------|
|WebSerber|80          |5000       |
|API      |5000        |3306       |
|Database |3306        |No Egree   |

Let us now look at Network Security in Kubernetes. Let's assume we have a cluster with a set of nodes hosting a set of pods and services. Each node, pod, and service has an IP address.

One of the pre-requisites for networking in Kubernetes is, that whatever solution we implement, the pods should be able to communicate with each other without having to configure any additional settings like routes.

All pods are on a virtual private network that spans across the nodes in the Kubernetes cluster. And they can all by default reach each other using the IPs, pod names, or services configured for that purpose.

Kubernetes is configured by default with an `All Allow` rule that allows traffic from any pod to any other pod or services within the cluster.

Let us now bring back the previous simple architecture and see how it fits in Kubernetes. For each component in the application, we deploy a pod one for the front-end webserver, for the API server, and one for the database. By default, all these three layers can communicate with each other.

<p align="center">
  <img src="images/31.png" alt="drawing" width="250"/>
</p>

What if we do not want the front-end WebServer to be able to communicate with the database server directly? Say, for example, our security teams and audits require us to prevent that. That is where we would implement a Network Policy to allow traffic to the database server only from the API server.

A Network policy is another object in the Kubernetes namespace. We can link a network policy to one or more pods. We can define rules within the network policy. In this case, we would say `Allow Ingress Traffic From the API Pod on Port 3306`.

Once this policy is created, it blocks all other traffic to the pod and only allows traffic that matches the specified rule. So how does this definition in theory actually work?

We label the pod and use the same labels on the podSelector field in the network policy. 

- Database pod that should be labeled

  ```properties
  labels:
     role: db
  ```

- podSelector field

  ```properties
  podSelector:
    matchLabels:
     role: db
  ```

And then we build the rule. Under policy, types specify whether the rule is to allow ingress or egress traffic or both. In our case, we only want to allow ingress traffic to the database pod. 

We specify the ingress rule that allows traffic from the API pod and we also specify the API pod again using labels and selectors. And the port to allow traffic on which is 3306.

```properties
policyTypes:
- Ingress
ingress:
- from:
  - podSelector:
      matchLabels:
        name: api-pod
  ports:
  - protocol: TCP
    port: 3306
```

Let us put all that together.

```properties
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
    podSelector:
      matchLabels:
        role: db
    policyTypes:
    - Ingress
    ingress:
    - from:
      - podSelector:
          matchLabels:
            name: api-pod
      ports:
      - protocol: TCP
        port: 3306
```

```bash
% kubectl create -f policy-definition.yaml
networkpolicy.networking.k8s.io/db-policy created
```

```bash
% kubectl get NetworkPolicy
NAME        POD-SELECTOR   AGE
db-policy   role=db        76s
```

#### Developing Network Policies

In Kubernetes, once we allow incoming traffic, the response or reply to that traffic is allowed back automatically. So in the previous example, when we define an ingress rule for the database, we don't need to specify any other rule for the output to reach the API layer.

But If the database pod tries to make an API call to the API pod, then that would not be allowed because that is now egress traffic originating from the database pod and would require a specific egress rule to be defined.

Let's say we only want to allow the API pod in the prod namespace to reach the database pod. For this, we need to add a new selector called the namespace selector property along with the pod selector property. Under this, we need to use match labels again to provide a label set on the namespace.

```properties
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
    podSelector:
      matchLabels:
        role: db
    policyTypes:
    - Ingress
    ingress:
    - from:
      - podSelector:
          matchLabels:
            name: api-pod
        namespaceSelector:
          matchLabels:
            name: prod
      ports:
      - protocol: TCP
        port: 3306
```

Do that's what the namespace selector does, it helps in defining from what namespace traffic is allowed to reach the database pod. 

If we wanted the database pod to be accessed by all objects in the prod namespace, then we would have removed the `podSelector` part and it would have been `namespaceSelector` only.

```properties
    policyTypes:
    - Ingress
    ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            name: prod
      ports:
      - protocol: TCP
        port: 3306
```

Let's look at another use case, say we have a backup server outside of the Kubernetes cluster and we want to allow the server to connect to the database pod. The podSelector and namespaceSelector fields that we use to define the traffic will not work as they will not be part of the cluster.

But we know the IP address of the backup server, we can configure a network policy to allow traffic from specific IP addresses. 

For this, we will add a new type of definition known as `ipBlock` definition which allows us to specify a range of IP addresses from which we can allow traffic to reach the database partition.

```properties
    policyTypes:
    - Ingress
    ingress:
    - from:
      - podSelector:
          matchLabels:
            name: api-pod
        namespaceSelector:
          matchLabels:
            name: prod

      - ipBlock:
          cidr: 192.168.8.17/32
      ports:
      - protocol: TCP
        port: 3306
```

These separate selectors can be used in separately ad individual rules or together as part of a single rule. In the above example, we have two elements, podSelector, and ipBlock. So these are two rules. 

The first rule has the podSelector and the namespaceSelector together and the second rule has the ipBlock selector. This definition works like the following equation;

`(podSelector=api-pod and namespaceSelector=prod) or ipBlock=192.168.8.17/32`

What if we separate them by adding `-` before the namespaceSelector like below?

```properties
- podSelector:
    matchLabels:
      name: api-pod

- namespaceSelector:
    matchLabels:
      name: prod

- ipBlock:
    cidr: 192.168.8.17/32
```

Now they're three separate rules in total. So this would mean that traffic matching the first rule is allowed that is from any pod matching the label api-pod in any namespace and the second rule is allowed for any pod in prod namespace and the third rule is for IP block specification. In such a case it works like the following equation, and almost all traffic is allowed.

`podSelector=api-pod or namespaceSelector=prod or ipBlock=192.168.8.17/32`

Let's assume, that instead of the backup server initiating a backup, we have an agent on the database pod that pushes backup to the backup server. In that case, the traffic is originating from the database pod to an external backup server. For this, we need to have the egress role defined.

We first need to add egress to the policyTypes and then add a new egress section to define the properties of the policy. Instead of `from` in Ingress, we need to add the keyword `to`. Under to, we can use any of the selectors, such as a pod, namespace or an IP block selector.

And in this case, since the database server is external, we use ipBlock selector and provide the CIDR block for the server. The port to which the requests will be sent will be 80, so we will define the port as 80.
 
```properties
    policyTypes:
    - Ingress
    - Egress
    
    ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            name: prod
      ports:
      - protocol: TCP
        port: 3306

    egress:
    - to:
      - ipBlock:
          cidr: 192.168.8.17/32
      ports:
      - protocol: TCP
        ports: 80
```

## State Persistence

### Volumes

Docker containers are ephemeral in nature. They are called when needed to process data and are destroyed when finished. This is also the case for data inside the container. To persist the data processed by the containers, we attach a volume to the containers when they are created. The data processed by the container is now placed in this volume thereby retaining it permanently.

Even if the container is deleted, the data generated or processed by it remains.

Just like in Docker, pods created in Kubernetes are temporary in nature. When a pod is created and deleted to process data, the data processed by it will also be deleted as well. To avoid this, we add a volume to the pod. The data generated by the pod is now stored in the volume. And even after the pod is deleted, the data remains. 

Suppose we have a single node Kubernetes cluster. We create a simple pod that generates a random number between 1 and 100 and writes that number to a file at number.out. It then gets deleted along with a random number. To retain the number generated by the pod, we create a volume and a volume needs storage. In this example, we specify a path `/data` on the host.

This way any files created in the volume would be stored in the directory data on my node. Once the volume is created, to access if from a container we mount the volume to a directory inside the container. We use the volume mounts field in each container to mount the data volume to the directory ` /Users/oktaytuncay/.kube/kube_exp` within the container.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: random-number-generator
spec:
  containers:
  - image: alpine
    name: alpine
    command: ["/bin/sh","-c"]
    args: ["shuf -i 0-100 -n 1 >> /Users/oktaytuncay/.kube/kube_exp/number.out]
    volumeMounts:
    - mountPath: /opt
      name: data-volume

  volumes:
  - name: data-volume
    hostPath:
      path: /data
      type: Directory
```

The random number will now be written to `/Users/oktaytuncay/.kube/kube_exp` mount inside the container, which happens to be on the data volume, which is in fact the /data directory on the host. When the pod gets deleted the file with the random number still lives on the host.

Let's take a step back and look at the volume storage options. We just used the hostPath option to configure a directory on the host as storage space for the volume. This will work on a single node. However, it is not recommended for use in a multi-node cluster. This is because the pods would use the /data directory on all the nodes and expect all of them to be the same and have the same data. 

But because they are on different servers, they cannot access the same data unless we can configure some kind of external replicated cluster storage solution. Kubernetes supports several types of different storage solutions such as NFS, glusterFS, Flocker, FibreChannel, CephFS, ScaleIO, or public Cloud solutions like Oracle, AWS, Azure, or Google.

#### Persistent Volumes

It would be better to manage the storage layer more centrally rather than managing it with the pod definition file. We can configure it so that an administrator can create a large storage pool and then have users extract parts from it as needed. That is where persistent volumes can help us.

<p align="center">
  <img src="images/32.png" alt="drawing" width="350"/>
</p>

A persistent volume is a cluster-wide pool of storage volumes configured by an administrator to be used by users deploying applications on the cluster. The users can now select storage from the pool using persistent volume claims.

We can use a configuration file like the one below to create a persistent volume. accessModes defines how a volume should be mounted on the hosts. The supported values are `ReadOnlyMany`, `ReadWriteOnce` and `ReadWriteMany`.

##### Persistent Volumes on Oracle Cloud Infrastructure (OCI)

1. Create a File System via File `Storage/File Systems` path in OCI.

<p align="center">
  <img src="images/33.png" alt="drawing" width="350"/>
</p>

2. Create the `StorageClass` that references the mount target. OCID can be found in `File Storage/File Systems/File System Details`

<p align="center">
  <img src="images/34.png" alt="drawing" width="350"/>
</p>

```properties
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: oci-fss
provisioner: oracle.com/oci-fss
parameters:
  mntTargetId: ocid1.filesystem.oc1.eu_frankfurt_1.aaaaaaaaaaanzeu7mzzgcllqojxwiotfouwwm4tbnzvwm5lsoqwtcllbmqwtcaaa
```

```bash
% kubectl apply -f storageClass.yml
storageclass.storage.k8s.io/oci-fss created
```

```bash
% kubectl get storageclass oci-fss
NAME      PROVISIONER          RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
oci-fss   oracle.com/oci-fss   Delete          Immediate           false                  8m45s
```

3. Create a Persistent Volume (PV).
  - Server IP can be found in `File Storage/Mount Targets/Mount Target Details`

```properties
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-vol1
spec:
 storageClassName: oci-fss
 capacity:
  storage: 50Gi
 accessModes:
  - ReadWriteOnce
 mountOptions:
  - nosuid
 nfs:
  server: 10.0.20.170
  path: "/FileSystem-OKE"
```

```bash
% kubectl apply -f oke-pv.yaml
persistentvolume/pv-vol1 created
```

#### Persistent Volume Claims

Persistent Volumes and Persistent Volume Claims are two separate objects in the Kubernetes namespace. An administrator creates a set of persistent volumes and a user creates persistent volume claims to use the storage.

Once the persistent volume claims are created, Kubernetes attach the persistent volumes to claims based on the request and properties set on the volume. Every persistent volume claim is attached to a single persistent volume. 

During the mounting process, Kubernetes tries to find a persistent volume that has sufficient capacity as requested by the claim. And any other request properties such as Access Modes, Volume Modes, Storage Class, etc...

However, if there are multiple possible matches for a single claim and we would like to specifically use a particular volume, we could still use labels and selectors to attach to the right volumes.

Finally, smaller volumes can be combined into a larger volume if all criteria match and there is no better option. There is a one-to-one relationship between claims and volumes, so no other claims can utilize the remaining capacity in the volume.

If there are no volumes available, the persistent volume request remains on hold until newer volumes are available to the cluster. Once newer volumes are available the claim will automatically be attached to the newly available volume.

If we want to create a Persistent Volume Claim, the YAML file should be as follows.

```properties
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce

  resources:
    requests:
      storage: 500Mi
```

Kubernetes looks at the volume created previously. The accessModes match, the capacity requested is 500 MB, but the volume is configured with 50 GB of storage. Since there are no other volumes available, the persistent volume claim is bound to the persistent volume.

```bash
% kubectl create -f pvc-definition.yaml
persistentvolumeclaim/myclaim created
```

When we run the `get persistentvolumeclaim` command, we see the claim is bound to the persistent volume we created.

```bash
% kubectl get persistentvolumeclaim
NAME      STATUS   VOLUME            CAPACITY   ACCESS MODES   STORAGECLASS   AGE
myclaim   Bound    ocid1.volume...   50Gi       RWO            oci            15s
```

To delete a PVC, we need to run `kubectl delete persistentvolumeclaim myclaim`. But what happens to the underlying persistent volume when the claim is deleted? We can choose with `persistentVolumeReclaimPolicy` what is to happen to the volume. By default, it's set to retain, meaning the persistent volume will remain until it is manually deleted by the admin.

It will not be available for reuse by any other claims or it can be deleted automatically with `persistentVolumeReclaimPolicy: Delete` option. This way as soon as the claim is deleted, the volume will be deleted as well. Thus freeing up storage on the device.

A third option is recycling. In this case, the data in the data volume will be cleaned before being made available to other claims.

### Stateful Sets

Before mentioning Stateful Sets, we should understand why we need them. Why can we just live with deployments?

<p align="center">
  <img src="images/37.png" alt="drawing" width="500"/>
</p>

Assume we are deploying a MySQL database with high availability solution. There are different technologies that we can use to have a highly available MySQL database. 

The most straightforward one is a single master and multi slave topology where all writes come into the master server and read can be served by either the master or any of the slave servers.

So the master server should be set up first before deploying the slaves. Once the slaves are deployed, we can perform a clone operation from the master server to the first slave.

After the initial copy is completed, replication from the master to that slave can start and, on the slave node will always be in sync with the database on the master. After that, we can set up the second slave.

We could do it the same way where we clone data from the master, but every time we do that, it's going to impact the resources on the master, especially the network interface. 

Since we have a copy of the Master on the first slave, it makes more sense to copy the data from the slave rather than the Master. So we wait for slave one to be ready and then clone data from slave one to slave to.

And finally, we enable continuous replication on slave two from the master. Note that both slaves are configured with the address of the Master node. That way, the slaves know where the master is. 

But if the address of the master or slave changes, the whole structure will be broken. So we definitely need a static hostname.

In this structure, first, the Master was created and then the slave-1 and slave-2 were created from the Master. We cannot guarantee that deployments in Kubernetes will be in this order. All pods of the deployment come up at the same time.

Stateful sets are similar to deployment sets. We can create pods based on a template, they can scale up and scale down, and they can perform rolling updates and rollbacks.

But there are some differences.

With stateful sets, pods are created in sequential order after the first pod is deployed, it must be in a running and ready state before the next pod is deployed.

So that helps us ensure that the master is deployed first and then slave-1 and then slave-2. Stateful sets assign a unique index. The first pod starts at zero and assigns a number increasing by one.

Each pod gets a unique name from this index, combined with the stateful set name. The first pod gets MySQL-0, the second gets MySQL-1, the third gets MySQL-2, etc. So no more random names.

If we'll scale up by deploying another pod, MySQL-3 for instance, then it would know that it can perform a clone from MySQL-2.

If the master node fails and the pod is recreated, it will still come up with the same name. Stateful sets maintain a sticky identity for each of their pods.

The master will always remain the master and will be available at the same address.

We might not always need a Stateful Sets, it really depends on the kind of application we're trying to deploy. We may consider deploying Stateful Sets with the following conditions.

- If the instances need to come up in a particular order
- If the instances need a stable name, etc..

We need to create a deployment definition file with a pod definition template inside it. Unlike the deployment definition file, the kind must be `StatefulSet` instead of deployment. 

Apart from that, a Stateful Set also requires a service name specified, we must specify the name of a `Headless service`. In the light of this information, our definition file should be as follows.

```properties
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
  labels:
    app: mysql

spec:
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql

  replicas: 3

  selector:
    matchLabels:
      app: mysql

  serviceName: mysql-h
```

The serviceName here is used to define the correct connection string for the pods. And I will explain this in more detail in the next section.

```bash
% kubectl create -f statefulset-definition.yml
statefulset.apps/mysql created
```

When we create a StatefulSet using the definition file, it creates pods one after the other. That's ordered graceful deployment. Each pod gets a stable, unique DNS record on the network that any other application can use to access a pod.

When we scale the StatefulSet, it scales in an ordered graceful fashion where each pod comes up, becomes ready and only then next pod comes up. This helps when we want to scale MySQL databases, as each new instance can clone from the previous instance.

```bash
% kubectl scale statefulset mysql --replicas=5
```

It works in the reverse order when we scale it down, the last instance is removed first, followed by the second last one.

```bash
% kubectl scale statefulset mysql --replicas=3
```

It's also same for termination. When we delete a StatefulSet, the pods are deleted in the reverse order.

```bash
% kubectl delete statefulset mysql
```

This is the default behavior of StatefulSet, but we can override this behavior to ensure that StatefulSet does not follow a sequential initialization. For this, we can set the `podManagementPolicy: Parallel` so that it deploys all pods in parallel. The default value of this field is ordered ready.

#### Headless Services

When we create a StatefulSet, it deploys one pod at a time and each pod has a stable, unique name, which is MySQL-0, MySQL-1, and MySQL-2, and so we can point the slaves to reach the master at MySQL-0.

However, there is something missing here, we redirect an application within the cluster to another application through services. So if we had a WebServer then we would need to create a service for the database to make the database server accessible to the Webserver.

The service acts as a load balancer. The traffic coming into the service is balanced across all the pods in the deployment. The service has a cluster IP and a DNS name associated it, which usually goes like `mysql.default.svc.cluster.local` 

So any other application within the environment, like a WebServer can use this DNS name to reach MySQL database.

<p align="center">
  <img src="images/38.png" alt="drawing" width="400"/>
</p>

Since this is a master-slave topology, the reads could be served by masters or slaves. But the writing must only be processed by the master. So, it's ok for the WebServer to read from the MySQL service we created but we can't write to that service as it's going to load balance the writes to all pods under the service. And that's not what we want for the MySQL cluster.

So we want to point the WebServer to the Master pod server only, but how do you do that? If we know the IP address of the Master pod, we can configure that in the WebServer. But IP addresses are dynamic and can change if the pod is recreated. So we can not use that.

What we need is a service that does not load balance requests, but gives us a DNS entry to reach each pod, and that's what a `headless service` is.

<p align="center">
  <img src="images/39.png" alt="drawing" width="400"/>
</p>

A headless service is created like a normal service, but it does not have an IP of its own and does not perform any load balancing. All it does is create a DNS entry for each pod using the pod name and a subdomain. When we create a headless service named MySQL-h, each pod gets a DNS record created in the form `pod_name.headless-service name.namespace.svc.cluster-domain`.

In our case, we can define the `mysql-0.mysql-h.default.svc.cluster.local` DNS entry for the Web application and thus using this connection string, it always makes the connection to the MySQL-0. The example definition file for that should be as follows.

```properties
apiVersion: v1
kind: Service
metadata:
  name: mysql-h # --> The name of the service
spec:
  ports:
    - port: 3306
  selector:
    app: mysql
  clusterIP: Node
```

When the headless service is created, the DNS entries are created for pods only if the two conditions are met while creating a pod. Under the spec section of a pod definition file, we have two optional fields, hostname, and subdomain.

We must specify the subdomain that will have the same value as the name of the service. When we do that, it creates a DNS record for the name of the service to point to the pod.

However, it still does not create a record for individual pods. For that, we must specify the hostname option in the definition file. Only then does it create a DNS record with a pod_name as well.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: mysql
spec:
  containers:
  - name: mysql
    image: mysql
  
  subdomain: mysql-h
  hostname: mysql-pod
```

When we deploy the pod as part of a deployment, if there is no hostname or subdomains specified, the deployment does not add a hostname or subdomain to the pod. So the headless service does not create a record for the pods.

If we specified the hostname or subdomain in the pod template section, then it assigns the same hostname and subdomain for all the pods because their deployment simply duplicates all the properties for the same pod.

And this is another way of showing that StatefulSet is different from Deployment, we don't need to specify a subdomain or hostname when creating StatefulSet. The StatefulSet automatically assigns the right hostname for each pod based on the pod name, and it automatically assigns the right subdomain based on the headless service name.

To define the correct headless server to the StatefulSet, we must specify the service name explicitly using the service name option
in the StatefulSet definition file.

That's how it knows what subdomain to assign to the pod, the StatefulSet takes the names that we specified and adds that as a subdomain property when the pod is created.

#### Storage in StatefulSets

<p align="center">
  <img src="images/40.png" alt="drawing" width="600"/>
</p>

As we did before, we created a persistent volume, then using that persistent volume, we created the persistent volume claim, and finally, we created a pod using this persistent volume claim. That's for a single persistent volume mapped to a single persistent volume claim to a single pod definition file.

<p align="center">
  <img src="images/41.png" alt="drawing" width="600"/>
</p>

With dynamic provisioning, we created StorageClass, we take out the manual creation of persistent volumes and use storage provisionals to automatically provision volume on Cloud providers which is Oracle in our case. With this solution, PV is created automatically. This works for a pod with a volume.

How does that change with Deployments or StatefulSets? With StatefulSets, when we specify the same PVC under the pod definition, all pods are created by that StatefulSets and try to use the same volume.

If we want separate volumes for each pod, as in the MySQL replication use case or any other replication solution, the pods should not share data. Instead, each pod should have its own storage layer. So each pod needs a PVC for itself.

A PVC is bound to a PV, so each PVC needs a PV. And of course, these PVs can be created from single or different storage. To automatically create a PVC for each pod in a StatefulSet, we can do that using a PersistentVolumeClaim template.

```properties
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
  labels:
    app: mysql

spec:
  replicas: 3
  selector:
    matchLabels:
      app: mysql

  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql
        volumeMounts:
        - mountPath: /var/lib/mysql
          name: data-volume

        volumes:
        - name: data-volume
          persistentVolumeClaim:
          claimName: data-volume

# After this section is taken from the PersistentVolumeClaim template.
  volumeClaimTemplates:
  - metadata:
      name: data-volume
    spec:
      accessModes:
        - ReadWriteOnce

    storageClassName: oci-fss
    resources:
      requests:
        storage: 500Mi
```

The `volumeClaimTemplates` is an array so we can specify multiple templates. 

We have a StatefulSet with `volumeClaimTemplates` and a `StorageClass` definition with the right provisioner for OCI. When the StatefulSet is created, 

1. It creates the first pod.
2. During the creation of the pod, a PVC is created which is associated with a StorageClass.
3. Then creates a PV and associate the PV with the volume.
4. Binds the PV to the PVC.
5. The same steps are performed for the other pods in the cluster.

If one of the pods in the cluster fails and is recreated or rescheduled onto a node, StatefulSet does not automatically delete the PVC or the associated volume to the pod. Rather, it allows the pod to reconnect to the same privacy to which it was added before. Thanks to this behavior, StatefulSet provides stable storage for pods.

**Example:**

1. Create a  PersistentVolumeClaim named 2-db-pvc.yaml

```properties
apiVersion: v1
kind: PersistentVolume
metadata:
  name: db-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /var/logs
```

```bash
% kubectl get pv
NAME    CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
db-pv   1Gi        RWO            Retain           Available                                   9s
```

2. Create a  PersistentVolumeClaim named 1-db-pv.yaml

```properties
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: db-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 512m
```

```bash
% kubectl get pvc
NAME     STATUS   VOLUME                                                                                         CAPACITY   ACCESS MODES   STORAGECLASS   AGE
db-pvc   Bound    ocid1.volume.oc1.eu-frankfurt-1.abtheljrimxdxjfhs5rgcsyzdf6jutmydybxznalykhr55yq7beegp3efmka   50Gi       RWO            oci            12s                             9s
```

The PersistentVolume has not been mounted by a Pod yet. Therefore, inspecting the details of the object shows <none>. Using the describe command is a good way to verify if the PersistentVolumeClaim was mounted properly:

```bash
% kubectl describe pvc db-pvc | grep Used
Used By:       <none>
```

3. Create a Pod named 3-app-consuming.yaml

**Note:** *claimName* should be the same as PersistentVolumeClaim name

```properties
apiVersion: v1
kind: Pod
metadata:
  name: app-consuming-pvc
spec:
  volumes:
    - name: app-storage
      persistentVolumeClaim:
        claimName: db-pvc
  containers:
  - image: alpine
    name: app
    command: ["/bin/sh"]
    args: ["-c", "while true; do sleep 60; done;"]
    volumeMounts:
      - mountPath: "/mnt/data"
        name: app-storage
```

The PersistentVolumeClaim now also shows the Pod that mounted it:

```bash
% kubectl describe pvc db-pvc | grep Used
Used By:       app-consuming-pvc
```
You can now go ahead and open an interactive shell to the Pod. Navigating to the mount path at /mnt/data gives you access to the underlying PersistentVolume:

```bash
% kubectl exec app-consuming-pvc -it -- /bin/sh
/ # cd /mnt/data
/mnt/data # ls
total 0
/mnt/data # touch test.db
/mnt/data # ls -ltr
total 0
-rw-r--r--    1 root     root             0 May 11 08:17 test.db
```

### Authentication, Authorization and Admission Control

`kube-apiserver` is at the center of all operations within Kubernetes. We interact with it via the `kubectl` utility or by accessing the API directly. And in this way, we can perform almost any operation on the cluster. So access to the API server itself is the first line of defense to control.

We need to answer two questions in here. Who can access the cluster and what can they do? Who can access the API server is defined by the `authentication` mechanisms. There are different ways that we can authenticate to the API server.

- Static Files - Username and Passwords
- Static Files - Username and Tokens
- Certificates
- External Authentication providers - LDAP
- Service Accounts for Machines

Once they gain access to the cluster, what can they do is defined authorization mechanisms, authorization is implemented using (RBAC) role-based access controls where users are associated with groups with specific permissions. In addition, there are other authorization modules like the (ABAC) attribute-based access control, Node Authorization, Webhook mode, etc...

All communication with the cluster between the various components, such as the etcd cluster, the kube controller manager, kube scheduler, kube API server, as well as those running on the worker nodes such as the kubelet and kube proxy is secured using TLS encryption.

#### Authentication

We can not create or view users in a Kubernetes cluster. However, Kubernetes can manage service accounts. We can create and manage service accounts using the Kubernetes API as below.

```bash
% kubectl create serviceaccount saccount1
serviceaccount/saccount1 created
```

```bash
% kubectl get serviceaccount       
NAME        SECRETS   AGE
default     1         6d12h
saccount1   1         36s
```

All user access is managed by the API server including through kubectl or API directly. The kube-apiserver authenticates the requests before processing it. There are different authentication mechanisms that can be configured. 

We can have a list of usernames and passwords in a static password file, or usernames and tokens in a static token file, or we can authenticate using certificates, or another option is to connect to the third-party authentication protocols like LDAP, Kerberos, etc...

We can create a list of users and their passwords in a CSV file and use that as the source for user information. The file has three columns; password, username, and user_id. 

```csv
password123, user1, u0001
password123, user1, u0001
password123, user1, u0001
password123, user1, u0001
password123, user1, u0001
```

Then as a `basic-auth-file` option, we can pass the filename to kube-apiserver as below. Click the <a href="https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/" target="_blank">link</a> for more details.

```bash
ExecStart=/usr/local/bin/kube-apiserver \\
 --advertise-address=${INTERNAL_IP} \\
 --allow-privileged=true \\
 --apiserver-count=3 \\
 --authorization-mode=Node,RBAC \\
 --bind-address=0.0.0.0 \\
 --enable-swagger-ui=true \\
 --enable-bootstrap-token-auth=true \\
 --etcd-servers=https://192.168.5.11:2379,https://192.168.5.12:2379 \\
 --event-ttl=1h \\
 --runtime-config=api/all \\
 --service-cluster-ip-range=10.96.0.0/24 \\
 --service-node-port-range=30000-32767 \\
 --v=2
 --basic-auth-file=user-details.csv
```

We must then restart the kube-apiserver for these options to take effect.

If we set up our cluster using the kubeadm tool, then we must modify the kube-apiserver pod definition file. The tool automatically restarts the kube-apiserver once we update the file. The kubeadm tool automatically The kube-apiserver is running as a Docker container on our master node and the location of the file is `/etc/kubernetes/manifests/kube-apiserver.yaml`/

```properties
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-apiserver
    - --authorization-mode=Node,RBAC
    - --advertise-address=152.70.191.125
    - --allow-privileged=true
    - --enable-admission-plugins=NodeRestriction
    - --enable-bootstrap-token-auth=true 
    - --basic-auth-file=user-details.csv
    image: k8s.gcr.io/kube-apiserver-amd64:v1.11.3
    name: kube-apiserver
```

To authenticate using the basic credentials while accessing the API server, we need to specify the username and password in a curl command as below.

```bash
curl -v -k https://152.70.191.125:6443/api/v1/pods -u "username:p"
```

In the CSV file with user details, we can add a fourth column with group details to assign users to specific groups.

```csv
password123, user1, u0001, group1
password123, user1, u0001, group1
password123, user1, u0001, group2
password123, user1, u0001, group2
password123, user1, u0001, group3
```

Similarly, instead of a static password file, we can have a static token file. Instead of a password, we specify a token. Pass the token file as an option `--token-auth-file=user-details.csv=user-details.csv` to the kube-apiserver.

While authenticating, we need to specify the token as an authorization bearer token as follows.

```bash
curl -v -k https://152.70.191.125:6443/api/v1/pods --header "Authorization: Bearer KfjsajandjaKDFLALDnsja38rnhc"
```

#### Security KubeConfig

It is much safer to use a certificate than to have the username and password written clearly. To do this we can use a similar command below.

```bash
curl -v -k https://152.70.191.125:6443/api/v1/pods \
  --key admin.key
  --cert admin.crt
  --cacert ca.crt
```

But we can specify the same information using the options `--server`, `--client-key`, `--client-certificate` and `--certificate-authority` with the kubectl utility.

```bash
kubectl get pods
  --server 152.70.191.125:6443
  --client-key admin.key
  --client-certificate admin.crt
  --certificate-authority ca.crt
```

Typing those in every time is a chore. To make our job easier, we can move this information to a configuration file named KubeConfig file as follows, and then we can specify this file with the `--kubeconfig` option in our command. 

```bash
--server 152.70.191.125:6443
--client-key admin.key
--client-certificate admin.crt
--certificate-authority ca.crt
```

Let's assume our config file name is `config`. Then the command should be as below. 

```bash
kubectl get pods --kubeconfig config
```

By default, the kubectl tool is in a file named config under the `$HOME/.kube/config` directory. So if we create the kube config file there, we don't have to specify the paths to the file explicitly in the kubectl command. That's the reason we haven't specified any options for our kubectl commands.

<p align="center">
  <img src="images/42.png" alt="drawing" width="500"/>
</p>

There are three sections in the config file. Clusters, users, and contexts. Clusters of the various Kubernetes clusters that we need access to. Say we have multiple clusters for dev, prod, and for one of the Cloud providers. 

Users are the user accounts with which we have access to these clusters. For example; admin user, dev user, or user for prod environment. 

Finally, context, marry these together. Context defines which user account will be used to access which cluster. For example, we could create a context named admin@production that will use the admin account to access a production cluster.

We're not creating any new users or configuring any kind of user access or authorization in the cluster with this process. We use existing users with their existing privileges and define which user to use to access which cluster.

This way we don't need to specify user certificates and server addresses in every kubectl command. So how does this fit in our example? 

The `server` specification in our command goes into the Clusters section with the name `MyKubeExample`. The `client-key`, `client-certificate` and, `certificate-authority` go into the Users section named MyKubeAdmin. Next, we create a context specifying to use the MyKubeAdmin user to access the MyKubeExample cluster named `MyKubeAdmin@MyKubeExample`.

Let's look at a real KubeConfig file. Each of these three sections is an array format in that we can specify multiple clusters, users, or contexts within the same file.

```properties
apiVersion: v1
kind: Config
clusters:
- name: MyKubeExample
  cluster:
    certificate-authority: /etc/kubernetes/pki/ca.crt
    server: https://MyKubeExample:6443

contexts:
- name: MyKubeAdmin@MyKubeExample
  context:
    cluster: MyKubeExample
    user: MyKubeAdmin

users:
- name: MyKubeAdmin
  user:
    client-certificate: /etc/kubernetes/pki/admin.crt
    client-key: admin.key
```

Kubernetes read the default path. So once the file is ready, we don't have to create any object.

```properties
apiVersion: v1
kind: Config

current-context: dev-user@oracle

clusters:
- name: MyKubeExample
- name: prod
- name: dev

contexts:
- name: MyKubeAdmin@MyKubeExample
- name: dev-user@oracle
- name: prod-user@prod

users:
- name: MyKubeAdmin
- name: admin
- name: dev-user
- name: prod-user
```

Kubernetes always use the context dev-user@oracle to access the Oracle clusters using the dev user's credential. 

There are command line options available within kubectl to view and modify the KubeConfig files. We can use the following command to view the current file.

```properties
% kubectl config view
apiVersion: v1
kind: Config
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://152.70.191.125:6443
  name: cluster-chduxhap4ea
contexts:
- context:
    cluster: cluster-chduxhap4ea
    user: user-chduxhap4ea
  name: context-chduxhap4ea
current-context: context-chduxhap4ea
users:
- name: user-chduxhap4ea
  user:
  ...
  ...
  ...
```

Alternatively, we can specify a KubeConfig file by passing the KubeConfig option in the command below.

```bash
% kubectl config view --kubeconfig=my-config
```

To make our config file the default, we can copy it to the default directory with the config name. 

- To change the current context
  ```bash 
  % kubectl config use-context prod-user@prod
  ```

Or we can use the following command instead of copying the file.

```bash
% kubectl config --kubeconfig=/root/my-kube-config use-context MyKubeExample
```
- To know the current context
  ```bash 
  % kubectl config --kubeconfig=/root/my-kube-config current-context
  ```

We can also make other changes in the file update or delete items in it. We can use `kubectl config -h` to see all possible operations.

The context section in the KubeConfig file can take an additional field called namespace where we can specify a particular namespace. This way, when we switch to that context, we'll automatically be in a specific namespace.

Instead of defining certificate-authority and specifying a path, we can also add the certificate-authority-data variable and we can paste the content of the certificate itself. If we already have crt file, we can fetch the content with the following command.

```bash
cat ca.crt | base64
```

Similarly, if you see a file with the certificate's data in the encoded format, we can use the following command to decode the certificate.

```bash
echo "AT17...hjR" | base --decode
```

#### API Groups

We can access the API server with the following command and can see the version of the cluster.

```bash
curl https://kube-master:6443/version
```

Similarly, we can get list of pods with API query.

```bash
curl https://kube-master:6443/api/v1/pods
```

APIs are categorized into two. The core group and the named group, the core group APIs (/api) is will all core functionality exists such as namespaces, pods, rc, events, nodes, pv, etc..

The named group APIs (/apis) are more organized and all the newer features are going to be made available through these named groups. It has groups under it apps, extensions, networking, storage, authentication, authorization, etc..

<p align="center">
  <img src="images/43.png" alt="drawing" width="500"/>
</p>

Within apps, we have deployments, replica sets, and stateful sets and within networking, we have network policies. So the ones at the top are API groups and the ones at the bottom are resources in those groups.

Each resource in this has a set of actions associated with them, things that we can do with these resources, such as list the deployments, get information about one of these deployments, create a deployment, delete a deployment, and update a deployment, watch deployment, etc.. These are known as Verbs.

If you get the Unauthorized message when running the curl command, as an alternative option, a kubectl proxy client can be started in a different session. 

The kubectl proxy command launches a proxy service locally on Port 8001 and uses credentials and certificates from our cube config file to access the cluster.

```bash
% kubectl proxy
Starting to serve on 127.0.0.1:8001
```

Now we can access the kubectl proxy servers at 8001, and the proxy will use the credentials from KubeConfig file to forward our request to the API server.

```curl
% curl http://localhost:8001 -k
{
  "paths": [
    "/.well-known/openid-configuration",
    "/api",
    "/api/v1",
    "/apis",
    "/apis/",
    "/apis/admissionregistration.k8s.io",
```

```curl
% curl http://localhost:8001/version -k
{
  "major": "1",
  "minor": "21",
  "gitVersion": "v1.21.5",
  "gitCommit": "3d40f84d0e5d471e102faa877bec487080a0375f",
  "gitTreeState": "clean",
  "buildDate": "2021-12-02T02:02:30Z",
  "goVersion": "go1.16.9 BoringCrypto",
  "compiler": "gc",
  "platform": "linux/amd64"
}
```

The `Kube Proxy` and `Kubectl Proxy` are not the same. Kube proxy is used to enable connectivity between pods and services across different nodes in the cluster.

Kubectl proxy is an HTTP proxy service created by Kubectl utility to access the Kube API server.

#### Authorization

There are different authorization mechanisms supported by Kubernetes, such as Node authorization, ABAC (Attribute Based Authorization), RBAC (Role Based Authorization), and Webhook.

**Node Authorization:** We know that the Kube API server is accessed by us for management purposes and kubelet on nodes within the cluster for management purposes within the cluster. Node Authorization is a special-purpose authorization mode that specifically authorizes API requests made by kubelets.

**ABAC:** Defines an access control paradigm whereby access rights are granted to users through the use of policies that combine attributes together.

For example, User Attribute-Based Authorization associates a user or a group of users with a set of permissions. Let's assume, the dev user can view, create and delete pods. We can do this by creating a policy file with a set of policies defined in a JSON format.

This way, we pass this file into the API server.

Similarly, we create a policy definition file for each user or group in this file. When we add, or delete security rules, we must edit the policy file manually and restart the Cube API server.

In short, attribute-based access control configurations are difficult to manage.

**RBAC:** Instead of directly associating a user or a group with a set of permissions, we define roles. For instance, we can create a role with the set of permissions required for developers and then we associate all the developers to that role.

Whenever a change needs to be made to the user's access, we simply modify the role and it reflects on all developers immediately.

**Webhook:** If we want to outsource all the authorization mechanisms, we can go with Webhook. 

Suppose we want to manage authorization externally rather than through built-in mechanisms like RBAC. For instance, `Open Policy Agent` is a third-party tool that helps with admission control and authorization.

We can have Kubernetes, make an API call to the open policy agent with the information about the user and her/his access requirements, and have the open policy agent decide if the user should be permitted or not.

**Always Allow and Always Denied:** Besides these four authorization mechanisms, `Always Allow` and `Always Denied`. As the name suggests, Always Allow, allows all requests without performing any authorization checks.

Always Deny is the opposite of that, denies all requests without performing any authorization checks.

The modes are set using the `--authorization-mode` option on the Kube API server, if we don't specify this option, it is set to `Always Allow` by default.

We can provide a comma-separated list of multiple mods we want to use `--authorization-mode=Node, RBAC, Webhook`. When you have multiple modes configured, the request is authorized using each one in the order it is specified.

For example, when a user sends a request, it's first handled by the Node Authorizer. The Node Authorizer handles only no requests.

So it denies the request. Whenever a module denies the request, it is forwarded to the next one in the chain.

The RBAC module performs its checks and grants the user permission, authorization is complete and the user is given access to the requested object. As soon as a module approves the request, no more checks are done and the user is granted permission.

##### Create a Role

Each rule has three sections apiGroups, resources, and verbs. The actions we want to give are defined by the verbs section. If we want to give another authorization to the developer group, we can add another rule to the same file.

```properties
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```

```bash
% kubectl create -f developer-role.yaml 
role.rbac.authorization.k8s.io/developer created
```

The next step is to link the user to that role. To do this we have to create another object called roll binding. The roll binding object links a user object to a role.

It has two sections. The subjects is where we specify the user details. The roleRef section is where we provide the details of the role we created. We should also note that roles and role bindings must be in the same namespaces. The namespace of RoleBinding determines where the permissions are granted.

```properties
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: developer-binding
subjects:
# You can specify more than one "subject"
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role #this must be Role or ClusterRole
  name: developer # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io
```

```bash
% kubectl create -f developer-binding.yaml 
rolebinding.rbac.authorization.k8s.io/developer-binding created
```

If we want to know if you have access to a particular resource in the cluster, we can take the benefit of the `can-i` command.

```bash
% kubectl auth can-i create deployments
yes
```

```bash
% kubectl auth can-i create pods 
yes
```

The user with admin privileges can run the same command for other users as follows.

```bash
% kubectl auth can-i create pods --as developers
no
```

```bash
% kubectl auth can-i create pods --as developers --namespace dev
yes
```

If we want to limit access within the namespace, we can do that too.

For example, we have five pods in the namespace and we want to give the user access to only some of these pods. We can restrict access by adding a resourceNames field to the rule.

```properties
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
  resourceNames: ["pod1", "pod2"]
```

##### Cluster Roles

A ClusterRole can be used to grant the same permissions as a Role. Used to authorize users to cluster-wide resources such as nodes, persistent volumes,  secrets and etc...

Cluster Roles are just like roles, except they are for cluster-scoped resources. For example, an admin role can be created to provide a cluster administrator permissions to view, create or delete nodes in a cluster. As seen below, resources are nodes.

```properties
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["get", "create", "list", "delete"]
```

The next step is to link the user to that cluster role. To do this we have to create another object called ClusterRoleBinding. The ClusterRoleBinding object links a user object to a role.

```properties
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-binding
subjects:
- kind: User
  name: admin # Name is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
```

#### Admission Controllers

<p align="center">
  <img src="images/44.png" alt="drawing" width="600"/>
</p>

We run commands from the command line using the kubectl utility to perform various operations on the Kubernetes cluster.

With the rules we can create with RBAC, we can define which user should have access to which objects or we can define what kind of API operations they should be authorized.

Let's say we want to create a pod without getting the image from a public Docker hub registry. Or maybe we don't want to use the latest image version. Or we do not permit running as a root user. Or we want the pod to always have the tag. Or you do not want to allow that request or allow certain capabilities only or to enforce that. or we just want to allow certain capabilities.

```properties
apiVersion: v1
kind: Pod
metadata:
  name: exp-pod
spec:
  containers:
    - name: postgres
--->  image: postgres:latest 
      command: ["sleep", "3600"]
      securityContext:
--->    runAsUSer: 0 --
        capabilities:
--->      add: ["ADMIN"]
```

So these are some of the things that we can't achieve with the RBAC and that is where admission controllers come in. Admission controllers, help us implement better security measures to enforce how a cluster is used.

Apart from simply validating configuration, admission controllers can do a lot more such as change the request itself or perform additional operations before the pod gets created.

What does each admission controller do?

**AlwaysPullImages:** Ensures that every time a pod is created, the images are always pulled. 

**DefaultStorageClass:** It monitors the creation of PVCs and automatically adds them to the default storage class if no storage class is specified.

**EventRateLimit:** Can help set a limit on the request would be the API server can handle at a time to prevent the API server from flooding with requests.

**NamespaceExists:** Rejects requests to namespaces that do not exist.

Let's assume, we want to create a pod in a namespace called dev that does not exist.

```bash
% kubectl run nginx --image nginx -n dev 
Error from server (NotFound): namespaces "dev" not found
```

Let's check the flow of the above request. 

The request first gets authenticated and then authorized, and it then goes through the admission controllers. The NamespaceExists admission controller handles the request and checks if the dev namespace is available if it is not the request is rejected.

There's another admission controller that is not enabled by default, and that is called the `NamespaceAutoProvision` admission controller. This admission controller automatically creates the namespace if it does not exist. 

To enable a non-default admissions controller or disable the default admissions controller, we need to add the following lines to the kube-apiserver.yaml file command section.

```bash
--enable-admissions-plugins=NamespaceAutoProvision
```

```bash
--disable-admissions-plugins=DefaultStorageClass
```

Note that the NamespaceAutoProvision and the NamespaceExists admissions controllers are deprecated and are replaced by the `NamespaceLifecycle` admission controller.

The `NamespaceLifecycle` admission controller will make sure that a request to a non-existent namespace is rejected and that the default namespaces such as default, kupe-system and kube-public cannot be deleted.

##### Validating and Mutating Admission Controller

The NamespaceAutoProvision and the NamespaceExists admission controllers are helping to validate if in namespace already exists and reject the request if it doesn't exist. This is known as a `Validating Admission Controller`.

Another admission controller DefaultStorageClass is enabled by default. This admission controller monitors the creation of PVCs and automatically adds them to the default storage class if no storage class is specified. This type of admission controller is known as a `Mutating Admission Controller.` It can change or mutate the object itself before it is created.

So those are two types of admission controllers. Mutating admission controllers are those that can change the request. Validating admission controllers are those that can validate the request and, allow or deny this.

These are all built-in admission controllers that are part of the Kubernetes source code and are compiled and shipped with Kubernetes.

If we want our own admissions controller with our own logic to support external admissions controllers, then we can talk about two special admissions controllers available `Mutating Admission Webhook` and then `Validating Admission Webhook`.

We can configure these Web hooks to point to a server that's hosted either within the Kubernetes cluster or outside it, and our server will have our own admission webhook service running with our own code and logic. 

After a request goes through all the built-in admission controllers, it hits the web host that's configured. And then once it hits the webhook, it makes a call to the admission webhook server by passing an admission review object in a JSON format.

This object has all the details about the request, such as the user that made the request and the type of operation the user is trying to perform and on what object and details about the object itself.

On receiving the request, the admission webhook server responds with an admission review object with a result of whether the request is allowed or not. If the allowed field in the response is set to true, then the request is allowed and if it's set the false, it is rejected.

The sample webhook configuration file should be as follows.

```properties
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  name: "pod-policy.example.com"
webhooks:
- name: "pod-policy.example.com"
  rules:
  - apiGroups:   [""]
    apiVersions: ["v1"]
    operations:  ["CREATE"]
    resources:   ["pods"]
    scope:       "Namespaced"
  clientConfig:
    service:
      namespace: "example-namespace"
      name: "example-service"
    caBundle: "Ci0tLS0tQk..."
```

### API Deprecations

<p align="center">
  <img src="images/45.png" alt="drawing" width="500"/>
</p>

A single API group can support multiple versions at a time. The rules in the API deprecation policy answer the following questions.

- Why do we need to support multiple versions? 
- How many should we support? 
- When can we remove an older version that is no longer required?

First, let's look at `Why`. 

<p align="center">
  <img src="images/46.png" alt="drawing" width="200"/>
</p>

For example, say that we are planning to contribute to the Kubernetes projects where we create an API group called `example.com`, under which we have two resources called sales and marketing. 

We developed and tested it in-house, we named it `v1alpha1` as it is the first alpha release of v1 and we are ready to merge it into the Kubernetes project. We can create a sales or marketing object using a YAML file and specify apiVersion like below.

```properties
apiVersion: example.com/v1alpha1
kind: sales
metadata:
  name: sales-kub
spec:
```

For instance, the marketing project didn't go well and we decided to remove this. In the next release, we `can not` remove it directly from the `v1alpha1` version.

That's where the first rule of API deprecation policy comes into play. 

<span style="color:red">*Rule 1: API elements may only be removed by incrementing the version of the API group.*</span>.

Meaning we can only remove the marketing element from the `v1alpha1` version of the API group. But It will continue to be present in the `v1alpha1` version of the element.

In other saying, the resource in a database is still at `v1alpha1`, but our API version has now changed to `v1alpha2`.

<p align="center">
  <img src="images/47.png" alt="drawing" width="400"/>
</p>

But, this is now going to be a problem. We will need to go back and change all API versions in our YAML files.

The users can use the same YAML files to create the resources, but internally it would be converted and stored as we want `v1alpha2` as below.

```properties
apiVersion: example.com/v1alpha2
kind: sales
metadata:
  name: sales-kub
spec:
```

So that brings us to the second rule of API deprecation policy. 

<span style="color:red">*Rule 2: API objects must be able to round-trip between API versions in a given release without information loss, with the exception of whole REST resources that do not exist in some versions.*</span>

If we create an API and object in the `v1alpha1` version and it is converted to `v1alpha2`, and then back to `v1alpha1`, it should be the same as the original `v1alpha1` version.

Let's say we fixed some bugs and are ready for beta version. 

Our first beta version `/v1beta1` is ready. And then after a few months, we release the next beta version of `/v1beta2`. And finally, we are releasing the stable version, which is the G.A (General availability) version, which we call `/v1`.

<p align="center">
  <img src="images/48.png" alt="drawing" width="500"/>
</p>

So that's kind of how an API evolves over time.

We don't have to have all the versions available at all times. We must deprecate and remove older versions as we release newer versions. Let's look at what are the rules and best practices around that.

For instance, `v1alpha1` is the first preferred/storage version, and let's call it `X`. With the Kubernetes release of `X+1`, we released the `v1alpha2` version of the API group. Since we are in the alpha phase, we are not required to keep the older `v1alpha1` version as part of this new release. 

And this is part of the rule for the Kubernetes deprecation policy.

<span style="color:red">*Rule 4a: Other than the most recent API versions in each track, older API versions must be supported after their announced deprecation for a duration of no less than:*</span>

<span style="color:red">- *GA: 12 months or 3 releases (whichever is longer)*</span>
<span style="color:red">- *Beta: 9 months or 3 releases (whichever is longer)*</span>
<span style="color:red">- *Alpha: 0 releases*</span>

Alpha versions need not be supported for any release. But Beta and G.A. versions once released must be supported anywhere from 9 to 12 months.

On `X+3`, we have `/v1beta2` release, from the beta version, it is necessary to have the previous beta version as well, in which case it should be `/v1beta1` as part of this version.

<p align="center">
  <img src="images/49.png" alt="drawing" width="300"/>
</p>

`/v1beta1` is now deprecated but not removed. It's still part of this release. It's still going to be there for a couple of releases before it is actually removed.

If you want to use the `/v1beta1`, it will display a deprecation warning.
Also note that at this time, `/v1beta1` is continuing to be the preferred version. So why is that?

Rule 4b is to answer this question.

<span style="color:red">*Rule 4b: The "preferred" API version and the storage version for a given group may not advance until after a release has been made that supports both the new version and the previous version.*</span>

So in this case, the current release is the first release where both new and previous version, that's `/v1beta1` and `/v1beta2` are supported. So we cannot change the preferred storage version yet. 

Instead, we have to wait until the next release.

<p align="center">
  <img src="images/50.png" alt="drawing" width="300"/>
</p>

The next release also has both versions, except `/v1beta2` becomes the preferred storage version.

On `X+5`, we have `/v1` stable version. But along with that, we also have `/v1beta1` and `/v1beta2` versions. `/v1beta2` is is still the preferred or storage version because this is the first release that has the `/v1` version. But both `/v1beta2` and `/v1beta2` are deprecated.

<p align="center">
  <img src="images/51.png" alt="drawing" width="400"/>
</p>

In the next release, we can remove the `/v1beta1` version because as per Rule 4a, the beta version needs to be supported for three releases. Beta one version was deprecated in release `X+3` and it's been around for `X+4` and `X+5`. So that's three releases and it can now be removed.

Suppose we are going to release the first version of `v2` with the next version and we have `/v2alpha1` version available for the first time.

In the past, when we had a new version available, we immediately deprecated the old version.

The `/v2alpha1` version is ready, should we deprecate the V1 version now? The answer is No, that's where `Rule3` comes in.

<span style="color:red">*Rule 3: An API version in a given track may not be deprecated until a new API version at least as stable is released.*</span>

Meaning G.A. versions can deprecate beta or alpha versions, but not the other way round and an alpha version can not deprecate a G.A. version.

Only the `v2` version can deprecate the `v1` version.

##### Kubectl Convert

- Old Version

  ```properties
  apiVersion: apps/v1beta1
  kind: Deployment
  metadata:
    name: nginx
  spec
  ```

- New Version

  ```properties
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: nginx
  spec
  ```

If we want to update the apiVersion of the application, we can use the `Kubectl convert` command as follows.

```bash
kubectl convert -f <old_file> --output-version <new-api>
```

```bash
kubectl convert -f nginx.yaml --output-version apps/v1
```

The `kubectl convert` command may not be available on the system by default. Since it is a separate plugin, it must be installed separately as in the <a href="https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/" target="_blank">Link</a>.


- Check all api-resources

  ```bash
  kubectl api-resources
  ```

### Custom Resource Definitions (CRD)

Let's say we want to create an application as a kind `FlightTicket`. But this will get an error as by default there is no kind with this name.

```properties
apiVersion: flights.com/v1
kind: FlightTicket
metadata:
  name: my-flight-ticket
spec:
  from: Istanbul
  to: London
  number: 2
```

We can't simply create any resource that we want without configuring it in Kubernetes API. First, we have to define what that resource is that we want to create.

For this, we need what is known as `Custom Resource Definition`(CRT). The CRT is a similar object with API version, type metadata, and specification and creation as follows.

The groups section must be the same as the apiVersion of the my-flight-ticket application type.

The kind section defined here under the names section and the kind section of the created application must be the same.

The singular name is just the flightticket, which is used to display the resource type in the output of the `kubectl` command. The plural is what is used by the Kubernetes is API resource. If we run the `kubectl api-resources` command, this is what will be shown.

We can also provide a short version of the name, let's say, ft. This way, we can just refer to the resource as ft when we run the kubectl commands.

In the versions section, we can clarify if this is the version servered or storage version of the application.

The schema defines all the parameters that can be specified under the spec section when we create the object. It defines what fields are supported and what type of value those fields support.

The schema uses `openAPIV3Schema` version, we can specify the different properties using an object type.

```properties
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: flighttickets.flights.com
spec:
  scope: Namespaced
  groups: flights.com
  names:
    kind: FlightTicket
    singular: flightticket
    plural: flighttickets
    shortnames:
      - ft
  versions:
    - name: v1
      served: true
      storage: true
  schema:
    openAPIV3Schema:
      type: object
      properties:
        spec:
          type: object
          properties:
            from:
              type: string
            to:
              type: string
            number:
              type: integer
```

```bash
kubectl create -f flightticket-custom-definition.yml
```

When we create the above-mentioned CRT then we can create the flightticket object as well.

```bash
kubectl create -f flightticket.yml
```

#### Custom Controllers

We created a CRT and then created the Flightticket object. What we need to do is to follow the status of the objects in etcd and perform operations such as flight ticket reservation, edit, or cancellation.

We can make such a requirement with Custom Controllers.

So a controller is any process or code that runs in a loop and is continuously monitoring the Kubernetes cluster and listening to events of specific objects being changed.

A custom controller can be developed by Python, but the preferred language for doing this is GO.

The sample controller can be found in the <a href="https://github.com/kubernetes/sample-controller" target="_blank">Link</a>.

As a prerequisite, GO must be installed on the computer where the development will be made.

```bash
% go version 
go version go1.17.6 darwin/amd64
```

```bash
% git clone https://github.com/kubernetes/sample-controller

% vi /Users/oktaytuncay/sample-controller/controller.go
```

`controller.go` file can be customized with our custom logic. When we build the code, we will run it as follows.

```bash
% ./sample-controller -kubeconfig=$HOME/.kube/config
```

### Deployment Strategies

Besides the recreate and rolling update strategy, there are two more deployment strategies. We can not specify these strategies as an option in the deployment, but they can be implemented in a different way.

#### Blue/Green Deployment Strategy

Blue/Green is a deployment strategy where we have the new version deployed alongside the old version.

The old version is called blue and the new version is called green and all traffic is sent to the old version in blue until all tests pass. After all, tests pass, all traffic is forwarded to the new version.

<p align="center">
  <img src="images/52.png" alt="drawing" width="600"/>
</p>

At first, we have the original version of our app deployed as a service and we will call it the blue deployment. Then we create a service to route the traffic.

Then we'll put a label on the pods to associate the service with the pods in the deployment and call it version: v1. We use the same label as the selector in the service.

We then deploy a second deployment and we'll call it the green deployment with a new version of the application. Once all tests pass, we change the label selector on the service and route the traffic to green deployment with the help of the service.

After this point, we just need to set the version:v2 tag to the pods in the new deployment. All that needs to be done for this is to change the label specified under the service selector to v2. And then the service will switch traffic to pods in the green deployment.

<p align="center">
  <img src="images/53.png" alt="drawing" width="450"/>
</p>

#### Canary Deployment Strategy

In this strategy, we deploy the new version again, but this time we only route a small percentage of traffic to it. The majority of traffic is being routed to the old version.

When the tests are passed and if everything looks good, then we can route all traffic to the new version of the application.

With Canary deployment, we want to achieve two things. 
- First, we want traffic to go to both versions at the same time. 
- But in doing so, we only want a small percentage of the traffic to go to the second version of the application.

First, let's see how we can traffic the traffic from a single service to two deployments at the same time. 

<p align="center">
  <img src="images/54.png" alt="drawing" width="450"/>
</p>

For this, we need to create a common label, which is shown in purple in in the above figure and we need to update the selector label in the service to match this common label.

That solves the first problem, we now have traffic going to both the versions of applications, but it's routing it equally.

If there are a total of 5 pods in both deployments, if we only have one pod in the canary deployment, we can ensure that only 20% of the traffic is routed to the application in the v2 version.

### Helm

When we design a complex architecture, many components need to be defined as follows.

Deployment for the pods, MySQL database servers, the Web Servers, a service to expose the Web Servers, the persistent volume to store the database, a persistent claim for the persistent volume, a secret to store the passwords, and maybe more.

When we want to update them, we have to update the YAML files of each, and even when we want to delete some of them, we also have to make the necessary changes in the dependent services.

And here is where Helm gets involved. Helm changes the paradigm. 

Kubernetes doesn't really care about our app as a whole. All that it knows is that we declared various objects and it proceeds to make each of them exist in our cluster.

But the Helm is built from the ground up to know these things. That's why it's sometimes called a package manager for Kubernetes. 

Helm looks at these objects as part of a larger package. When we need to perform an action, we are not telling how objects should touch. We just say which package we want to act on.

Depending on the package name, it knows which objects to change and how even if there are hundreds of objects belonging to that package.

We can customize the settings we want for our application or package by specifying the desired values during installation. Instead of having and editing multiple files in multiple YAML files, there will be a single location where we can specify each custom setting at install time.

```bash
helm install <APP_NAME>
```

It is called the values.yaml file and, this is where we can change the size of our persistent volumes, passwords, and so on.

We can upgrade our application with a single command. Helm will know what individual objects need to change to make this happen.

```bash
helm upgrade <APP_NAME>
```

We can also roll-back to the previous version.

```bash
helm rollback <APP_NAME>
```

The application can be uninstalled via a single command. It keeps track of all the objects used by each app so it knows what to remove.

```bash
helm uninstall <APP_NAME>
```

We no longer have to micro-manage every object in the Kubernetes. Helm can do that for us.

#### Helm Installation

Helm installation instruction can be found from the <a href="https://helm.sh/docs/intro/install/" target="_blank">Link</a> for Linux, Windows, or Mac operating systems.

#### Automation with Helm

The question is, how does Helm do this automation?

The first step is to convert the values found in all YAML files into templates where they can be variables, as in the example below. 

With the change below, the version of the image became a variable.

```properties
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels: 
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end    
    spec:
      containers:
      - name: nginx-container
        image: {{ .Values.image }}
  replicas: 3
  selector:
    matchLabels:
      type: front-end
```

And these values are stored in a file named values.yaml. This way,
anyone who wants to deploy this application can customize their deployment by simply changing the values from the single file of values.yaml.

```bash
image: nginx
storage: 50G
passwordEncoded: PyVWtyLmsh...
```

A combination of templates and values.yaml, gives us a final version of definition files that can be used to deploy the application on the Kubernetes cluster. The templates and the values.yaml file forms a Helm chart.

A single Helm chart may be used to deploy a simple application and it will have all the necessary template files for different services as well as the values file with the variables.

It also has a chart.yaml file that has information about the chart itself, such as the name of the chart, the charts version, a description of what the chart is, some keywords associated with the application, and information about the maintainers.

An example chart file is as follows.

```properties
apiVersion: The chart API version (required)
name: The name of the chart (required)
version: A SemVer 2 version (required)
keywords:
  - A list of keywords about this project (optional)
home: The URL of this projects home page (optional)
sources:
  - A list of URLs to source code for this project (optional)
dependencies: # A list of the chart requirements (optional)
  - name: The name of the chart (nginx)
    version: The version of the chart ("1.2.3")
maintainers: # (optional)
  - name: The maintainers name (required for each maintainer)
    email: The maintainers email (optional for each maintainer)
    url: A URL for the maintainer (optional for each maintainer)
```

We can create our own chart for your own application, or we can explore existing charts from the <a href="https://artifacthub.io/" target="_blank">Artifact Hub</a> and look for charts uploaded by other users.

This hub is called a repository that stores helm charts and there are about 6000 charts there. We can search using this web interface or we can search using the command line as below.

```bash
% helm search hub wordpress
URL                                               	CHART VERSION 	APP VERSION        	DESCRIPTION                                       
https://artifacthub.io/packages/helm/kube-wordp...	0.1.0         	1.1                	this is my wordpress package                      
https://artifacthub.io/packages/helm/riftbit/wo...	12.1.16       	5.8.1              	Web publishing platform for building blogs and ...
```

The artifact hub is the community-driven chart repository, but there are other repositories as well. In order to use other repositories, first of all, the repo must be added as follows.

```bash
% helm repo add bitnami https://charts.bitnami.com/bitnami
```

Even if we have multiple repos, we can only search a specific repo with the following command.

```bash
% helm search repo wordpress
```

We can list the existing repos using the below command.

```bash
% helm repo list
NAME   	URL                               
bitnami	https://charts.bitnami.com/bitnami
```

Once we find the chart, the next step is to install a chart on our cluster with the following command.

```bash
% helm install [release-name] [chart-name]
```

When this command is run, the Helm Chart Package is downloaded from the repository, extracted, and, installed locally. For each installation of a chart is called a release, and each release has a release name.

Other useful helm commands can be found below.

- To list installed packages

  ```bash
  % helm list
  ```

- Uninstall packages

  ```bash
  % helm uninstall my-release
  ```

- If we only want to download but not install packages

  ```bash
  % helm pull --untar bitnami/wordpress
  ```

- Install the local chart

  ```bash
  % helm install release-2 ./wordpress
  ```

## Useful Kubernetes Commands and Tricks

#### Aliases

|Alias|Object|
|-----|------|
|po|POD|
|rs|ReplicaSets|
|deploy|Deployments|
|svc|Services|
|ns|Namespaces|
|netpol|NetworkPolicies|
|pv|PersistentVolumes|
|pvc|PersistentVolumesClaims|
|sa|ServiceAccounts|

### Useful Commands

1. Set the context and namespace at the same time.

```bash
% kubectl config set-context <context-of-question> --namespace=<namespace-of-question>
```

2. Using alias in Kubernetes

```bash
% alias k=kubectl
% k version
```

3. Check api-resources.

```bash
% kubectl api-resources
NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
namespaces                        ns           v1                                     false        Namespace
nodes                             no           v1
```

4. List all fields of Pod's spec.

```bash
% kubectl explain pods.spec
```

5. Create pod with the imperative approach.

```bash
% kubectl run frontend --image=nginx --restart=Never --port=80
```

5. Create a manifest file without creating a pod using the imperative approach.

```bash
% kubectl run frontend --image=nginx --restart=Never --port=80 --dry-run=client -o yaml > pod.yaml
% vi pod.yaml
% kubect create -f pod.yaml
```

6. Check the logs of the Pod.

```bash
% kubectl logs frontend.
```

7. Open an interactive shell to the container.

```bash
kubectl exec -it nginx -n ckad -- /bin/sh
```

8. Start temporary Pod and run wget command.

```bash
kubectl run busybox --image=busybox --rm -it --restart=Never -n ckad -- wget 10.244.1.2:80
```

9. Combine 7 and 8.

```bash
kubectl run loop-pod --image=nginx -n ckad --port=80 --rm -it --restart=Never -- /bin/sh -c 'for i in 1 2 3 4 5; do echo "Test $i"; done'
```

10. Generate a YAML file.

```bash
kubectl run loop-pod --image=nginx -n ckad --port=80 -o yaml --dry-run=client --restart=Never -- /bin/sh -c 'for i in 1 2 3 4 5; do echo "Test $i"; done' > loop-pod.yaml
```

11. Check the container's Environment Variables.

```bash
kubectl exec configured-pod -- env
```

12. Create the ConfigMap by using the YAML manifest defined in the file.

```properties
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: nextapp
data: 
  config.json: |-
      {
        "dbConfig": 
          {
           "host": "localhost",
           "port": 5432,
           "dbName": "customers"
          }     
      }
```

13. Lists the events across all Pods for a given namespace.

```bash
$ kubectl get events
LAST SEEN   TYPE      REASON            OBJECT               MESSAGE
3m14s       Warning   BackOff           pod/custom-cmd       Back-off restarting \
                                                             failed container
```

14. Get the IP address and the node port of the node that runs the Pod web-app

```bash
$ kubectl get pod web-app -o json | jq '.status.hostIP' -r
```

```bash
$ kubectl get service web-app-service -o json | jq '.spec.ports[0].nodePort' -r
```

## Lab - Create a PostgreSQL Deployment

Create the following 5 YAML files with the specified names in the specified directory.

### Create a Namespace

**File_name:** 1-namespace.yaml

```properties
apiVersion: v1
kind: Namespace
metadata:
  name: postgres
```

### Create a ConfigMap

**File_name:** 2-configmap.yaml

```properties
apiVersion: v1
kind: ConfigMap
metadata:
  name: postgres-config
  namespace: postgres
  labels:
    app: postgres
data:
  POSTGRES_DB: postgresdb
  POSTGRES_USER: postgresadmin
  POSTGRES_PASSWORD: admin123
```

### Create PersistentVolumes and PersistentVolumeClaims

**File_name:** 3-pv-pvc.yaml

```properties
kind: PersistentVolume
apiVersion: v1
metadata:
  name: data-pv
  namespace: postgres
  labels:
    type: local
    app: postgres
spec:
  storageClassName: manual
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: "/mnt/data1"
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: data-pvc
  namespace: postgres
  labels:
    app: postgres
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 5Gi
---
kind: PersistentVolume
apiVersion: v1
metadata:
  name: logs-pv
  namespace: postgres
  labels:
    type: local
    data: logs
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: "/mnt/data2"
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: logs-pvc
  namespace: postgres
  labels:
    data: logs
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 10Gi
```

### Create a Postgres Deployment

**File_name:** 4-deployment.yaml

```properties
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres
  namespace: postgres
spec:
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres:11
          imagePullPolicy: "IfNotPresent"
          ports:
            - containerPort: 5432
          envFrom:
            - configMapRef:
                name: postgres-config
          volumeMounts:
            - mountPath: /var/lib/postgresql/data
              name: postgredb
            - mountPath: /logs
              name: logs
      volumes:
        - name: postgredb
          persistentVolumeClaim:
            claimName: data-pvc
        - name: logs
          persistentVolumeClaim:
            claimName: logs-pvc
```

### Create a NodePort Service

**File_name:** 5-service.yaml

```properties
apiVersion: v1
kind: Service
metadata:
  name: postgres
  namespace: postgres
  labels:
    app: postgres
spec:
  type: NodePort
  ports:
   - port: 5432
  selector:
   app: postgres
```

**Create the objects in the following order**

```bash
kubectl create -f 1-namespace.yaml
kubectl create -f 2-configmap.yaml
kubectl create -f 3-pv-pvc.yaml
kubectl create -f 4-deployment.yaml
kubectl create -f 5-service.yaml
```

**Finding the Pod the database is running on**

```bash
kubectl get all -n postgres | grep pod | awk '{print $1}'
"pod/postgres-77fc9b6c67-l4l4s"
```

**Connect to the PostgreSQL DB**

```bash
kubectl exec -it postgres-6dfd7b58bd-dn7sf -n postgres -- psql -h localhost -U postgresadmin --password -p 5432 postgresdb
```

*The password is admin123, as specified in 1-configmap file.*

**Connect to the Node where PostgreSQL is running**

```bash
kubectl exec -it postgres-6dfd7b58bd-dn7sf -n postgres -- /bin/sh
```

**Add External-IP to the service**

```bash
kubectl patch svc postgres -n postgres -p '{"spec":{"externalIPs":["192.168.0.194"]}}'
```

```bash
kubectl get services -n postgres 
NAME       TYPE       CLUSTER-IP     EXTERNAL-IP     PORT(S)          AGE
postgres   NodePort   10.96.38.170   192.168.0.194   5432:31453/TCP   75m
```

### Tutorial and Useful Links

- https://github.com/bmuschko/ckad-crash-course
- https://github.com/dgkanatsios/CKAD-exercises
- https://dev.to/liptanbiswas/ckad-practice-questions-4mpn
- https://kubernetes.io/docs/home/