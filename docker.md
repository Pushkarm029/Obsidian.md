# Docker

![Docker](https://cdn.hashnode.com/res/hashnode/image/upload/v1642773221418/7A9XnkEAEb.png?w=1600&h=840&fit=crop&crop=entropy&auto=compress,format&format=webp)

inspired by [this](https://blog.wemakedevs.org/getting-started-with-docker)

### Container
A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.


### Virtual Machines
Virtual machines are heavy software packages that provide complete emulation of low level hardware devices like CPU, Disk and Networking devices. 


### Containers vs Virtual Machines

![IMAGE](https://wac-cdn.atlassian.com/dam/jcr:92adde69-f728-4cfc-8bab-ba391c25ae58/SWTM-2060_Diagram_Containers_VirtualMachines_v03.png?cdnVersion=1127)

### Docker
Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security allows you to run many containers simultaneously on a given host. 

### Docker Runtime
- Basically Allows us to start and stop containers.
- Low Level Runtime - RunC - Tp work with the operating system to start and stop the containers.
- Higher Level - Containerd - helps in managing RunC, interact container with internet.

![Runtime](https://miro.medium.com/v2/resize:fit:828/format:webp/1*c3AiZFHuib7FUGyINzkEag.png)


### Docker Engine 
- used to interact with docker.
-  Docker Engine acts as a client-server application with:
   - A server with a long-running daemon process dockerd.
   - APIs which specify interfaces that programs can use to talk to and instruct the Docker daemon.
   - A command line interface (CLI) client docker.


### Orchestration

- orchestration is the automated configuring, coordinating, and managing of computer systems and software.
- Many tools exist to automate server configuration and management, including Kubernetes, Ansible, Puppet, Salt, Terraform and AWS CloudFormation.
### Docker / Container Image
- A Docker image is a file used to execute code in a Docker container.
- Docker images act as a set of instructions to build a Docker container, like a template.
-  A Docker image contains application code, libraries, tools, dependencies and other files needed to make an application run. When a user runs an image, it can become one or many instances of a container.

**Basic Commands**

```bash
$ docker pull ubuntu:18.04 (18.04 is tag/version (explained below))
$ docker images (Lists Docker Images)
$ docker run image (creates a container out of an image)
$ docker rmi image (deletes a Docker Image if no container is using it)
$ docker rmi $(docker images -q) (deletes all Docker images)

```

**Listing Hash Values of Docker Images**

```bash
$ docker images -q --no-trunc
sha256:3556258649b2ef23a41812be17377d32f568ed9f45150a26466d2ea26d926c32
sha256:9f38484d220fa527b1fb19747638497179500a1bed8bf0498eb788229229e6e1
sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e
```

**Docker Images**

```bash
$ docker images
REPOSITORY   TAG     IMAGE ID        
ubuntu       18.04   3556258649b2   
centos       latest  9f38484d220f    
hello-world  latest  fce289e99eb9
```


### Docker File

![dsd](https://dev-to-uploads.s3.amazonaws.com/i/o2dmqkdzcscl4atbhoj3.png)

- Describes steps to create a Docker image. 
- It’s like a recipe with all ingredients and steps necessary in making your dish.
- These images can be pulled to create containers in any environment.

**Steps To Create Docker File**

- Create a file named ‘Dockerfile’
- By default on building, docker searches for ‘Dockerfile’

```bash  
$ docker build -t myimage:1.0
```

- During building of the image, the commands in RUN section of Dockerfile will get executed. 
  

```bash 
$ docker run ImageID
```

- The commands in CMD section of Dockerfile will get executed when you create a container out of the image.

```bash
FROM ubuntu
MAINTAINER kunal kushwaha <kunal@gmail.com>
RUN apt-get update
CMD [“echo”, “Hello World”]
```

### Difference b/w Dockerfile and Image
![diff](https://lh6.googleusercontent.com/H8mhf23JNy-zCPrLaNs_H4h6K1xLRHv-P0JS4_Ad86xSo7En4tLT3POuOJPrcBNXG5lWDy2Y6fdNzRrzoB9SSLxrHhwrdk-qO28__D19NzO01OkkyBdr7YzZo2K_46HidAoUpmxeW2FOF42uOtAg3Pnfe_gcWafYs7xYywgdFeRdK3kV-p7LfIY7Z9h9tg)

- A Dockerfile is the Docker image’s source code. A Dockerfile is a text file containing various instructions and configurations.
- Images are read-only blueprints that include container-creation instructions. A Docker image is a container created to operate on the Docker framework.

### Open Container Initiative (OCI)

- The Open Container Initiative exists to create industry standards for container formats and runtimes.

- The image that Docker produces isn’t really a Docker-specific image—it’s an OCI (Open Container Initiative) image. Any OCI-compliant image, regardless of the tool you use to build it, will look the same to Kubernetes. Both containerd and CRI-O know how to pull those images and run them. This is why we have a standard for what containers should look like.

### Docker Archtechture
![arch](https://dev-to-uploads.s3.amazonaws.com/i/dmgpglab3k1z23g02cr5.png)

### Docker daemon
It listens to the API requests being made through the Docker client and manages Docker objects such as images, containers, networks, and volumes.

### Docker client
This is what you use to interact with Docker. When you run a command using docker, the client sends the command to the daemon, which carries them out. The Docker client can communicate with more than one daemon.

### Docker registries
This is where Docker images are stored. Docker Hub is a public registry that anyone can use. When you pull an image, Docker by default looks for it in the public registry and saves the image on your local system on DOCKER_HOST. You can also store images on your local machine or push them to the public registry.


### Containers Commands

```bash
  $ docker ps (list all containers)
  $ docker run ImageName/ID (checks locally for image, if not available it will go to registry and then go to DOCKER_HOST)
  $ docker start ContainerName/ID
  $ docker kill ContainerName/ID (Stops a running container) 
  $ docker rm ContainerName/ID (Deletes a stopped container)
  $ docker rm $(docker ps -a -q) (Delete all stopped containers)
```

### Docker Layers
When you run a build, the builder attempts to reuse layers from earlier builds. If a layer of an image is unchanged, then the builder picks it up from the build cache. If a layer has changed since the last build, that layer, and all layers that follow, must be rebuilt.


```bash
$ docker inspect ubuntu:18.04
“RootFS”: {
    “Type”: “layers”,
    “Layers”: [
“sha256:543791078bdb84740cb5457abbea10d96dac3dea8c07d6dc173f734c20c144fe”,
“sha256:c56e09e1bd18e5e41afb1fd16f5a211f533277bdae6d5d8ae96a248214d66baf”,
“sha256:a31dbd3063d77def5b2562dc8e14ed3f578f1f90a89670ae620fd62ae7cd6ee7”,
“sha256:b079b3fa8d1b4b30a71a6e81763ed3da1327abaf0680ed3ed9f00ad1d5de5e7c”
    ]
},
```

### Recap with Demo

- Pulling an image from Docker registry


```bash
$ docker images
REPOSITORY      TAG      IMAGE ID     CREATED      SIZE
$ docker pull ubuntu:18.04
18.04: Pulling from library/ubuntu
7413c47ba209: Pull complete
0fe7e7cbb2e8: Pull complete
1d425c982345: Pull complete
344da5c95cec: Pull complete
Digest:sha256:c303f19cfe9ee92badbbbd7567bc1ca47789f79303ddcef56f77687d4744cd7a
Status: Downloaded newer image for ubuntu:18.04
$ docker images
REPOSITORY     TAG        IMAGE ID          CREATED         SIZE
ubuntu         18.04      3556258649b2      9 days ago      64.2MB

```

- Running image to create container (-it stands for interactive mode)

```bash
$ docker run -it ubuntu:18.04
root@4183618bcf17:/# ls
bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var
root@4183618bcf17:/# exit
exit

```
- Listing all containers

```bash
$ docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS
4183618bcf17   ubuntu:18.04   “/bin/bash”   4 minutes ago   Exited
```