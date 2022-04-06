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
>   - [Cluster](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#cluster)
>   - [Namespaces](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#namespaces)
>   - [Pods](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#pods)
>   - [Deployments](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#deployments)
>   - [Services](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#services)
>   - [ConfigMaps](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#configmaps)
>   - [Secrets](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#secrets)
> - [Getting Status of Different Resources](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#getting-status-of-different-resources)
> - [Debugging a Pod](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#debugging-a-pod)

</br>

## Understanding Kubernetes
Kubernetes is a container orchestration tool. As most applications are now moving towards containerization, managing multiple or a large number of containerized applications becomes difficult or even impossible. Kubernetes is able to help with managing these large number of containers with ease.

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

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
[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

### Installing Minikube
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
```
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

### Installing Docker
You can refer to the Docker installation [here](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#setting-up-docker). \
[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

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
[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

</br>

### Namespaces
We can categorize different resources in different namespaces. This allows us to better segregated applications and resources. 

We can create a namespace using the command line:
```
kubectl create namespace [namespace-name]
```
We can also use a YAML file to create a namespace:
1. Create a YAML file called namespace.yml
```
vi namespace.yml
```
2. Copy the following below:
```
---
apiVersion: v1
kind: Namespace
metadata:
  name: development
```
3. To create the namespace from the YAML file, run this command:
```
kubectl apply -f namespace.yml
```
4. To check if the namespace has been successfully created:
```
kubectl get namespaces
```
[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

</br>

### Pods
Pods are the smallest units in Kubernetes. A single Pod can consist of one or more containerized applications. Containers inside a pod tend to serve the same goal.

We can create a pod using the command line:
```
kubectl run [pod-name] --image=[image-name] --restart=Never
```
We can also use a YAML file to create a pod:
1. Create a namespace
```
kubectl create namespace development
```
2. Create a YAML file called pod.yml
```
vi pod.yml
```
3. Copy the following below:
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
4. To create the pod from the YAML file, run this command:
```
kubectl apply -f pod.yml
```
5. To check if the pod has been successfully created:
```
kubectl get pods -n development
```
> NOTE: To create a pod in a specific namespace, the namespace must exist first.

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

</br>

### Deployments
Deployments provide declarative updates for pods. A deployment is typically a YAML file that is used to declare the configurations of a single pod. This pod can be replicated to as many replicas necessary. Any changes to this deployment YAML file will be used to update the pods under this deployment. We can also delete an entire deployment as well.

Deployment YAML files will contain information by which, the deployment will know which pods belongs to which deployment using metadata. This connection is made using the labels and matchLabels key-value pairs.

We can create a deployment using the command line:
```
kubectl create deployment [deployment-name] --image=[image-name]
```

We can also use a YAML file to create a deployment:
1. Create a namespace called nginx
```
kubectl create namespace nginx
```
2. Create a YAML file called deployment.yml
```
vi deployment.yml
```
3. Copy the following below:
```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: nginx
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
4. To create the deployment from the YAML file, run this command:
```
kubectl apply -f deployment.yml
```
5. To check if the deployment has been successfully applied:
```
kubectl get deployments
```
```
kubectl get pods
```
> NOTE: If there are any changes made to the original deployment.yml, we can use the same apply command to apply the updates to the deployment.

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

</br>

### Services
Each pod upon creation is given an IP address and this IP address is used for pods to talk to one another. However, pods are ephemeral. They die easily and when a pod is newly created again, the IP address changes and this is troublesome. So we use Service.

Service is a resource that consolidates all the pods that serves the same function and collates their IP addresses in one place. If a pod dies and a new pod is created, the Service will be able to remove the old unused IP address and add the new IP address inside.

Using Service and replicas, we are able to direct connections to existing replicas while a new one is recreating. Usually, a service YAML file is written together in the same file as the deployment YAML file. Using metadata, the service will know which deployment it should be connected to as well. This connection is made using the selector and labels key-value pairs.

We can use a YAML to create a service:
1. Create a YAML file named service.yml
```
vi service.yml
```
2. Copy the following below:
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
      port: 8081
      targetPort: 80
```
3. To create the service from the YAML file, run this command:
```
kubectl apply -f service.yml
```
> In the example above, the port 8081 belongs to the service and the targetPort 80.
> 
> NOTE: We can create different types of service. By default, a created service is an internal service. We can specify the service type by adding a key-value pair under the specs section
> ```
> [...]
> specs:
>   [...]
>   type: <service-type>
> ```

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

</br>

### ConfigMaps
Using a ConfigMap resource, we are able to store variables that our deployments can take reference from. 

We can create a configmap using a YAML file:
1. Create a YAML file named configmap.yml
```
vi configmap.yml
```
2. Copy the following below:
```
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongodb-configmap
data:
  database_url: mongodb_service
```
3. To create the configmap from the YAML file, run this command:
```
kubectl apply -f configmap.yml
```
> Now we can use the variable "database_url" in our deployment YAML files
> ```
> [...]
> valueFrom:
>   configMapKeyRef:
>     name: mongodb-configmap
>     key: database_url
> ```

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

</br>

### Secrets
Using a Secret resource, we are able to store variables that our deployment needs to take reference from but DO NOT want people to know what the actual value is. This is useful for admin/password values. These values have to be base64 encoded as well.

We can create a secret using a YAML file:
1. Create a YAML file named secret.yml
```
vi secret.yml
```
2. Copy the following below:
```
---
apiVersion: v1
kind: Secret
metadata:
  name: mongodb-secret
type: Opaque
data:
  mongo-root-username: dXN1cm5hbWU=
  mongo-root-password: cGFzc3dvcmQ=
```
> We can base64 encode our username and password by running this command:
> ```
> echo -n 'username' | base64
> ```
> ```
> echo -n 'password' | base64
> ```
3. To create a secret from the YAML file, run this command:
```
kubectl apply -f secret.yml
```
4. Now we can use the variable "mongo-root-username" and "mongo-root-password" in our deployment files.
> ```
> [...]
> valueFrom:
>   secretKeyRef:
>     name: mongo-secret
>     key: mongo-root-username
> [...]
> valueFrom:
>   secretKeyRef:
>     name: mongo-secret
>     key: mongo-root-password
> ```

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

</br>

### Ingress
With ingress, we are able to use an actual domain name to access the application. Ingress provides routing rules which can redirect the user to different applications using different routing paths.

To use ingress, we need an ingress controller.

We can get an ingress controller in Minikube using the following command:
```
minikube addons enable ingress
```
We can create an ingress using a YAML file:
1. Create a YAML file named ingress.yml
```
vi ingress.yml
```
2. Copy the following below:
```
---
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: myapp-ingress
spec:
  rules:
  - host: myapp.com
    http:
      paths:
      - backend:
        serviceName: myapp-internal-service
        servicePort: 8080
```
3. To create a secret from the YAML file, run this command:
```
kubectl apply -f ingress.yml
```
4. Edit our /etc/hosts file
```
sudo vim /etc/hosts
```
> Under the last line of the file, add:
> ```
> ingress.ip.address &nbsp; &nbsp; domainname.com
> ```
> 
> The serviceName should point to the name in the service you created under the service's metadata's name. \
> The servicePort should point to the service's port as well.

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

</br>

### Volumes


[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

</br>

## Getting Status of Different Resources
```
kubectl get <resource-type>
```
> pod-name/deployment-name allows us to target a component more specifically \
> -A allows us to get information of everything \
> -n <namespace> allows us to get information in a specific namespace \
> -o wide gives us more details compared to the basic details \
> -o yaml shows us the YAML file relating to that particular component

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)

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

[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes#table-of-contents)
