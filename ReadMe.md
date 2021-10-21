#### Basic Concepts
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
    kubectl cluster info
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