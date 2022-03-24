# Docker Documentation
This is my own personal documentation of the common Docker commands. <br>As most OS are Linux in DevOps, this in the context of Linux (Ubuntu).


## Setting up Docker
```
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```
```
$ sudo apt-get update
```
```
$ sudo apt-get install \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
```
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
$ sudo apt-get update
```
```
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Check if Docker is installed
```
$ sudo docker run hello-world
```


## Checking Docker Version
```
$ docker --version
```


## Getting an Image (Example: Nginx)
```
$ docker pull nginx
```
We can specify the version of the image with a tagname as well
```
$ docker pull nginx:latest
```
Check if the image is successfully pulled onto your local machine
```
$ docker images
```
Output in Command line:
> | REPOSITORY  | TAG         | IMAGE ID    | CREATED     | SIZE        |
> | ----------- | ----------- | ----------- | ----------- | ----------- |
> | Nginx       | latest      | 98ebf73aba75| 4 days ago  | 109MB       |


## Creating a Docker Container
```
$ docker run -d nginx:latest
```
> The -d means we are creating and running the container in detached mode. Without detached mode, the container will run and cause the terminal to hang

We can check if the container is created using:
```
$ docker ps -a
```
> docker ps -a checks the status of all created containers
