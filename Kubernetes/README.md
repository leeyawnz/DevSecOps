# Kubernetes
This is my own personal documentation of the common Docker commands. <br>As most OS are Linux in DevOps, this in the context of Linux (Ubuntu). \
Updated: Apr 2022

</br>

## Table of Contents
> - [Setting Up Kubernetes](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#setting-up-kubernetes)
>   - [Installing Kubectl](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#installing-kubectl)
>   - [Installing Minikube](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#installing-minikube)
>   - [Installing Docker](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#installing-docker)

</br>

## Setting Up Kubernetes
### Installing Kubectl
Do take note to install Kubectl as the ubuntu sudo permission user and not root.
```
sudo apt-get update
```
```
sudo apt-get install -y apt-transport-https ca-certificates curl
```
```
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
```
```
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
```
sudo apt-get update
```
```
sudo apt-get install -y kubectl
```
Checking if Kubectl is properly installed:
```
kubectl version --client
```
[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes%20(WIP)#table-of-contents)

### Installing Minikube
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
```
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes%20(WIP)#table-of-contents)

### Installing Docker
You can refer to the Docker installation [here](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#setting-up-docker). \
[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes%20(WIP)#table-of-contents)

</br>

## Creating and Starting a Cluster
We can start a cluster using this command:
```
minikube start
```
> Note: Minikube is used to start/delete cluster
To interact with the cluster, we use the kubectl command:
```
kubectl cluster-info
```
```
kubectl get pods -A
```
We can also check if a container is running:
```
docker ps
```

</br>

## Getting Status of Different Components
```
kubectl get nodes
```
```
kubectl get pods
```
> To get status of all pods,
> ```
> kubectl get pods -A
> ```
> To get status of pods in a specific namespace,
> ```
> kubectl get pods -n <namespace>
> ```
> To get more information of the pods,
> ```
> kubctl get pods -owide
> ```

```
kubectl get services
```

## Create a Pod/Deployment
Using the command line, we can create a deployment. Pods are created via this deployment.
```
kubectl create deployment <name> --image=<imagename>
```
> e.g:
> ```
> kubectl create deployment nginx-depl --image=nginx
> ```
However, it is more efficient to create a deployment by applying a deployment YAML file.
We can create/delete a deployment with the command below:
```
kubectl apply -f <config-file-name>.yml
```
```
kubectl delete -f <config-file-name>.yml
```
> e.g: \
> Create a file called nginx-deployment.yml
> ```
> vi nginx-deployment.yml
> ```
> Inside the file, copy the below information.
> ```
> apiVersion: apps/v1
> kind: Deployment
> metadata:
>   name: nginx-deployment
>   labels:
>     app: nginx
> spec:
>   replicas: 1
>   selector:
>     matchLabels:
>       app: nginx
>     template:
>       metadata:
>         labels:
>           app: nginx
>         spec:
>           containers:
>           - name: nginx
>             image: nginx:1.16
>             ports:
>             - containerPort: 80
> ```
> Let's apply the configuration.
> ```
> kubectl apply -f nginx-deployment.yml
> ```
> > We should get an output that says "deployment.apps/nginx-deployment created"
> 
> If the nginx-deployment.yml file has been changed and we run the apply command again, Kubernetes knows whether to create a new deployment or to update an existing deployment.
> > If an update is applied, we should get an output that says "deployment.apps/nginx-deployment configured"

</br>

## Debugging a Pod
```
kubectl logs <pod-name>
```
```
kubectl describe pod <pod-name>
```
```
kubectl exec -it <pod-name> -- bin/bash
```
