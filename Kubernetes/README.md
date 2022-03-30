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
Checking if Minikube is running:
```
minikube start
```
[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes%20(WIP)#table-of-contents)

### Installing Docker
You can refer to the Docker installation [here](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#setting-up-docker). \
[Back to Top](https://github.com/leeyawnz/DevSecOps/tree/main/Kubernetes%20(WIP)#table-of-contents)
