## Core Concepts

**Node:** Node is a machine, physical or virtual one which Kubernetes installed. And node is a worker machine and that is where containers will be launchaned by Kubernetes. It was also known as minion's in the past.

**Cluster:** Cluster is a set of nodes grouped together.
Master node watches over the Nodes in the cluster and is responsible for the actual orchestration of containers on the worker nodes.

When you install Kubernetes on a system you're actually installing the following components.

- **An Api Server:** API Server acts as the fron end for Kubernetes. The users, management devices, command line interfaces; all talk to the API server to interact with Kubernetes cluster.
- **An etcd service:** Etcd service is a distributed reliable key-value store used by Kubernetes to store all data used to manage the cluster. When you have multiple nodes and multiple masters in your cluster, etcd store all that information on all the nodes in the cluster in a distributed manner. etcd is responsible for implementing locks within the cluster to ensure that there are no conflict between the masters.
- **A kubelet service:** Kubelet is the agent that runs on each node in the cluster. The agent is responsible for making sure that the containers are running on the nodes as expected. s
- **A Container Runtime:** The container runtime is the underlying software that is used to run containers. In our case it happens to be Docker, but there are other options as well.
- **Controllers:** The controllers are the brain behind orchestration. They are responsible for noticing and responding when nodes, conatiners or end points goes down.
- **Schedulers:** The scheduler is responsible for distributing work or containers across multiple nodes. It looks for newly created containers and assigns them to nodes. The controllers make decions to bring up new containers in such cases.

    ![pic1](images/1.png)

How are these components distributed across different types of servers. The worker node or minion as it is also known is where the containers are hosted. For example Docker containers and to run Docker containers on a system we need container **Container RunTime** installed. This does not have to be Docker, there are other container runtime alternatives available such as **rkt** (rocket) or **CRI-O**

The master server has the **kube-apiserver** and that is what makes it a master. Similarly the worker nodes have to **kubetet** agent that is responsible for interacting with a master to provide health information of the worker node and carry out actions requested by the Master on the worker nodes. 
All the information gathered are stored in a key value store on the master. The master also has the control manager and the scheduler.

**kubectl (kube control):** The kube control tool is used to deploy and manage applications on a Kubernetes cluster.

**Examples:**

- View information about the cluster
    ```
    kubectl cluster-info
    ```

- List all nodes part of the cluster**
    ```
    kubectl get nodes
    ```

#### POD

Kubernetes does not deploy conatiners directly on the worker nodes. The containers are encapsulated into a Kubernetes object known as pods. A pod is a single instance of an application. A pod is the smallest object that you can create in Kubernetes

![pic2](images/2.png)

What if you need to scale your application? 
You need to add additional instances of your Web application to share the load. Where would you pin up additioanl instances?
Do we bring up new container instance within the same pod? 
The answer is no.
We need to create a new pod with a new instance of the same application. As you can see, we have two instances of the Web application running on two separate pods on the same Kubernetes at a node.
So what if the load gets higher and the current node doesn't have enough capacity? 
Well, then you can always deploye additional pods on a new node in the cluster. You will have a new node added to the cluster to expand the cluster physical capacity.

A single pod can have multiple containers, exccpt for the fact that they're usually not multiple conatiner of the same kind.
If our intention is to scale our application, then we would need to create additional pods.

![pic3](images/3.png)

But sometimes we might have a scenario where we have a helper container that might be doing some kind of supporting task for our Web application, such as processing a user enter data, processing a file uploaded by the user, etc. and you want these helper containers to live alongside your application container.

In that case, you can have both of these containers, part of the same pod, so that when a new application conatiner is created, the helper is also created and when it dies, the helper also dies. Since they are part of the same pod.

The two containers can also communicate with each other directly by referring to each other as localhost, since they share the same network space. Plus they can easily share the same storage space as well.

##### Docker vs Kubernetes

![pic4](images/4.png)

A fundamental difference between Kubernetes and Docker is that Docker is a containerization platform, and Kubernetes is a container orchestrator for container platforms like Docker.

In Docker, we would need to establish network connectivity between containers ourselves using links and networks. 

We would need to create sharable volumes and shared among the containers and need to maintain a map of that as well. 

And most importantly, we will need to monitor the state of the application container, and when it dies, we will manually kill the helper container as it is no longer needed.

When a new container is deployed, we would need to deploy the new helper container as well with pod.

Kubernetes does all of this for us automatically. We just need to define what containers a pod consists of and the containers in a pod by default will have access to the same storage, the same network, namespace and same fate as in they will be created together and destroyed together.

Even if our application didn't happen to be so complex and we could live with a single container, Kubernetes still requires you to create pods.

This is good in the long run as your application is now equipped for architectural changes and scale in the future.

**After this step, Kubernetes must be installed in order to test the commands. You can use the links below for installation.***


<a href="https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/" target="_blank">**`Installation Guide for Linux`**</a>

<a href="https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/" target="_blank">**`Installation Guide for MacOS`**</a>

<a href="https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/" target="_blank">**`Installation Guide for Windows`**</a>

###### How to Deploy Pods?

```bash
$ kubectl run nginx --image nginx
pod/nginx created
```

`kubectl run nginx` command creates a pod automatically and deploys an instance of the nginx docker image, but where does it get application image from?

The application image and in this case the nginx image is downloaded from the repository docker hub. 
Docker hub is a public repository where the latest images of various applications are stored.

We could configure Kubernetes to pull the image from the public or a private repository within the organization.

Now that we have a pod created, how do we see the list of pods available. `kubectl get pods` command helps us see the list of pods in our cluster.

```bash
$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          8m57s
```
We have create nginx but how a user can access the nginx Web server? So, in the current state, we have not made the Web server accessible to external users. We can access it internally from the node but for now, we can not access the Web server from outside the node.

##### YAML in Kubernetes

Kubernetes uses YAML files as inputs for the creation of objects such as pods, replicasets, deployments, services, etc. All of these follow a similar structure. Kubernetes definition file always contains for top level fields.
The API version, kind, metadata and spec. These are the top level or root level properties. 
These are also required fields, so you must have them in your configuration file.

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
**apiVersion:** Version of the Kubernetes API we are using to create the object. Depending on what we are trying to create, we must use the right API version. Since we're working on pods, we will set the API version as `v1`. Few other possible values for this field are apps/v1-beta extension/v1-beta etc.

**kind:** kind  refers to type of object we are trying to create which in this case happens to be a pod. So, we will set it as `Pod`. Could be replica set, deployment or service.

**metadata:** The metadata is data about the object like its name, labels, etc. Unlike the first two where we have specified a string value this is in the form of a dictionary. So, everything under metadata is intended to the right a little bit and so name and labels are children of metadata. Children's (name and labels) locations should be the same as their sibling's.

Under metadata the name is a string value so we can name our pod. `myapp-pod` and the labels is a dictionary, so labels is a dictionary within the metadata dictionary. 
And it can have any key and value pairs as we wish. 

For now, we have added a label app with the value `myapp`. 

Similarly, we could add other labels as we see fit which will us identify these objects at a later point in time.

For example. there are hundreds of pods running a front end application and hundreds of pods running a backend application or a database. It will be difficult to group these pods once they're deployed.

If we label them now as `front-end` backend or database, we will be able to filter the parts based on this label at a later point in time.

It's important to keep in mind that under metadata we can only specify name or labels or anything else that Kubernetes expects to be under metadata.

We can not ad any other property as we wish under metadata. However under the labels we can have any kind of key or value pairs we see fit. 

So it's important to understand what each of these parameters expect.

**spec:** Spec is dictionary has a property under it called `containers`. Containers is a list an array. The reason this property is a list is because the pods can have multiple containers witin them. In this case, we will only add a single item in the list since we plan to have only a single container in the pod. 
The `-` right before the name indicates that this is the first item in the list. The item in the list is a dictionary so add name and image property the value for image is nginx which is the name of the Docker image in the docker repository.
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

To create a pod with the above yml file;

```bash
$ kubectl create -f pod-definition.yml
pod/myapp-pod created
```
kubectl decribe command will tell us information about the pod; when it was created, what labels are assinged to it, what docker containers are a part of it, and the events associated with that pod.

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

% kubectl create -f pod-definition.yml
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

##### Editing Existing Pods

1. If you are given a pod definition file, edit that file and use it to create a new pod.

2. If you are not given a pod definition file, you may extract the definition to a file using the below command.

    ```bash
    kubectl get pod <pod-name> -o yaml > pod-definition.yaml
    ```

    Then edit the file to make the necessary changes, delete and re-create the pod.

3. Use the `kubectl edit pod <pod-name>` command to edit pod properties.

#### Kubernetes Replication Controllers

![pic5](images/5.png)

Containers are the brain behind Kubernetes. They're the processes that monitor Kubernetes objects and respond accordingly.

One of the most important controllers is the replication controller.

To prevent users from losing access to the application, we would like to have more than one instance or pod running at the same time. That way, If one fails, we still have our application running on the other one.

The replication controller helps us multiple instance of a single pod in the Kubernetes cluster, thus providing high availability.

Even if you have a single pod, the replication controller can help by automatically bringing up a new pod when the existing one fails, thus the replication controller ensures that the specified number of pods are running at all times, even if it's just one or hundred.

##### Load Balancing & Scaling

Another reason we need replication controller is to create multiple parts to share the load across them.

For example, we have a single pod serving a set of users, when the number of users increase, we deploy additional pod to balance the load across the two pods.

If the demand further increases and if we were run out if resources on the first node, we could deploy additional parts across the other nodes in the cluster.

It's important to note that there are two similar terms, `Replicaton Controller` and `ReplicaSet` both have the same purpose, but they're not the same.

Replication Controller is the older technology that is being replaced by ReplicaSet, is the new recommended way to set up replication.

**Replication Controller Example:**
The Replication Controller configuration file and the previously used configuration file are very similar to each other.

The extra thing to do here is to move all the contents of the pod definition (metadata and spec parts) to the template section of the file replication controller. They should be children of the template section.

Looking at our file now, we have two metadata sections; one is for the replication controller and another for the pod. And also we have two spec section one for each.

The replicas parameter is the number that determines how many pod there will be at a time. It is important to note that replicas and template sections are direct children of the spec section.

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

Using the yaml file, to create a Replication Controller and check its status;

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
- apiVersion is different what we had before for application controller
- The spec section is exactly the same except!
  - ReplicaSet requires a selector definition, the selector section helps the ReplicatSet identify what pods fall under it. It's because ReplicaSet also manage pods that were not created as part of the ReplicaSet creation. For example, there were pods created before the creation of the ReplicaSet that matched labels specified in the selector. The ReplicaSet also take those pods into consideration when creating the replicas.
  The selector is not a required field in case of a replication controller, but it's still available. When you skip, it assumes it to be the same as the labels provided in the pod definition file. In case of ReplicaSet, a user input is required for this property and it has to be written in the form of `MatchLabels`.
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

Using the yaml file, to create a ReplicaSet and check its status;

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

##### Labels and Selectors

![pic6](images/6.png)

Why do we label our pods and objects in Kubernetes?

Say we deployed three instances of our front-end Web application as three pods. We would like to create a replication controller or replicaset to ensure that we have three active pods at any time.

I case they were not created, the replicaset will create them for you. The role of the replicaset is to monitor the pods and if any of them were to fial, deploy a new ones. The replica set is in fact a process that monitors the pods.

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

This way the replicaset knows which pods of monitor.
The same conecpt of labels and selectors is used in many other places throughout Kubernetes.

##### Scale

Say we started with three replicas in the future we decided to sclae to six.
There are multiple ways to do it.

1. Update the number of replicas in the definition file to six.

    ```
    replicas: 6
    selector:
      matchLabels:
        type: front-end
    ```

    Then run the `kubectl replace -f replicaset-definition.yml` command to specify the same file and that will update the replica set to have six replicas.

2. Run the kubectl scale command used the replicas parameter to provide the new number of replicas and specify the same file as input.

    ```
    kubectl scale --replicas=6 -f replicaset-definition.yml
    ```

    We either input the definition file or provide the replicaset name in the type name format.

    ```
    kubectl scale --replicas=6 replicaset myapp-replicaset
    ```

    However, remember that using the filename as an input will not update the existing number of replicase in the file.
    In other words, the number of replicas in the replicaset-definition.yml file will still be three, even though you scaled your replica set to have six replicas using the kubectl scale command and the file as input.

##### Commands

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

#### Deployments

![pic8](images/8.png)

You have a Web server and that needs to be deployed in a production environment. You need many instances of running web server. And also whenever a newer version of application builds become available on the Docker registry and if you upgrade them all at once, this may impact users accessing the application. So, you might want to upgrade them one after the other.

Let's say your upgrade resulted in an error and you want to roll back the changes that were recently carried out.

Another scenario that can be illustrated is,you would like to make multiple changes to your environment such as upgrading the underlying web server versions, scaling the environment, modifying the resource allocations etc...

And you would like to apply a pause to your environment, make the changes and then resumes. So, all the changes are rolled out together.

All of these capabilities are available with the Kuberetes deployments.

Each container is encapsulated in PODs. Multiple set PODs are deployed using replication controllers or replicaset.
And then comes deployment which is a Kubernetes object that comes higher in the hierarchy.

The deploymeny provides us with the capability to upgrade the underlying instances seamlessly using rolling updates, undo changes, pause and resume changes as required.

Replicaset and deployment definition files are similar. Except for the kind which is going to be deployment.

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

Deployment automatically creates a Replica Set and PODs. If we run the commands, we will be able to see the PODs with the name of deploymant and replicaset.

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

##### Output Commands

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

#### Namespaces

![pic9](images/9.png)

Let's assume there are two boys named Michael to differentiate them from each other, we call them by their last names Jordan and Jackson.
They come from different houses and there are other members in the house. Individuals in the household only address each other by their first names.

For example, the father addresses is Michael. However, if the father wishes to address Michael in the other house, he uses his full name. (Michael Jackson)

Someone outside of these houses would also use the full name to refer to anyone these houses.

Each of these houses have their own set of rules that defines who does what. Each of these houses have their own set of resources that can consume.

These houses correspond to Namespaces in Kubernetes.

We have created objects such as PODs, Deployments and services in our cluster. Whatever we have been doing, we have been doing within a Namespace.

We were inside a house all this while, this namespace is known as the default Namespace and it's created automatically by Kubernetes when the cluster is first setup.

Kubernetes creates a set of PODs and services for it's internal purpose such as those required by the networking solution, the DNS service etc.

To isolate these from the user and to prevent accidentally deleting or modifying the services, Kubernetes creates them under another Namespace created at cluster startup named `kube-system`.

A third namespace created by Kubernetes automatically is called `kube-pulic`. This is where resources that should be made available to all users are created.

![pic10](images/10.png)

If you want to use the same cluster both Dev and Production environment but at the same time isolate the resources between them, you can create a different namespace for each of them.

That way while working in the Dev environment you don't accidentally modify resources in production.

Each of these Namespaces can have it's own set of policies that define who can do what.

We can also assign POD of resources to each of these Namespaces, that way each Namespace is guaranteed a certain amount and does not use more its allowed limit.

The default Namespace that we've been working on, just like how the members within the house refer to each other by their first names. The resources within a namespace can refer to each other simply by their names.

**Example:**
  - **Namespace:** Default
  - **Objects:**
    - web-pod
    - db-service
    - web-deployment

In this case, the webapp-pod can reach db-service simlpy using the hostname db-service.

> mysql.connect("db-service")

If required, the webapp-pod can reach a service in another Namespace as well. For this you must append the name of the namespace to the name of the service.

For example for the web-pod in the default namespace to connect to the database in the Dev environment, in this case format should be `service.namespace.svc.cluster.local` 

> mysql.connect("db-service.dev.svc.cluster.local")

We're able to do this because when the service is created a DNS entry is added automatically in this format.

Looking closely at the DNS name of the service, the last part `cluster.local` is the default domain name of the Kubernetes cluster `.svc` is the subdomain for service followed by the namespace and then the name of the service itself.

![pic11](images/11.png)

To list PODs in another namespace, we need to use the namespace option in the command along with the name of the namespace.

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

To create a pod in anothe namespace, we need to use the namespace option.

> kubectl create -f pod-definition.yml --namespace=dev

If we want to make sure that this pod gets created in the dev environment all the time even if we don't specify the namespace in the command line, we can put the namespace definition into the pod definition file under the metadata section.

```properties
metadata:
  name: myapp-pod
  namespace: dev
  ...
```

So, how do we create a new namespace? Like any other object, use a namespace definition file. The API version is v1, kind is namespace and under metadata specify the name.

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

But what if we want to switch to the dev namespace permanently so that we don't have to specify the namespace option anymore.

In that case use the kubectl config command to set the namespace in the current context to dev.

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

Similary. we can switch to the prod namespace the same way.

To view pods in all namespace, we can use the all namespaces option in the command.


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

Taking a closer look at the command `kubectl config set-context $(kubectl config current-context) --namespace=dev`, this command first identifies the current context and then sets the namespace to the desired one for that current context. Context are used to manage multiple clusters in multiple environments from the same management system.

##### Resource Quota

To limit resources in a namespace, we can create a resource quota. We first need to specify the namespace for which we want to create a quota and the under spec, we can provide our limits then creat the quota.

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

#### Imperative commands

Although we mostly work declaratively using definition files, we can quickly handle one-time tasks using imperative commands and at the same time easily create a definition template.

By default as soon as the command is run, the resource will be created. If you simply want to test your command use the below option. This will not create the resource, instead, tell you whether the resource can be created and if your command is right.
  - `--dry-run=client`

##### Pod

1. Create an NGINX Pod
  - `kubectl run nginx --iname=nginx`

2. Generate Pod manifest yaml file with `-o yaml option`. This command will not create it with the `--dry-run` option. 

  - `kubectl run nginx --image=nginx --dry-run=client -o yaml`

##### Deployment

1. Generate deployment with 4 replicas

  - `kubectl create deployment nginx --iname=nginx --replicas=4`

2. Scale a deployment 

  - `kubectl scale deployment nginx --replicas=5`

3. Alternatife way to generate deployment is to save the yaml definition to a file and modify.

  - `kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml`

  We can save then update the yaml file with the other fields before creating the deployment.

##### Service

1. Create a service named redis-service of type ClusterIP to expose pod redis on port 6379. The following command will automatically use the pod's labels as selectors.

  - `kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`

2. Alternatife way of 1. The following command will not use the pods labels as selectors, instead it will assume selectors as **app=redis**

  - `kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml`

3. Create a service named ngix of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes. The following command will automatically use the pod's labels as selectors but we can not specify the node port. We have to generate a definition file and then add the node port in manually before creating the service with the pod.

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

#### Commands & Arguments

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

The docker file has an entry point as well as CMD instructions specified. The entry point is the command that is run at statup and the CMD is the default parameter passed to the command. With the args option in the pod definition file, we override the code CMD instruction in the docker file.

But what if we need to override the entry point? For instance, from sleep to an imaginary sleep 2.0 command.

In the docker world, we can run with entrypoint option set to the new command as follows.

> docker run --name ubuntu-sleeper --entrypoint sleep2.0 ubuntu-sleeper 10

The corresponding entry in the pod definition file would be using a command field. The command field corresponds to entry point instruction in the docker file.

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

##### Editing PODs and Deployments

###### Edit PODs

We can not edit specifications of an existing POD other than the below.

- spec.containers[*].image
- spec.initContainers[*].image
- spec.activeDeadlineSeconds
- spec.tolerations

For example, we cannot edit the environment variables, service accounts, resource limits of a running pod. But if we really want to, we have 2 options.

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

###### Edit Deployments

With Deployments we can easily edit any field/property of the POD template.

Since the pod template is a child of the deployment specification, with every change the deployment will automatically delete and create a new pod with the new changes. 

So if you are asked to edit a property of a POD part of deployment we may do that simply by running the command

> kubectl edit deployment my-deployment

#### Environment Variables

When we want to set an environment, we can use the env attribute in the YAML file. Env is an array so every item under the env property startus with a "-" indicating an item in the array.

Each item has a name and a value property.
The name is the name of the environment variable made available with the container and the value is its value.

```properties
spec:
  env:
    - name:  app_colur
      value: blue
```

A direct way of specifying the environment variables using a plain key-value pair format. However there are other ways of setting the environment variables such as using config maps and secrets.

The difference in this case is that instead of specifying value we say `valueFrom` and the specification od conflict maps and secret keys.

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

Config maps are used to pass configuration data in the form of key value pairs in Kubernetes.

When the pod is created, the config map is injected into the pod. So the key-value parents are available as environment variables for the application hosted inside the container in the pod.

There are two phases involved in configuring config maps.

1. Create the ConfigMap
2. Inject them into the pod.

There are two ways of creating a ConfigMap. The imperative way, without using a definition file and the declarative way by using a config  map definition file.

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

For the declarative approach, we can create a definition file. In the same way as in the pod file; file has apiVersion, type and metadata. But here we have data instead of spec. Let's say the file is named config-map.yaml.

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

We can create as many ConfigMaps as wee need in the same way for vaious different purposes.

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

To inject an environmet variable, add a new property to the container called `envFrom`. The envFrom property is a list so we can pass as many environment variables as required.

Each item in the list corresponds to a configmap item. We can specify name of the configMap we created earlier.

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

Let's assume, we have a simple python we application that connect to the MySQL database.

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

One option would be to move these values into a conflict map but the conflict map stores configuration data in plain text format. So, it's definetly not the right place to store a password.

Secrets are used to store sensitive information like passowrd or keys. The're similar to conflict maps except that they're stored in an encoded or hashed format as with conflict maps.

There are two steps involved in working with secrets.

1. Create a secret.
2. Injected into the pod.

There are two ways of creating a secret. As in ConfigMap, imperative and declarative.

<span style="color:red">Imperative</span>

```bash
kubectl create secret generic 
  <secret-name> --from-literal=<key>=<value>
```

If you want to add additional key-value paris, you can add as many --from-literal options as you want.

```bash
kubectl create secret generic \
  app-secret --from-literal=DB_Host=mys \
             --from-literal=DB_user=root \
             --from-literal=DB_Password=paswrd
```

However this could get complicated when you have too many secrets to pass. In another wat to input the secret data is through the file. Use the from file option to specify a path to the file that contains the required data.

```bash
kubectl create secret generic \
  <secret-name> --from-file=app_secret.properties
```

The data from this file is read and stored under the name of the file. 

<span style="color:green">Declarative</span>

```bash
kubectl create -f secret-data.yml
```

For declarative metod, we create a definition file juts like how we did the conflict map. The file contains apiVersion, type, meta and data section as below and let's name the file secret-data.yaml to use it in the next steps.

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
On a linux host, run the command `echo -n` followed by the exte we're trying to convert.

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

To view more information, `kubectl describe secrets` command can be used. This shows the attributes in the secret but hides the value themselves to view the values as well.

When we execute the `kubectl get secret <secret-name> -o yaml` command, we can see the hash value. 

So, how do we decode these hashed values? When we run the following commands, we can get the actual passwords.

```bash
% echo -n 'bXlzcWw=' | base64 --decode
mysql%                                                                        

% echo -n 'cGFzd3Jk' | base64 --decode
paswrd%
```

##### Secrets in Pods

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

To inject an environmet variable, add a new property to the container called `envFrom`. The envFrom property is a list so we can pass as many environment variables as required.

Each item in the list corresponds to a secret item in the list. We can specify name of the secret we created earlier.

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

What we just saw was injection secrets as environment varialbles into the pods. There are other way to inject secrets into pods. 

We can inject a single environment variables;

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

In this case, since we have three attributes in our secret, there files are created. And If we look at the contents of the DB_Password file we see the password in it.

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

Also the way kubernetes handles secrets. Such as:

* A secret is only sent to a node if a pod on that node requires it.
* Kubelet stores the secret into a tmpfs so that the secret is not written to disk storage.
* Once the pod that depends on the secret is deleted, kubelet will delete its local copy of the secret data as well.

Read about the <a href="https://kubernetes.io/docs/concepts/configuration/secret/#protections" target="_blank">**Protections**</a> and <a href="https://kubernetes.io/docs/concepts/configuration/secret/#risks" target="_blank">**Risks**</a>  of using secrets.

There are other better ways of handling sensitive data like passwords in Kubernetes, such as using tools like Helm Secrets, HashiCorp Vault.

#### Docker Security

##### Process Isolation

Unlike virtual machines, containers are not completely isolated from their host. Containers and the host of containers share the same kernel. Containers are isolated using Namespace in Linux. The host has a Namespace and the containers have their own Namespace. All the processors run by the containers are in fact run on the host itself, but in their own namespace. As far as the docker container is concerned, it is in its own namespace and it can see its own process only, It can not see anything outside of it or in any other namespace.

Suppose we are running the `docker run ubuntu sleep 3600` command in Docker, when we list the processes from withing the docker container, we see the sleep process with a process id of one.

```bash
% ps aux
USER          PID  %CPU %MEM      VSZ    RSS   TT  STAT STARTED      TIME COMMAND
root          1     0.0  0.0      4708   724   ??  S    Mon09AM 156:58.82 sleep 3600
```

For the docker host all processes of its own as well as those in the child Namespaces are visible as just another process in the system. So, when we list the processes on the host, we see a list of processes including the sleep command but with a different process id.


```bash
root               229   0.7  0.1  4397552  12204   ??  Ss   Mon09AM   1:24.24 /usr/libexec/trustd
root                 1   0.6  0.1  4360104  18148   ??  Ss   Mon09AM   9:24.69 /sbin/launchd
root               146   0.6  0.0  4378072   5128   ??  Ss   Mon09AM   0:26.44 /usr/libexec/diskarbitrationd
oktaytuncay      27808   0.5  2.3 34550456 389200   ??  S    12:58PM   1:12.97 /Applications/Google Chrome.app/Contents/Frameworks/
root              1754   0.0  0.0     4708    724   ??  S    Mon09AM 156:58.82 sleep 3600
```

This is because the processes can have different process ids in different namespaces and that's how docker isolates containers within the system.

##### Users

The docker host has a set of users. By default docker processes run within containers as the root user. This can be seen in the output of the command we ran above. If you don't want to run with root user, `--user` parameter can be used.

> docker run --user=1017 ubuntu sleep 3600

Another way to enforce user security is to have this defined in the docker image itself at the time of creation. For example, we can use the default ubunut image and set the used id to 1017 using the user instruction. Then we can build the custom image.

By default docker runs a container with a limited set of capabilities. And so the process running within the container do not have the privileges to reboot the host or perform operations that can disturb the host or other containers running on the same host. 

If you wish to override this behaviour and provide additional privileges then what is available, use the `cap-add` option in the docker run command.

> docker run --cap-add MAC_ADMIN ubuntu

Similarly, we can drop privileges as well using the `cap-drop` option or in case we wish to run the container with all privileges enabled, then we can use `privileged flag`

> docker run --cap-drop KILL ubuntu

> docker run --privileged ubuntu

##### Security Contexts

When we run a docker container, we have the option to define a set of security standards such as `--user`, `--cap-add`, `--cap-drop`. These can be configred in Kubernetes as well. As we know already in Kubernetes containers are encapsulated in pods. We may choose to configure the security settings at a container level or at a pod level.

If we configure it a pod level, the settings will carry over to all the containers within the pod. If we configure it at both the pod and container, the settings on container will override the settings on yhe pod.

The following pod runs an Ubuntu image with the sleep command. To configure the security context in the container, we can  add a field called `securityContext` under the spec section.

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

#### Service Account

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

In that case, this whole process of exporting the service account token and configuring the third party application to use it can be made simpleport automatically mounting the service token secret as a volume inside the pod.

That way, the token to access the Kubernetes API is already placed inside the pod and can be easily read by the application, we don't have to provide it manually.

When we look at the list of service accounts, we will see that there is a default service account that exits already for all namespace in Kubernetes.

```bash
% kubectl get serviceaccount          
NAME           SECRETS   AGE
dashboard-sa   1         26m
default        1         19d
```

A service account named default is automatically created. Each namespace has its own default service account. Whenever a pod is created, the default service account and it's token are automatically mounted to that pod as a volume mount.

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

The secret token mounted at location `/var/run/secrets/kubernetes.io/serviceaccount` inside the pod.

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

We can not edit the service account of an existing pod. We must delete/re-create the pod. However, in case of a deployment, we'll able to edit the service account as any changes to the pod definition file will automatically trigger a new rollout for the deployment.

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

#### Resource Requirements

Whenever a pod is placed on a node, it consumes resources available to that node. The scheduler takes into consideration the amount of resources required by a pod and available resources on the nodes.

If the node has no sufficient resources, the scheduler avoids placing the pod on that node, instead of placing the pod on one where sufficient resources are available.

If there is no sufficient resources available on any of the nodes, Kubernetes host back scheduling the pods and we'll see the pod in a pending state.

By default, Kubernetes assumes that a pod or a container withing a pod, requires 0.5 CPU and 256 Mb of memory. When the scheduler tries to place the pod on a node, It uses these numbers to identify a node which has sufficient amount of resources available.

If we know that our application will need more than this numbers, we can modify numbers by specifying in the pod or deployment definition file as below.

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

1 count of CPU is equivalent to 0.5 OCPU, 1 AWS vCPU, 1 Azure Core and 1 Hyperthread. 

We could request a higher numer of CPU for the container, provided our nodes are sufficiently funded.

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

A container can not use more CPU resources than its limit. However, this is not the case with the memory. A container can use more memory resources than its limit. So if a pod tries to consume more memory than its limit constantly, the pod will be terminated.

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

As of now there are no restrictions or limitations and scheduler places the pods across all of the nodes to balance them out equally.

![pic13](images/13.png)

So let's say we have dedicated resources on Node 1 for a particular use case or application. So, we would like only those pods that belong to this application to be places on Node 1.

First, we prevent all pods from being placed on the node by placing a taint on the node. Let's call it blue.

So in this case none of the pods can be placed on Node 1, as none of them can tolerate the taint blue. This solves half of our requirement. No unwanted pods are going to be places on this node. 

The other half is to ensure that certain pods are placed on this node. For this, we must specify which pods are tolerant of this particular taint. 

In our case, we want to allow only pod D to be placed on this node. Thus, we add toleration to pod D and pod D is toleranted to blue.

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

IF we would like to dedicate the node to pods in application blue, then the key value pair would be app=blue.

The `taint-effect` defines what would happen to the pods if they do not tolerate the taint. There are three main effects.

- **NoSchedule :** The pods will not be scheduled in the node as described above
- **PreferNoSchedule :** The system will try to avoid placing the pod on the node but that is not guarenteed.
- **NoExecute :** New pods will not be scheduled on the node and existing pods on the node. If any, will be evicted if they do not tolerate the taint. These pods may have been scheduled on the node before taint was applied to the node.

**Example :**
```bash
kubectl taint nodes node1 app=blue:Noschedule
```

Tolerations are added to pods. To add a toleration to a pod, in the spec section of the pod definition file add a section called tolerations. Move the same values used while creating the taint under the section.

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

We had three nodes running some workloads. We do not have any taints or tolerations at this point. So they are schedued this way.

After scheduled, we then decide to dedicate Node 1 for a special application and as such we taint the node with the application name and add a toleration to the pod that belongs to the application, which happens to be pod D in this case.

While tainting the node we set to taint effect to NoExecute. And as such once the taint on the node takes effect it evicts pod C from the node. Which simply means that the pod is killed.

The pod D continues to run on the node as it has a toleration to the taint blue.

Remember taints and tolerations are only meant to restrict nodes from excepting certain pods. In this case Node 1 can only accept pod D but id does not guarantee that pod D will always be placed on Node 1 since there are no taints or restrictions applied on the other two nodes.

So remember taints and tolerations does not tell the pod to got to a particular node. Instead it tells the node to only accept pods with certain tolerations.

If the requirement is to restrict a pod to certain nodes, this can be achieved with `node affinity`.

So far we have only been referring to the worker nodes. We also have master nodes in the cluster, which is technically just another node that has all the capabilities of hosting a pod plus it runs all the management software.

If you noticed, the scheduler does not schedule any pods on the master node. Why is that?

When the Kubernetes cluster is first set up, a taint is set on the master node. Automatically that prevents any pods from being scheduled on this node. 

We can see this and modify this behavior if needed. However a best practice is not to deploy application workloads on a master node. 

- To see this taint:

  ```bash
  kubectl describe node kubemaster | grep Taint
  ```

#### Node Selectors

![pic16](images/16.png)

Let' say, we have a three-node cluster and two are smaller nodes with lower hardware resources and one of them is a larger node and configured with higher resources.

We have different kinds of workloads running in the cluster. For workloads that require higher horsepower, we want to forward it to the larger node in case the job requires extra resources.

However in the current default setup, any pod can go to any nodes. So, if the pod is deployed to pod 2 or 3, it may not receive the resource it needs.

To avoid this, we can set a limitation on the pods so that they only run on particular nodes. There are two ways to do this.

The first is using node selectors, which is the simple and easier method. To limit the pod to run on the larger node, we can add a new property called `nodeSelector` to the spec section and specify the size as `Large`.

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

The primary purpose of node affinity feature is to ensure that pods are hosted on particular nodes.

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

If we think our pod could be placed on a large or a medium node, we could simply add the value to the list of values like below.

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

The `exists` operator will simply check if the label size exist on the nodes and we don't need the values section for that as it does not compare the values.

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

There is also additional type of node affinity planned as follows.

- `requiredDuringSchedulingRequiredDuringExecution`

There are two states in the life cycle of a pod when considering node affinity, `DuringScheduling` and `DuringExecution`.

**DuringScheduling :** is the state where a pod does not exist and is created for the first time. We have no doubt that when a pod is created, the affinity rules specified are considered to place the pod on the right node. 

What if the node with matching labels are not available? For example, we forgot to label that node as Large. That is where the type if node affinity used comes into play.

| |DuringScheduling|
|-|----------------|
|Type 1|Required|
|Type 2|Preferred|

If you select the required type, which is the first row, the scheduler will mandate that the pod be placed on a node with the given affinity rules.

If it cannot the find one, the pod will not be scheduled, this type will be used in cases where the placement of the pod is crucial. If a matching node does not exist, the pod will not be scheduled. 

But let's say the pod placement is less important than running the workload itself.

In that case, we could set it to preferred and in cases where a matching node is not found, the scheduler will simply ignore no definitive rules and place the pod on any available node.

Thus is a way of telling scheduler, ***try the best to place the pod on matching node, but if you really cannot find one, just place is anywhere.***

**DuringExecution :** is the state where a pod has been running and a change is made in the environment that affects node affinity, such as a change in the label of a node.

For example, an administrator removed the label we said earlier, `size=Large` from the node. What would happen to the pods that are running on the node?

| |DuringExecution|
|-|----------------|
|Type 1|Ignored|
|Type 2|Ignored|

As we can see above, there are two types of node affinities available today and this value is set to ignore it. 

This means that the pods will continue to run and any changes in node affinity will not affect them once they are scheduled.

### Multi-Container PODs

There are different patterns of Multi-Container pods in Kubernetes such as the Ambassador, Adapter and Sidecar.

The idea of decoupling a large monolithic application into sub-components known as Microservice enables us to develop and deploy a set of independent small and reusable code.

This architecture can then help us scale up, down as well as modify each service as required as opposed to modifying the entire application.

However, sometimes we may need two services such as a web server and a logging service to work together. We need one agent instance per web server instance paired together that can scale up and down together.

And that is why we have multi-container pods that share the same lifecycle which means they are created together and destroyed. They share the same network layer which means they can refer to each other as localhost and they have access to the same storage volumes.

Thus way, we don't have to establish volume sharing or services between the pods to enable communication between them.

To create a multi-container pod, add the new container of information to the pod definition file. 

Remember the container section under the spec section in a pod definition file is an array and the reason it is array is to allow multiple containers in a single pod.

In this case we add a new container named log-agent to our existing pod-definition file.

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

  - name: log-agent
    image: log-agent
```

There are three common patterns when it comes to designing multi-container pods.

The first is the Sidecar pattern in the logging service example above. The others are the Adapter and the Ambassador pattern.

A good example of a Sidecar pattern is deploying a logging agent alongside a web server to collect logs and forward them to a central log server.

Building on that example, say we have multiple applications generating logs in different formats. It will be hard to process the various formats on the central logging server. So before sending the logs to the central server, we would like to convert the logs to a common format.


```sql
- 17-Sep-2021 17:07:12 "GET /index1.html" 200

- 17/Sep/2021:17:07:12 -800 "GET /index2.html" 200

- GET 1621451764 "/index3.html" 200
```

For this, we deploy an Adapter container. The Adapter container processes the logs before sending it to the central server

```sql
- 17-Sep-2021 17:07:12 "GET /index1.html" 200 
  -> 17-Sep-2021 17:07:12 "GET /index1.html" 200 

- 17/Sep/2021:17:07:12 -800 "GET /index2.html" 200 
  -> 17-Sep-2021 17:07:12 "GET /index2.html" 200 

- GET 1621451764 "/index3.html" 200 
  -> 17-Sep-2021 17:07:12 "GET /index3.html" 200 
```

So our application communicates to different database instances at different stages of development. Let's say we have Prod, Test ,and Dev databases.

We should make sure to change this connectivity in our application code depending on the environment in which we are deploying our application.

We may choose to outsource logic to a separate container within our pod. So that our application can always refer to a database at the localhost and the new container can proxy the request to the right database.

This is knows as an Ambrassador container.

These are different patterns in designing a multi-container pod. When it comes to implementing them using a pod definition file, It is always the same. We simply have multiple containers within the pod definition file.

### Observability

#### Readiness Probes

A pod has a pod status and some conditions. The pod status tell us where the pod is in it's lifecycle. When a pod is first created, it is in a pending state. This is when the scheduler tries to figure out where to place the pod.

If the scheduler can't find the node to place the pod, it remains in a pending state. To find out why it's stuck in a pending state, we can run `kubectl describe pod` command and it can tell us exactly why.

Once the pod is scheduled, it goes into a `ContainerCreating` status where the images required for the application are pulled and the container starts.

Once all the container in a pod starts, it goes into a `Running` state where it continues to be until the program completes successfully or is terminated.

Conditions define pods status. It is an array of true or false values that tell is the state of the pod. When a pod is scheduled on a node, the `PodScheduled` condition is set to true. When the pod is `Initialized`, it's value is ste to true.

We know that a pod has multiple containers. When all the containers in the pod are ready, the `ContainersReady` condition is set to True and finally the pod itself is considered to be `Ready` 

- `PodScheduled` **<span style="color:green">True</span>** or <span style="color:red">False</span>
- `Initialized` **<span style="color:green">True</span>** or <span style="color:red">False</span>
- `ContainersReady` **<span style="color:green">True</span>**  or <span style="color:red">False</span>
- `Ready` **<span style="color:green">True</span>**  or <span style="color:red">False</span>

If we try to run an instance of an Jenkins server, we'll notice that it takes about 10 to 15 seconds for the server to initialize before a user can access the WebUI.

Even after the WebUI is initialized, it takes a few seconds for the server to warm-up and be ready to serve users. During this wait period, if we look at the state of the pod, it continues to indicate that the pod is ready which is not true. 

So, how does Kubernetes know whether the application inside the container is actually running or not. And why does it matter if the state is reported correctly?

Let's look at a simple scenario where we create a pod and expose it to external users using a service. The service will route traffic to the pod immediately.

The service relies on the pods ready condition to route traffic. By default, Kubernetes assumes that as soon as the container is created, it is ready to serve user traffic. So, it sets the value of the ready condition for each container to True.

But if the application within the container took longer to get ready the service is unaware of it and sends traffic through as the container is already in a ready state causing users to hit a pod that is not yet running a live application.

What we need here is a way to tie the ready condition to the actual state of the application inside the container. As a developer of the application, we know better what it means for the application to be ready.

![pic17](images/17.png)

There are different ways that we can define if an application inside a container is actually ready. We can setup different kinds of HTTP tests to see if the API server response.

In case of a database, we may set to see if a particular TCP socket is listening or we may simply execute a command within the container to run a custom script that will exit successfully if the application is ready.

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

Let's say we run an nginx image on Docker and it starts to server users but for some reason, WebServer crashes and the ngix process and the container exits.

We noticed that when we run the `docker ps -a` command, the status is `Exit (1) 17 seconds ago`.

Since docker is not an orchestration engine, the container continues to stay dead and it will continue to deny requests until we manually create a new container.

When we run the same Web Application with Kubernetes, when the application crashes, Kubernetes tries to restart the container to restore the service to the users.

We can see the count of restarts increase in the output of `kubectl get pods` command.

However, what if the application is not really working but the container continues to stay alive? 

Say, for instance, due to a bug in the code, the application is stuck in an infinite loop. According to Kubernetes, the container runs and therefore assumes the application is up however, users who reach the container cannot use the service.

In that case, the container needs to be restarted or destroyed and a new container is to be brought up. That is where the liveness probe can help us.

Liveness probe can be configured on the container to periodically test whether the application within the container is actually healthy.

If the test fails, the container is considered unhealthy and is destroyed and created.

For a Web application, it could be when the API server is up and running, or for a database, we may test to see if a particular TCP socket is listening or we may simply execute a command to perform a test.

The liveness probe is configured in the definition file, as we did with the readiness probe. The difference is to user liveness instead of readiness.

```properties
livenessProbe:
  httpGet:
    path: /api/ready
    port: 8080
```

Similar to readiness probe, we have httpGet option for APIs, tcpSocket for port and exec for commands and as well as additional options like `initialDelaySeconds` before the test is run, `periodSeconds` to define the frequency and `failureThreshold`

#### Container Logging

Let's start with logging in Docker. When we run `docker runcodes/event-simulator` in Docker, it generates random event simulating a web server. These are events streamed to the standard output by the application. 

If we were to run the same command in a detached mode using `-d` option, we would not see the logs. If we wanted to view the logs, we can use the docker logs command followed by the container id. The `-f` option helps us see the live log trail.

> docker logs -f <cont_id>

We can create pod with the same docker image using the pod definition file.

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

If there are multiple containers within a pod, we must specify the name of the container explicitly in the command. Otherwise, It would fail asking you to specify a name. In this case, we need to specify the name of the container and that prints the relevant log messages.

```bash
kubectl logs -f myapp-pod nginx-container
```

This is how the simple logging functionality implemented with Kubernetes works

#### Monitor and Debug Applications

In Kubernetes, we can monitor performance metrics such as CPU, memory, network, and disk utilization, as well as node-level metrics such as the number of nodes in the cluster and how many are healthy.

Pod-level metrics like the number of pods and performance metrics like CPU and memory consumption on each pod.

So we need a solution that will monitor these metrics, store them and provide analytics around this data.

Kubernetes does not come with a full feature built-in monitoring solution. But there are a number of open source solutions available like Metrics Server, Prometheus, Elastic Stack and Proprietary solutions like Datadog and Dynatrace.

Heapster was one of the original projects that enabled monitoring and analysis features for Kubernetes. A lot of regerence can be found for reference architectures on monitoring Kubernetes. However, heapster is now deprecated and slimmed down version was formed known as the Metric Server.

We can have one metric server per Kubernetes cluster. The Metrics Server retrieves metrics from each of the Kubernetes nodes and pods, aggregates them and store in memory. 

Note that the metrics server is only an in-memory monitoring solution and does not store the metrics on the disk. And as a result we can't see historical performance data.

So how to generate metrics for pods in these nodes?

Kubernetes runs an agent on each node called kubelet which is responsible for receiving instructions from the Kubernetes API Master server and running pods on the nodes. 

The kubelet also contains a subcomponent known as the cAdvisor or Container Advisor. cAdvisor is responsible for retrieving performance metrics from pods and exposing them through the kubelet API to meet the metrics available for the metrics server.

If we are using minicube for the local cluster, the below command will enable the metric server.

> minikube plugins metric-enable server

For all other environments, the following command deploys the metric server by cloning the metric server deployment file from the GitHub repository.

```bash
% git clone https://github.com/kubernetes-sigs/metrics-server
```

Then deploying the required components using the kubectl apply command. This command deploys a set of pods, services and roles to enable the metric server to poll for performance metrics from the nodes in the cluster.

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Verify that the metrics-server deployment is running the desired number of pods with the following command.

```bash
% kubectl get deployment metrics-server -n kube-system

NAME             READY   UP-TO-DATE   AVAILABLE   AGE
metrics-server   1/1     1            1           2m48s
```

A little while later, the metric server will collect and process data. After processing, cluster performance can be viewed by running the following command

```bash
% kubectl top node

NAME          CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%   
10.0.10.214   42m          1%     2142Mi          6%        
10.0.10.25    27m          0%     2072Mi          6%        
10.0.10.252   46m          1%     2108Mi          6% 
```

The above command provides the cpu and memory consumption of each of the nodes.

The following command can be used to view performance metrics for pods in Kubernetes.

```bash
% kubectl top pod

NAME                                CPU(cores)   MEMORY(bytes)   
myapp-deployment-7df67f74c5-lnnzq   0m           4Mi             
myapp-deployment-7df67f74c5-ndb8n   0m           3Mi             
myapp-deployment-7df67f74c5-zdfgq   0m           3Mi             
myapp-pod
``` 