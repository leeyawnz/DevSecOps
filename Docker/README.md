# Docker Documentation
This is my own personal documentation of the common Docker commands. <br>As most OS are Linux in DevOps, this in the context of Linux (Ubuntu).

</br>

## Table of Contents:
> - [Setting up Docker](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#setting-up-docker)
> - [Checking Docker Version](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#checking-docker-version)
> - [Getting a Docker Image](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#getting-a-docker-image-example-nginx)
> - [Creating and Running a Docker Container](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#creating-and-running-a-docker-container)
> - [Exposed Container Ports](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#exposed-container-port)
> - [Managing Docker Containers](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#managing-docker-containers)
>    - [Naming Docker Containers](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#naming-docker-containers)
>    - [Formatting docker ps Output](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#formatting-docker-ps-output)
> - [Docker Volumes](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#docker-volumes)
>    - [Host <-> Container](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#host---container-volumes)
>    - [Container <-> Container Volumes](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#container---container-volumes)
> - [Building Images Using Dockerfile Volumes](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#build-images-using-dockerfile)
>    - [Creating a Dockerfile]()
>    - [Dockerfile Syntax]()
>    - [Dockerfile Demo](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#dockerfile-demo)
>    - [Caching and Layers](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#caching-and-layers)
>    - [Using Alpine](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#alpine)
> - [Version Control](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#version-control)
>    - [Implementing in Dockerfile](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#implementing-in-dockerfile)

</br>

## Setting up Docker
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
```
sudo apt-get update
```
```
sudo apt-get install \
     ca-certificates \
     curl \
     gnupg \
     lsb-release
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt-get update
```
```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Check if Docker is installed.
```
sudo docker run hello-world
```

</br>

## Checking Docker Version
```
docker --version
```

</br>

## Getting a Docker Image
Here we are using Nginx as an example.
```
docker pull nginx
```
We can specify the version of the image with a tagname as well.
```
docker pull nginx:latest
```
Check if the image is successfully pulled onto your local machine.
```
docker images
```
Output in Command Line:
> | REPOSITORY  | TAG         | IMAGE ID    | CREATED     | SIZE        |
> | ----------- | ----------- | ----------- | ----------- | ----------- |
> | Nginx       | latest      | 98ebf73aba75| 4 days ago  | 109MB       |

We can also remove images.
```
docker rmi -f <image-id>
```
> Using -f forcefully removes the image

</br>

## Creating and running a Docker Container
```
docker run -d nginx:latest
```
> The -d means we are creating and running the container in detached mode. Without detached mode, the container will run and cause the terminal to hang.

We can check if the container is created using:
```
docker ps -a
```
> docker ps -a checks the status of all created containers.

Output in Command Line:
> | CONTAINER ID | IMAGE        | COMMAND                  | CREATED        | STATUS         | PORTS  | NAMES             |
> | ------------ | ------------ | ------------------------ | -------------- | -------------- | ------ | ----------------- |
> | 7c16ce4bf5b0 | nginx:latest | "nginx -g 'daemon of..." | 38 seconds ago | 37 seconds ago | 80/tcp | suspicious_synder |

We can enter into a docker container with the following command:
```
docker exec -it <container-id/container-name> /bin/bash
```
> This command is useful in checking if the docker container was properly configured

</br>

## Exposed Container Port
In the previous output, we can see under the ports section that the container has an exposed port.
> | PORTS  |
> | ------ |
> | 80/tcp |
This is the port that the container is exposing. To connect to this container, we need to map a localhost port to the container's port.
> Example:\
> 8080 (localhost port) --> 80 (container port)
To map a localhost port to the container's port, it has to be included in the run stage.
```
docker run -d -p 8080:80 nginx:latest
```
> We can check this by going to localhost:8080
We can also map multiple localhost ports to a single container port.
```
docker run -d -p 3000:80 -p 8080:80 nginx:latest
```
> We can check this by going to localhost:3000 and localhost:8080

## Managing Docker Containers
We can stop a running container.
```
docker stop <container-id/container-name>
```
We can start a stopped container.
```
docker start <container-id/container-name>
```
We can remove containers.
```
docker rm <container-id/container-name>
```
> This removes the container if the container is stopped.
```
docker rm -f <container-d/container-name>
```
> This forcefully removes the container even if it is still running.

We can remove multiple containers as well.
```
docker rm $(docker ps -aq)
docker rm -f $(docker ps -aq)
```

### Naming Docker Containers
We can name containers including the --name following command:
```
docker run --name <name>
```
> e.g \
> docker run --name website \
> This allows us to better identify what each container does

### Formatting docker ps Output
We can format the output so that container infomation are easier to read on the command line.
```
export FORMAT="ID\t{{.ID}}\nNAME\t{{.Names}}\nIMAGE\t{{.Image}}\nPORTS\t{{.Ports}}\nCOMMAND\t{{.Command}}\nCREATED\t{{.CreatedAt}}\nSTATUS\t{{.Status}}\n"
```
```
docker ps --format=$FORMAT
```
| Ouput:  |                               |
| ------- | ----------------------------- |
| ID      | a0440e23a60a                  |
| NAME    | website                       |
| IMAGE   | nginx:latest                  |
| PORTS   | 0.0.0.0:9000->80/tcp.         |
| COMMAND | "nginx -g 'daemon of..."      |
| CREATED | 2019-07-24 23:23:25 +0100 BST |
| STATUS  | Up About a minute             |

</br>

## Docker Volumes
Volumes allows us to share data, files and folders. We can share files and folders between host and container and also between containers. We can find documentations on where to mount the volumes at DockerHub.

### Host <-> Container Volumes
With the example of Nginx:
```
docker run --name website -v $(pwd):/usr/share/nginx/html:ro -d nginx
```
> The command above takes the present working directory (pwd) and mounts the data onto the nginx container. \
> Any changes that is made in this pwd, it will be reflected in the nginx container also. \
> We can substitute the $(pwd) with an absolute path on the host machine as well. \
>  \
> The :ro command means read-only, so you are unable to make any volume modifications from within the container. By removing the :ro command, any modifications made from within the container's directory where the volume is mounted, will be reflected in the host machine's volume as well.
> ```
> docker run --name website -v $(pwd):/usr/share/nginx/html -d nginx
> ```

### Container <-> Container Volumes
With the example of Nginx:
```
docker run --name website-copy --volumes-from website -d -p 8081:80 nginx
```
> The command above takes any changes from the website container and is modified in the website-copy container.

</br>

## Build Images Using Dockerfile
Dockerfile allows use to build our own images. By running the Dockerfile, we can create our own custom images and custom containers.

### Creating a Dockerfile
```
vi Dockerfile
```
### Dockerfile Syntax
> FROM - mandatory syntax for creating an image \
> ADD/COPY - adds or copies files from a directory to the container \
> WORKDIR - creates a directory and switches into the newly created directory \
> RUN - command triggers while building the docker image \
> CMD - command triggers after the image has been built and launched, can be overwritten \
> ENTRYPOINT - default command that will run regardless, cannot be overwritten \
> \
> ENTRYPOINT vs CMD \
> e.g \
> docker ps -a \
> We can imagine the ENTRYPOINT command as the "docker" portion and the CMD command as the "ps -a" which can be changed

### Dockerfile Demo
Create an directory called website
```
mkdir website
```
```
cd website
```
Create an index.html file in your working directory
```
vi index.html
```
> Add this in your index.html file
> ```
> <h1> Hello World </h1>
> ```

Create a Dockerfile in your working directory that contains your source code.
```
vi Dockerfile
```
> Inside the Dockerfile add:
> ```
> FROM nginx:latest
> ADD . /usr/share/nginx/html
> ```
> The FROM command is the base image of the container. \
> The ADD command adds all files from the host's pwd (reference with the dot) into the container's /usr/share/nginx/html directory.

Building the Image from Dockerfile
```
docker build -t website:latest .
```
> Builds an image with name and tag of website:latest using the Dockerfile in the pwd (reference by the dot).

### Caching and Layers
When building an image, Docker is able to utilize caching to check for differences in the new image you're building. So bear in mind when buildng an image, think about what changes and what does not and utilize caching so that building an image would be faster.

### Using Alpine
We can utilize the alpine version of an image to make it even more lightweight and this would build images quicker as well. We can find the alpine version in the relevant official DockerHub images. This is important when using Dockerfile to build your images.
> Comparing nginx:latest and nginx:alpine images, the difference in the image size is almost 105MB!

</br>

## Version Control
Docker is one of the tools used in the CI/CD pipeline to provide value as quick as possible to end-users. The most important thing about DevOps is Version Control. If an error is detected, version control kicks in to rollback to the last working version so that users will still be able to use the application without any issues. So it comes as no surprise that Docker has its form of Version Control.

### Implementing in Dockerfile
To implement version control for your Dockerfile, make sure that you use the full version tagname after your image name after the FROM syntax.
> FROM node:10.16.1-alpine \
>  \
> By specifying the specific version in your Dockerfile, you would be able to know for which base image you application has no problems running on. If you constantly use the node:latest tag, if the newest version of node is not compatible with your existing code base, you would not know which node version to revert to.
