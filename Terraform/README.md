# Terraform
This is my own personal documentation of the common Terraform commands.
As most OS are Linux in DevOps, this in the context of Linux (Ubuntu).

</br>

## Table of Contents:
> - [Setting Up Terraform]()

</br>

## Setting Up Terraform
```
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
```
```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
```
```
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
```
```
sudo apt-get update && sudo apt-get install terraform
```
Check if Terraform is successfully installed:
```
terraform version
```

Terraform requires Docker as well. To check the Docker Set Up, you can go [here](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#setting-up-docker).
