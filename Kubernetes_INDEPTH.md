
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


## K8s Volumes : 
### need for volumes :
suppose you have a sql database in pod, when you restart pod every time, the existing db will be deleted, so we volume is used.

- storage that doesn't depend on pod lifecycle.
- writes/reads directory.

## Persistent Volume
- a cluster resource
- created via YAML file.
- kind : PersistentVolume 

### Where does this storage come from and who makes it available?
- you need to create and manage them by yourself.

YAML Example for NFS Storage : 
![[Pasted image 20230909110105.png]]

- depending on storage type,  spec attributes differ.
- Persistent volumes are not namespaced. they just available to whole cluster.


### Who creates the PV and When ?
- K8s admin sets up and maintains the cluster. and also make sure that the cluster have enough resources. storage resource is provisioned by Admin.
- K8s users deploys the app in cluster. users need to claim them using pvc(kind:PersistentVolumeClaim).

- we have to claim the pvc to use the volume : 
![[Pasted image 20230909110947.png]]

### Levels of Volume abstractions
- Pod requests the volume through the PV claim.
- Claim tries to find a volume in cluster
- Volume has the actual storage backend
- Claims must be in the same namespace!
- Volume is mounted into the pod.
- Volume is the mounted into container.


- you can use multiple and different types in 1 pod.


### Storage Class
- SC provisions Persistent Volumes dynamically.. ..when PersistentVolumeClaim claims it
- creates pv dynamically in background.


```yaml
apiVersion: storage.k8s.io/v1
kind: Storage Class
metadata:
name: storage-class-name
provisioner: kubernetes.io/aws-ebs
parameters:
type: iol
iopsPerGB: "10"
fsType: ext4
```



![[Pasted image 20230909112135.png]]



## Kubernetes Objects

### Pods
- smallest unit in K8s. it is a group of one or more containers.

![[Pasted image 20230909142054.png]]

```yml
# To check existing Pods
kubectl get pods
kubectl get pods -owide

# To Create Pod
kubectl create -f sample-pod.yml

# describe
kubectl describe pod pod_name

# delete
kubectl delete pod pod_name

#execute pod
kubectl exec -it pod_name bash
```

### The Pod's Lifecycle
![[Pasted image 20230909143211.png]]


### Init Containers - TBC

### How to Create multi-container pod?

![[Pasted image 20230910100309.png]]

Spec.YML
```yml
apiVersion: v1
kind: Pod
metadata:
name: multi-container
spec:
restartPolicy: Never
volumes:
name: shared-data
emptyDir: {} I
containers:
name: nginx-container
image: nginx
volumeMounts:
- name: shared-data
mountPath: /usr/share/nginx/html
name: alpine
image: alpine
volumeMounts:
- name: shared-data
mountPath: /mem-info
command: ["/bin/sh", "-c"]
args:
- while true; do
	date >> /mem-info/index.html ;
	egrep --color 'Mem | Cache | Swap' /proc/meminfo >> /mem-info/index.html ;
	sleep 2;
	done
```

```bash
kubectl apply -f multi-container.yml

kubectl exec -it multi-container -c nginx-container sh
```


### Container Probes : 

![[Pasted image 20230910101758.png]]

- To delay traffic until the pod is ready.
- Readiness probe is mostly used to check dependencies of the pod is up there.
- Liveness probe - check pod is live or it will restart.
- Startup probe - special type of liveness probe, runs first after halting  every other probes.

```yml
apiVersion: v1
kind: Pod
metadata:
creation Timestamp: null
labels:
run: nginx
name: nginx
spec:
containers:
	image: nginx
	name: nginx
livenessProbe:
httpGet:
path: /
port: 80
ports:
containerPort: 80
resources: {}
dns Policy: ClusterFirst
restartPolicy: Always
status: {}
```

### Container Resource

```yml
apiVersion: v1
kind: Pod
metadata:
name: cpu-mem-demo2
spec:
containers:
name: cpu-mem-demo
image: saiyam911/stress
resources:
limits:
#cpu: "3"
memory: "200Mi"
requests:
#cpu: "3"
memory: "100Mi¹"
command: ["stress"]
#args: ["--cpu", "3"]
args: ["--vm", "1", "--vm-bytes", "250M", "--vm-hang", "1"]
```

### K8s Deployments

```yml
kind: Deployment
metadata:
labels:
app: demo
name: demo
spec:
replicas: 3
selector:
matchLabels:
app: demo
template:
metadata:
labels:
app: demo
spec:
containers:
image: nginx
name: nginx
ports:
```

![[Pasted image 20230911120928.png]]

#### How the process works internally? (zero-downtime)
- 4th replica of a pod is created.
- Then, Liveness and readiness check runs.
- after successfully passing the checks, pod is added to load balancer.
- previous pods is deleted, but port still gets traffic due to termination grace period (usually 30 secs).

### Deployment

```bash
kubectl create deployment demo --image=nginx --replicas=3 --port-80

# Check rollout status
kubectl rollout status deployment demo

# Create new deplyment 
kubectl set image deployment/demo nginx=nginx:1.15.0 --record

# demo kubectl rollout status deployment demo
# Waiting for deployment "demo" rollout to finish: 2 out of 3 new replicas have been updated...
# Waiting for deployment "demo" rollout to finish: 2 out of 3 new replicas have been updated...
# Waiting for deployment "demo" rollout to finish: 2 out of 3 new replicas have been updated...
# Waiting for deployment "demo" rollout to finish: 1 old replicas are pending termination...
# Waiting for deployment "demo" rollout to finish: 1 old replicas are pending termination...

#deployment "demo" successfully rolled out

# Replica Set Status
kubectl get rs
```


##### roll back to previous 

```yml
kubectl rollout history deployment demo


kubectl rollout undo deployment demo --to-revision=2
```

### Kubernetes StatefulSets

- StatefulSet is the workload API object used to manage stateful applications.
- Manages the deployment and scaling of a set of [Pods](https://kubernetes.io/docs/concepts/workloads/pods/), _and provides guarantees about the ordering and uniqueness_ of these Pods.
- a StatefulSet maintains a sticky identity for each of its Pods. These pods are created from the same spec, but are not interchangeable: each has a persistent identifier that it maintains across any rescheduling.

![[Pasted image 20230911152205.png]]


```yml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx
  serviceName: "nginx"
  replicas: 3
  minReadySeconds: 10
  template:
    metadata:
      labels:
        app: nginx
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: nginx
        image: registry.k8s.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "my-storage-class"
      resources:
        requests:
          storage: 1Gi
```


### Kubernetes DaemonSets
A _DaemonSet_ ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

- running a cluster storage daemon on every node
- running a logs collection daemon on every node
- running a node monitoring daemon on every node

In a simple case, one DaemonSet, covering all nodes, would be used for each type of daemon. A more complex setup might use multiple DaemonSets for a single type of daemon, but with different flags and/or different memory and cpu requests for different hardware types


```yml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-elasticsearch
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      # these tolerations are to have the daemonset runnable on control plane nodes
      # remove them if your control plane nodes should not run pods
      - key: node-role.kubernetes.io/control-plane
        operator: Exists
        effect: NoSchedule
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: quay.io/fluentd_elasticsearch/fluentd:v2.5.2
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log

```

### K8s Services
![[Pasted image 20230911191719.png]]
![[Pasted image 20230911191847.png]]
![[Pasted image 20230911191937.png]]

- To deploy a Kubernetes application to a Kubernetes cluster, we need to spin up pods on different nodes. A pod contains the running container of the application, which can be a Docker container or any other container. To make the application more resilient, we should have pods running on all nodes, at least multiple pods. We can use pure Kubernetes manifests, templating solutions like customize, or home charts to spin up the application. While other tools like developer platforms may be used, we will focus on Kubernetes manifests and pure Kubernetes manifests for this video.
- The pod will spin up when the pod dies, and Kubernetes reconciles resources by specifying the number of pods to run within the cluster. Deployments allow for the creation of a replica set of two pods, ensuring that at any point in time, at least one pod of the application is running. The selector and template specify the container image, and the label "node application" is a key value. To check the pods, use the command "kubectl get all -n example" and verify the number of pods. If the pod dies, the application can be accessed through localhost 8080. However, if the pod dies, the application will not automatically reconnect to another pod.
- A Kubernetes service is deployed within a deployment to dynamically connect to different pods. This allows for dynamic routing and forwarding if a pod dies. In a front-end application, services are spun up between different sections, with one service assigned to the front end and another to the database. This allows for communication between different pods within the deployment, independent of endpoints or node nodes. This approach ensures smoother flow and avoids tight coupling between different pods.
- Kubernetes services come in three types: NodePort, LoadBalancer, and Headless Service. To spin up a service that routes traffic to deployments, use the kubectl apply command. The service and node application are accessible through port 3000. Open port 3000 and the application should open again. To delete a pod, use the kubectl delete pod command. A new pod is spun up, and access can be restored through localhost 3000.



### ClusterIP
##### using namespace everytime just for tut

ClusterIP is the most common type of Kubernetes service. Now, ClusterIP is the default type. So, if you don't specify any type, this will be used. In ClusterIP, our endpoint, our service, will only be accessible from within the cluster, not from the outside world. ClusterIP is not used to route traffic from the outside world, only traffic within the cluster. So, anything happening within a cluster can be routed through ClusterIP.

```yml
kubectl get all -n example

kubectl get endpoints -n example
```

If we use the command `kubectl get pods -o wide -n example`, we can see the IP addresses for our two different pods.

The pods have endpoints and their own internal IP address. And the service can discover these endpoints. Now, the endpoints and the application are running on port 8080. So, that's why you see port 8080 in the endpoints. But the service itself is accessible through port 3000. So, how is that configured? And how does the service discover these endpoints?

![[Pasted image 20230911220152.png]]

When I specify targetport: 8080, it has taken the name. So, I'm targeting port 8080. So, let's add this to the service. Now, this is the same as my containerPort in this case. So, the targetPort is targeting the containerPort here. So, what I do is I port-forward to my localhost.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
name: node-application
spec:
replicas: 2
selector:
matchLabels:
run: node-application
template:
metadata:
labels:
run: node-application
spec:
containers:
- name: node-application
image: anaisurlichs/node-example:1.0.0
ports:
containerPort: 8080
imagePullPolicy: Always
```




#### Nodeport
- So now, in the case of ClusterIP, our IP address will only be accessible from within the nodes within our cluster, they won't be accessible cluster-wide, and this is where type NodePort comes in.
- So, in most cases, you want Kubernetes to choose the NodePort for you within the different range, and then you can configure external traffic.
- let's say there is traffic from the outside world, and we want to route the traffic into our cluster. We can then set up our load-balancing traffic routing solution, and they will all connect to our NodePort here.
- Now, NodePort is probably the type used least in this case. This is because you would want to use ClusterIP for most cases. 
- However, if you want to set up a custom solution, you can use the type NodePort to route the traffic. So each of those nodes will have this specific port as they are NodePort defined. And this way, the service will know how to connect to those different nodes dynamically.

#### Headless Service

`kubectl get all -n example`

If we want to specify Headless Service, we will not specify any type. We would set it to type none, then spin up a Headless Service. Now, Headless Service has more advanced use cases and specific use cases that allow us to customize a lot and use resources that are not Kubernetes-specific to connect to the service. The services that we discussed within this video are specific to Kubernetes, but in some cases, you might want to set up custom solutions that are not dependent on a particular type.

So, summarizing, type NodePort and type Headless Service allow us to set up more customized routing solutions for our service running within our Kubernetes cluster.

![[Pasted image 20230911222102.png]]


### Kubernetes Load Balancers 

![[Pasted image 20230911223029.png]]

A LoadBalancer, basically as a type-service, allows us to dynamically route the traffic between services to those different pods within those different nodes. So, we will be able to access from the outside world through an external IP address, our application running within our Kubernetes cluster.

going back to our terminal, as you can see, right now, we have the type NodePort running within our cluster. But, it's not a type LoadBalancer. So, we want to define within our resource that we want to change the type NodePort to type LoadBalancer. And now, we want to apply our updates, so we will use the command `kubectl apply -f manifests/service.yaml -n example`. And we want to apply this Service YAML file to our namespace example, where we have all our resources running right now.

We will now verify the updates using the `kubectl get all -n example` command. So, checking back on our resources, it should now be of type LoadBalancer. And before, we never saw an external IP address to find. In this case, we're seeing our external IP address defined, and this is our external IP address. Now we can use this address and port 3000 to access our application. That's the accessed cluster right through this port to connect to our different nodes and our different pods within our nodes. However, we want to access our application through the external IP through the load balancer that we've set up here.

These pods have been spun up in addition to the others to serve my LoadBalancer. So, I can go ahead and use kubectl to delete my pods. I'm going to delete it by using `kubectl delete [POD_NAME] -n example`, and I'm going to delete this pod in -n example. So, while this is deleting, I will still be able to access my application even though one of the pods has been deleted and spun up again because the load balancer type doesn't care.







-> for next time revising



### k8S Networking

- computer networking as it is
- ![[Pasted image 20230911225830.png]]
- ![[Pasted image 20230911225902.png]]


#### Container Networking

![[Pasted image 20230911230156.png]]

- Let's start by reminding ourselves what a node looks like in Kubernetes. It typically would have a virtual machine. 
- This virtual machine would have a kubelet running on top of it, and that kubelet would be scheduling containers via its Container Runtime Interface. 
- A container, as represented here, forms part of a pod, and a pod is a collection of one or more containers, a pod being the smallest atomic unit of deployment in Kubernetes.

#### How does container to container networking take place?

- But on top of this, we have a network namespace. Network namespace is the ability to partition the network layer into isolated stacks per process. Hence, we have a root network namespace, but what we're going to do is when we create a new pod, we define the pod as having its own network namespace.
- This also means the second part is that when we launch a container, we have to rely upon the docker command of the net container function to link these containers into the pod network namespace.


#### Pod to Pod Networking
 ![[Pasted image 20230911231056.png]]

- Both the pods have their own unique pod network namespaces, and both have to share the root network namespace.
- The first thing that's going to happen that you'll notice that is a change is that we're going to have a new type of device added. And this is called a veth pair.
- A veth pair is a virtual ethernet device that can be used like a patch cable to connect two different network spaces together. So I can have veth0 on the root network namespace, and I can have veth1 representing my second pod on the root network namespace, and this can go on for as many pods as you have running on your host.
- At this point, if I wish to send data between the two pods from one container to another, I still need to have a few more components. One of these is the L3 virtual Linux switch called a Linux bridge. The Linux bridge exists on the host as part of the root network namespace. It serves the purpose of inspecting the data frames from the incoming packets from the container and forwarding them to the correct address. Let's talk about that because there's a lot to decompress there. When this container wishes to look up the IP address of the adjacent pod to which it's connecting, the first thing that happens is it hits the ethernet connection, which goes through the veth pair. The veth pair then hits the bridge.
- At this point, the bridge will decapsulate the packet. Then, it will look at the data frame to inspect the target IP address and check to see if an IP address corresponds to a MAC address in its lookup table. As discussed in previous videos, the MAC address will be bound to a network interface. If it finds a receiver on this network namespace that matches the target, it will send on the forwarded data and add the MAC address if it doesn't exist. The way that it adds the MAC address is that it will perform a broadcast to every device on this network. Again, this is using the ARP protocol.


#### Node Networking

![[Pasted image 20230911231656.png]]




#### Container Network Interfaces (CNI)


what is CNI?
The reason that CNI is popular and why it was necessary is that Kubernetes doesn't provide its own networking implementation, it just defines the model, and it leaves us to fill the gaps and build that model out.

What is Flannel and how does it work?

Flannel is a CNI that provides a powerful solution for inter-host networking by creating a virtual network layer encapsulated in a VPC. It has a larger address range than the VPC level of the/19, with 65,536 addressable IPs in the Flannel pod network. Flannel can carve up at the host and machine levels, providing three layers of addressable IP ranges.

A flat overlay network is created inside a VPC on a cluster, allowing packets to be sent from one virtual machine or Kubernetes node to another in the cluster. The journey of sending IP packets to the destination accurately involves crossing the network namespace within the pod, going across the veth pair, and down to the L3 Linux bridge. This bridge forwards the packets through to flannel0, a TUN device simulating a network layer device.

When flannel0 is set up, flanneld sets up kernel routing table rules, dropping packets when they are identified as part of the wider/16 overlay network. The kernel lane attempts to route these packets but ultimately pushes them back up through the TUN0 device, which is forwarded instead to the flanneld daemon process. The daemon process queries etcd for the subnet range of the target host IP, and that subnet range can be resolved to the underlying VM VPC level address.

Flannel uses a UDP Send to the node in question, intercepted by the flannel daemon running there, performing inverse activity going down through the flannel0 tunnel into the kernel and back up. The routing table pushes the packets towards the local Docker bridge, moving up over the L3 into the veth, across the network namespace, and up into the container.