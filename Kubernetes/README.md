# Kubernetes
This is my own personal documentation of the common Docker commands. <br>As most OS are Linux in DevOps, this in the context of Linux (Ubuntu). \
Updated: Apr 2022

</br>

## Table of Contents
> - [Understanding Kubernetes](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#understanding-kubernetes)
> - [Setting Up Kubernetes](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#setting-up-kubernetes)
>   - [Installing Kubectl](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#installing-kubectl)
>   - [Installing Minikube](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#installing-minikube)
>   - [Installing Docker](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#installing-docker)
> - [Kubernetes Resources](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#kubernetes-resources)

</br>

## Understanding Kubernetes
Kubernetes is a container orchestration tool. As most applications are now moving towards containerization, managing multiple or a large number of containerized applications becomes difficult or even impossible. Kubernetes is able to help with managing these large number of containers with ease.

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes%20(WIP)#table-of-contents)

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

## Kubernetes Resources
### Cluster
A Kubernetes cluster is a set of nodes that run containerized applications. We need to create a cluster first before we can do anything within the cluster.
> Do note that we are using Minikube here. This is a test environment and not a production environment.

</br>

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

### Namespaces
We can categorize different resources in different namespaces. This allows us to better segregated applications and resources. 

We can create a namespace using the command line:
```
kubectl create namespace [namespace-name]
```
We can also use a YAML file to create a namespace: \
E.g \
Create a YAML file called namespace.yml
```
vi namespace.yml
```
Copy the following below:
```
---
apiVersion: v1
kind: Namespace
metadata:
  name: development
```
To create the namespace from the YAML file, run this command:
```
kubectl apply -f namespace.yml
```
To check if the namespace has been successfully created:
```
kubectl get namespaces
```

</br>

### Pods
Pods are the smallest units in Kubernetes. A single Pod can consist of one or more containerized applications. Containers inside a pod tend to serve the same goal.

We can create a pod using the command line:
```
kubectl run [pod-name] --image=[image-name] --restart=Never
```
We can also use a YAML file to create a pod: \
E.g \
Create a YAML file called pod.yml
```
vi pod.yml
```
Copy the following below:
```
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: development
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```
To create the pod from the YAML file, run this command:
```
kubectl apply -f pod.yml
```

</br>

### Deployments
Deployments provide declarative updates for pods. A deployment is typically a YAML file that is used to declare the configurations of a single pod. This pod can be replicated to as many replicas necessary. Any changes to this deployment YAML file will be used to update the pods under this deployment. We can also delete an entire deployment as well.

Deployment YAML files will contain information by which, the deployment will know which pods belongs to which deployment using metadata. This connection is made using the labels and matchLabels key-value pairs.

We can create a deployment using the command line:
```
kubectl create deployment [deployment-name] --image=[image-name]
```

We can also use a YAML file to create a deployment: \
E.g \
Create a YAML file called deployment.yml
```
vi deployment.yml
```
Copy the following below:
```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
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
To create the deployment from the YAML file, run this command:
```
kubectl apply -f deployment.yml
```
If there are any changes made to the original deployment.yml, we can use the same apply command to apply the updates to the deployment.

</br>

### Service
Each pod upon creation is given an IP address and this IP address is used for pods to talk to one another. However, pods are ephemeral. They die easily and when a pod is newly created again, the IP address changes and this is troublesome. So we use Service.

Service is a resource that consolidates all the pods that serves the same function and collates their IP addresses in one place. If a pod dies and a new pod is created, the Service will be able to remove the old unused IP address and add the new IP address inside.

Using Service and replicas, we are able to direct connections to existing replicas while a new one is recreating. Usually, a service YAML file is written together in the same file as the deployment YAML file. Using metadata, the service will know which deployment it should be connected to as well. This connection is made using the selector and labels key-value pairs.

Create a YAML file named service.yml
```
vi service.yml
```
Copy the following below:
```
---
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```
To create the deployment from the YAML file, run this command:
```
kubectl apply -f service.yml
```

In the example above, the port 80 belongs to the service and the targetPort 9376 


</br>

## Getting Status of Different Components
```
kubectl get nodes
```
```
kubectl get pods
```
```
kubectl get services
```
```
kubectl get deployment
```
> pod-name/deployment-name allows us to target a component more specifically \
> -A allows us to get information of everything \
> -n <namespace> allows us to get information in a specific namespace \
> -o wide gives us more details compared to the basic details \
> -o yaml shows us the YAML file relating to that particular component

</br>

## Create a Pod/Deployment
```
kubectl run <pod-name> --image=<imagename>
```
> You can also add -n <namespace> to specify where the pod should be created.
Creating a pod, we can use a YAML file as well.
```
vi sample-pod.yml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```
```
kubectl apply -f sample-pod.yml
```

Using the command line, we can create a deployment. Pods can also be created via deployment.
```
kubectl create deployment <deployment-name> --image=<imagename>
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
>   template:
>     metadata:
>       labels:
>         app: nginx
>     spec:
>       containers:
>       - name: nginx
>         image: nginx:1.16
>         ports:
>         - containerPort: 80
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
```
kubectl get events
```
> If necessary, do include -n <namespace> for better identification of which specific pod you are targetting.
