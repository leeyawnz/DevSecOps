# Kubernetes
This is my own personal documentation of the common Docker commands. <br>As most OS are Linux in DevOps, this in the context of Linux (Ubuntu).

## Table of Contents

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
### Installing Minikube
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
```
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
### Installing Docker
You can refer to the Docker installation [here](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#setting-up-docker).
