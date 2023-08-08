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
