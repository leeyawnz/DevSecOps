# Terraform
This is my own personal documentation of the common Terraform commands. \
As most OS are Linux in DevOps, this in the context of Linux (Ubuntu).

</br>

## Table of Contents:
> - [Setting Up Terraform](https://github.com/leeyawnz/DevSecOps/blob/main/Terraform/README.md#setting-up-terraform)
> - [Terraform in Action](https://github.com/leeyawnz/DevSecOps/blob/main/Terraform/README.md#terraform-in-action)

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

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Terraform/README.md#table-of-contents)

</br>

## Terraform in Action
Create a directory named learn-terraform-docker-container
```
mkdir learn-terraform-docker-container
```
Navigate into the directory
```
cd learn-terraform-docker-container
```
Create a file named main.tf
```
vi main.tf
```
> Copy the following code into the main.tf file
> ```
> terraform {
>   required_providers {
>     docker = {
>       source  = "kreuzwerker/docker"
>       version = "~> 2.13.0"
>     }
>   }
> }
> 
> provider "docker" {}
>
> resource "docker_image" "nginx" {
>   name         = "nginx:latest"
>   keep_locally = false
> }
> 
> resource "docker_container" "nginx" {
>   image = docker_image.nginx.latest
>   name  = "tutorial"
>   ports {
>     internal = 80
>     external = 8000
>   }
> }
> ```
Initialize the project
```
terraform init
```
Creating the infrastructure
```
terraform apply
```
> Check if the Docker container has been created
> ```
> docker ps -a
> ```
> \
> Can also run [localhost:8000](localhost:8000) to see if webserver is running.

Destorying the infrastructure
```
terraform destroy
```
> We can check that the Docker container has been destroyed
> ```
> docker ps -a
> ```

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Terraform/README.md#table-of-contents)
