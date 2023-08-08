

![K8s](https://kubernetes.io/images/kubernetes-horizontal-color.png)

  



### Why do we use docker?

- Docker is a containerization platform that packages your application and all its dependencies together in the form of containers so as to ensure that your application works seamlessly in any environment be it development or test or production.

  

### Configuration Management

  

![Configuration Management](https://wac-cdn.atlassian.com/dam/jcr:6bcf8795-5881-4e87-9cfe-4b3cd07cf64c/ConfigurationManagement_@2x.png?cdnVersion=1127)

  
  

### Monolithic Applications

- A monolithic architecture is a traditional model of a software program, which is built as a unified unit that is self-contained and independent from other applications.

  

![Monolithic Applications](https://wac-cdn.atlassian.com/dam/jcr:95b9a276-c524-42b1-8d06-ded56d589858/Monolithic%20architecture@2x.png?cdnVersion=1127)

  

### Microservices

- Microservices is an architectural method that relies on a series of independently deployable services.

- These services have their own business logic and database with a specific goal. Updating, testing, deployment, and scaling occur within each service.

  

![Microservices](https://wac-cdn.atlassian.com/dam/jcr:5308ccab-dc94-46f5-978c-8a77b8d5be57/Microservice%20architecture@2x.png?cdnVersion=1127)

  
  

### Orchestrators

- An orchestrator is a tool that automates and manages your containers, including how they communicate with each other.

- Helps us in deploying and managing containers dynamically.

---------------------------------------

- Zero - Downtime deployments-------  C

- Scale-----------------------------------  N

- Self-Heal Containers------------------  C

- Load Balancing  -----------------------  F

-----------------------------------Projects

  

### Kubernetes

- automates operational tasks of container management

- deploying applications

- rolling out changes

- Run it on your cloud or locally.

- you can migrate from one provider to another.

- replicate and scale services.

- fault tolerance and self healing.

- you can use volumes like external hard disks.

- load balancing.

- logs and service discovery.\

- using secrets you can store secret passwords and keys.

- it is much more than a orchestrators.

  

### K8s vs Docker

- Docker lets you put everything you need to run your application into a box that can be stored and opened when and where it is required. Once you start boxing up your applications, you need a way to manage them; and that's what Kubernetes does.

  

- Kubernetes is a Greek word meaning  ‘captain’ in English. Like the captain is responsible for the safe journey of the ship in the seas, Kubernetes is responsible for carrying and delivering those boxes safely to locations where they can be used.

- Kubernetes can be used with or without Docker

  

### Cluster -> Control Plane + Nodes

![Cluster](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

- Nodes - Basically a server

- Control Plane - The control plane manages the worker nodes and the Pods in the cluster.

- Worker Node host the Pods that are the components of the application workload.

- Pods - set of running containers in cluster.

  

### Control Plane Components

  

- kube-apiserver

- etcd

- kube-scheduler

- kube-controller-manager

- cloud-controller-manager

  
  

### Node Components

  

- kubelet

- kube-proxy

- Container runtime

  

### KubeCTL - Kubernetes Command Line Tool

### Pod

### Control Plane

### kubernetes architecture