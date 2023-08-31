
# K8S Tutorial

## MiniKube
- minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.
- can only create single node cluster.

### Exploring Minikube Commands
- minikube start
- minikube kubectl -- get po -A
- minikube docker-env
- docker container ls
- minikube ssh - to enter into the container
- kubectl config current-context
- kubectl get all --all-namespaces
- minikube stop - data will be preserved
- minikube delete


## Kubeadm
- used to create multi node clusters


### Linux Commands for Kubernetes
- ls -> lists the information about directories or files in current directory.
- ls -ltr -> gives little bit more information
- cd -> change directory
- touch name_of_file -> create an empty file
- vi name_of_file  -> text editor -> followed by :wq! to exit
- cat name_of_file -> reading the files sequentially
- cp name_of_file destination-> copy command
- mv name_of_file destination -> move command
- chmod permission_code name_of_file -> changing file permissions
- kill -> killing the process
- env -> getting the list of environment variables
- export -> exporting the variables
- curl -> transfer data from or to a server
- tar -> archive utility
- du -> disk usage command
- ping -> network connectivity

## Architecture
![[Pasted image 20230807163144.png]]
\






### K3s

- Fully compatible Kubernetes distribution.
- Less Complex than **K8s**

#### **K3s is a fully compliant Kubernetes distribution with the following enhancements** :

- Packaged as a single binary.
- Lightweight storage backend based on sqlite3 as the default storage mechanism. etcd3, MySQL, Postgres are also available.
- Wrapped in simple launcher that handles a lot of the complexity of TLS and options.
- Secure by default with reasonable defaults for lightweight environments.

#### **Architecture**

![[how-it-works-k3s-revised-9c025ef482404bca2e53a89a0ba7a3c5.svg]]


x----------------------------------------------------------------------------------------------------x

# K8s Architecture

Source -> https://devopscube.com/kubernetes-architecture-explained/

![[Pasted image 20230823112310.png]]

## K8s Control Plane Components

### 1. kube-apiserver

- The **kube-api server** is the central hub of the Kubernetes cluster that exposes the Kubernetes API.
-  when you use kubectl to manage the cluster, at the backend you are actually communicating with the API server through **HTTP REST APIs**.
- the internal cluster components like the scheduler, controller, etc talk to the API server using [gRPC](https://grpc.io/docs/what-is-grpc/introduction/).
- gRPC is a high-performance, open-source remote procedure call (RPC) framework that can be used to create efficient and scalable distributed applications. It is based on the HTTP/2 protocol.
- Remote Procedure Call is a software communication protocol that one program can use to request a service from a program located in another computer on a network without having to understand the network's details.
- The communication between the API server and other components in the cluster happens over TLS.

### 2. etcd
- Kubernetes is a distributed system and it needs an efficient distributed database like etcd that supports its distributed nature. It acts as both a backend service discovery and a database.
- etcd stores all configurations, states, and metadata of Kubernetes objects
- Kubernetes api-server uses the etcd’s watch functionality to track the change in the state of an object.
- etcd stores all objects under the **/registry** directory key in key-value format. For example, information on a pod named Nginx in the default namespace can be found under **/registry/pods/default/nginx**
![[Pasted image 20230824102206.png]]


### 3. kube-scheduler

- responsible for **scheduling pods on worker nodes**.
- When you deploy a pod, you specify the pod requirements such as CPU, memory, affinity, taints or tolerations, priority, persistent volumes (PV),  etc. The scheduler’s primary task is to identify the create request and choose the best node for a pod that satisfies the requirements.
![[Pasted image 20230824102454.png]]

How it works?
1. To choose the best node, the Kube-scheduler uses **filtering and scoring** operations.
2. In **filtering**, the scheduler finds the best-suited nodes where the pod can be scheduled. For example, if there are five worker nodes with resource availability to run the pod, it selects all five nodes. If there are no nodes, then the pod is unschedulable and moved to the scheduling queue. If It is a large cluster, let’s say 100 worker nodes, and the scheduler doesn’t iterate over all the nodes. There is a scheduler configuration parameter called `**percentageOfNodesToScore**`. The default value is typically **50%**. So it tries to iterate over 50% of nodes in a round-robin fashion. If the worker nodes are spread across multiple zones, then the scheduler iterates over nodes in different zones. For very large clusters the default `**percentageOfNodesToScore**` is 5%.
3. In the **scoring phase**, the scheduler ranks the nodes by assigning a score to the filtered worker nodes. The scheduler makes the scoring by calling multiple [scheduling plugins](https://kubernetes.io/docs/reference/scheduling/config/#scheduling-plugins). Finally, the worker node with the highest rank will be selected for scheduling the pod. If all the nodes have the same rank, a node will be selected at random.
4. Once the node is selected, the scheduler creates a binding event in the API server. Meaning an event to bind a pod and node.

### 4. Kube Controller Manager
- [Controllers](https://kubernetes.io/docs/concepts/architecture/controller/) are programs that run infinite control loops. Meaning it runs continuously and watches the actual and desired state of objects. If there is a difference in the actual and desired state, it ensures that the kubernetes resource/object is in the desired state.
- **Kube controller manager** is a component that manages all the Kubernetes controllers. Kubernetes resources/objects like pods, namespaces, jobs, replicaset are managed by respective controllers. Also, the kube scheduler is also a controller managed by Kube controller manager.
![[Pasted image 20230824104238.png]]

### 5. Cloud Controller Manager (CCM)

- When kubernetes is deployed in cloud environments, the cloud controller manager acts as a bridge between Cloud Platform APIs and the Kubernetes cluster.
- This way the core kubernetes core components can work independently and allow the cloud providers to integrate with kubernetes using plugins. (For example, an interface between kubernetes cluster and AWS cloud API)
- Cloud controller integration allows Kubernetes cluster to provision cloud resources like instances (for nodes), Load Balancers (for services), and Storage Volumes (for persistent volumes).

![[Pasted image 20230824105318.png]]

1. Deploying Kubernetes Service of type Load balancer. Here Kubernetes provisions a Cloud-specific Loadbalancer and integrates with Kubernetes Service.
2. Provisioning storage volumes (PV) for pods backed by cloud storage solutions

Overall **Cloud Controller Manager manages the** lifecycle of cloud-specific resources used by kubernetes.


Following are the three main controllers that are part of the cloud controller manager.

1. **Node controller:** This controller updates node-related information by talking to the cloud provider API. For example, node labeling & annotation, getting hostname, CPU & memory availability, nodes health, etc.
2. **Route controller:** It is responsible for configuring networking routes on a cloud platform. So that pods in different nodes can talk to each other.
3. **Service controller**: It takes care of deploying load balancers for kubernetes services, assigning IP addresses, etc.


## Kubernetes Worker Node Components

### 1. Kubelet

- Kubelet is an agent component that runs on every node in the cluster. it does not run as a container instead runs as a daemon, managed by systemd.
- It is responsible for registering worker nodes with the API server and working with the podSpec (Pod specification – YAML or JSON) primarily from the API server. podSpec defines the containers that should run inside the pod, their resources (e.g. CPU and memory limits), and other settings such as environment variables, volumes, and labels.
- To put it simply, kubelet is responsible for the following.

1. Creating, modifying, and deleting containers for the pod.
2. Responsible for handling liveliness, readiness, and startup probes.
3. Responsible for Mounting volumes by reading pod configuration and creating respective directories on the host for the volume mount.
4. Collecting and reporting Node and pod status via calls to the API server.

- Kubelet is also a controller where it watches for pod changes and utilizes the node’s container runtime to pull images, run containers, etc.

- Other than PodSpecs from the API server, kubelet can accept podSpec from a file, HTTP endpoint, and HTTP server. A good example of “podSpec from a file” is Kubernetes static pods.

- Static pods are controlled by kubelet, not the API servers.

- This means you can create pods by providing a pod YAML location to the Kubelet component. However, static pods created by Kubelet are not managed by the API server.

- Here is a real-world example use case of the static pod.

- While bootstrapping the control plane, kubelet starts the api-server, scheduler, and controller manager as static pods from podSpecs located at `/etc/kubernetes/manifests`

Following are some of the key things about kubelet.

1. Kubelet uses the CRI (container runtime interface) gRPC interface to talk to the container runtime.
2. It also exposes an HTTP endpoint to stream logs and provides exec sessions for clients.
3. Uses the CSI (container storage interface) gRPC to configure block volumes.
4. It uses the CNI plugin configured in the cluster to allocate the pod IP address and set up any necessary network routes and firewall rules for the pod.

![[Pasted image 20230824112438.png]]

### 2. Kube Proxy 

- To understand kube proxy, you need to have a basic knowledge of Kubernetes Service & endpoint objects.

- Service in Kubernetes is a way to expose a set of pods internally or to external traffic. When you create the service object, it gets a virtual IP assigned to it. It is called clusterIP. It is only accessible within the Kubernetes cluster.

- The Endpoint object contains all the IP addresses and ports of pod groups under a Service object. The endpoints controller is responsible for maintaining a list of pod IP addresses (endpoints). The service controller is responsible for configuring endpoints to a service.

- Kube-proxy is a daemon that runs on every node as a [daemonset](https://devopscube.com/kubernetes-daemonset/). It is a proxy component that implements the Kubernetes Services concept for pods. (single DNS for a set of pods with load balancing). It primarily proxies UDP, TCP, and SCTP and does not understand HTTP.

- When you expose pods using a Service (ClusterIP), Kube-proxy creates network rules to send traffic to the backend pods (endpoints) grouped under the Service object. Meaning, all the load balancing, and service discovery are handled by the Kube proxy.
![[Pasted image 20230824113542.png]]

**So how does Kube-proxy work?**

Kube proxy talks to the API server to get the details about the Service (ClusterIP) and respective pod IPs & ports (endpoints). It also monitors for changes in service and endpoints.

Kube-proxy then uses any one of the following modes to create/update rules for routing traffic to pods behind a Service

1. **[IPTables](https://wiki.centos.org/HowTos/Network/IPTables)**: It is the default mode. In IPTables mode, the traffic is handled by IPtable rules. In this mode, kube-proxy chooses the backend pod random for load balancing. Once the connection is established, the requests go to the same pod until the connection is terminated.
2. **[IPVS](https://en.wikipedia.org/wiki/IP_Virtual_Server):** For clusters with services exceeding 1000, IPVS offers performance improvement. It supports the following load-balancing algorithms for the backend.
3. **Userspace** (legacy & not recommended)
4. **Kernelspace**: This mode is only for windows systems.


### 3. Container Runtime

- container runtime is a software component that is required to run containers.
- Container runtime runs on all the nodes in the Kubernetes cluster. It is responsible for pulling images from container registries, running containers, allocating and isolating resources for containers, and managing the entire lifecycle of a container on a host.

- Two Main Concepts :->
1. **Container Runtime Interface (CRI):** It is a set of APIs that allows Kubernetes to interact with different container runtimes. It allows different container runtimes to be used interchangeably with Kubernetes. The CRI defines the API for creating, starting, stopping, and deleting containers, as well as for managing images and container networks.
2. **Open Container Initiative (OCI):** It is a set of standards for container formats and runtimes

- Kubernetes supports multiple [container runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) (CRI-O, Docker Engine, containerd, etc) that are compliant with **[Container Runtime Interface (CRI)](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md)**. This means, all these container runtimes implement the CRI interface and expose gRPC [CRI APIs](https://kubernetes.io/docs/concepts/architecture/cri/) (runtime and image service endpoints).

- the kubelet agent is responsible for interacting with the container runtime using CRI APIs to manage the lifecycle of a container.
- It also gets all the container information from the container runtime and provides it to the control plane.
![[Pasted image 20230824114000.png]]

1. When there is a new request for a pod from the API server, the kubelet talks to CRI-O daemon to launch the required containers via Kubernetes Container Runtime Interface.
2. CRI-O checks and pulls the required container image from the configured container registry using **`containers/image`** library.
3. CRI-O then generates OCI runtime specification (JSON) for a container.
4. CRI-O then launches an OCI-compatible runtime (runc) to start the container process as per the runtime specification.


## Kubernetes Object

Source -> https://kubernetes.io/docs/concepts/overview/working-with-objects/

- Kubernetes uses these Object to represent the state of your cluster
- Specifically, they can describe:
	
	- What containerized applications are running (and on which nodes)
	- The resources available to those applications
	- The policies around how those applications behave, such as restart policies, upgrades, and fault-tolerance

- For example: in Kubernetes, a Deployment is an object that can represent an application running on your cluster.
- When you create an object in Kubernetes, you must provide the object spec that describes its desired state, as well as some basic information about the object (such as a name).
- **Most often, you provide the information to `kubectl` in a .yaml file.**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80

```

```shell
kubectl apply -f /deployment.yaml
```


## Kubernetes Namespace

- _namespaces_ provides a mechanism for isolating groups of resources within a single cluster.
- Namespaces are intended for use in environments with many users spread across multiple teams, or projects.

```shell
# Check already existing Namespaces
kubectl get ns

# TO run a image with a specific namespace
kubectl run nginx --image=nginx -n dev
# 'dev' is name of namespace
```

```shell
# Like When You Run
kubectl get pods
# it will all pods with namespace='default'
# to change the default namespace to 'dev'
kubectl config set-context --current --namespace=dev
```

## Kubernetes Labels and Selectors

- Labels enable users to map their own organizational structures onto system objects in a loosely coupled fashion, without requiring clients to store these mappings.
- Example labels:

- `"release" : "stable"`, `"release" : "canary"`
- `"environment" : "dev"`, `"environment" : "qa"`, `"environment" : "production"`
- `"tier" : "frontend"`, `"tier" : "backend"`, `"tier" : "cache"`


```yaml
metadata:
  name: label-demo
  labels:
    environment: production
    app: nginx
```

```shell
# To get Existing Labels
kubectl get pods --show-labels

# To add label 'demo=true'
kubectl label pod nginx demo=true
```


- The API currently supports two types of selectors: _equality-based_ and _set-based_.  



# Kubernetes configuration and security

## ConfigMaps
- A ConfigMap is an API object used to store non-confidential data in key-value pairs. [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a [volume](https://kubernetes.io/docs/concepts/storage/volumes/).

### Creating ConfigMaps

```shell
kubectl create configmap test --from-file=file.prop

kubectl describe cm test

kubectl create configmap test2 --from-env-file=env.prop
```

## Secrets

- A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key.
```shell
kubectl get secrets

kubectl create secret generic admin --from-literal=admin-user=admin

kubectl create secret generic dev --from-literal=dev-user=dev

kubectl logs secrets-env
```


## Kubernetes Access Control

![[Pasted image 20230831105419.png]]


## Kubernetes Authentication

![[Pasted image 20230831110237.png]]


- the regular user's credential is not checked by k8s. it is checked by a external management system.
- Service account are k8s objects and k8s handles them.
- they are created as a k8s object. each is associated with secret, and secret have a token which is used for auth.

#### How to Authenticate a Kubernetes Cluster -> https://www.youtube.com/watch?v=Ux4tNEbG_4w


## Authorization in K8s

![[Pasted image 20230831111340.png]]

- Node Authorization - special type of special purpose authorization that allows the k8s to perform API operations.
- ABAC - which is attribute based access control. you also need to specify the attribute based authorization policy file. policy file is JSON based and it is where each line is a policy object.
- RBAC - role based access control. you need to create a role then assign it to a particular service account or user. contains three things -> User, verb and object.
- Webhook - external service called by the API server and then it decides whether the request is allowed or not. simple HTTP callback when something is changed.

### RBAC Authorization

- To enable RBAC, start the [API server](https://kubernetes.io/docs/concepts/overview/components/#kube-apiserver) with the `--authorization-mode` flag set to a comma-separated list that includes `RBAC`; for example:
```shell
kube-apiserver --authorization-mode=Example,RBAC --other-options --more-options
```

![[Pasted image 20230831113253.png]]

#### demo 

```shell
kubectl create ns demo
kubectl create sa sam -n demo
kubectl create clusterrole cr --verb=get,list,watch,delete --resource=secrets,pods,deployments
kubectl create rolebinding super --serviceaccount=demo:sam -n demo --clusterrole=cr
kubectl run demo --image=nginx --serviceaccount=sam -n demo
```
