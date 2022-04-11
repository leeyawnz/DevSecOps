# Docker Documentation
This is my own personal documentation of the common Docker commands. <br>As most OS are Linux in DevOps, this in the context of Linux (Ubuntu). \
Updated: Apr 2022

</br>

## Table of Contents:
> - [Understanding Docker](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#understanding-docker)
> - [Setting up Docker](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#setting-up-docker)
> - [Getting a Docker Image](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#getting-a-docker-image)
> - [Creating and Running a Docker Container](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#creating-and-running-a-docker-container)
> - [Exposed Container Ports](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#exposed-container-port)
> - [Managing Docker Containers](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#managing-docker-containers)
>    - [Naming Docker Containers](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#naming-docker-containers)
>    - [Formatting docker ps Output](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#formatting-docker-ps-output)
> - [Docker Volumes](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#docker-volumes)
>    - [Host <-> Container](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#host---container-volumes)
>    - [Container <-> Container Volumes](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#container---container-volumes)
> - [Docker Network](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#docker-network)
> - [Building Images Using Dockerfile Volumes](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#build-images-using-dockerfile)
>    - [Creating a Dockerfile](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#creating-a-dockerfile)
>    - [Dockerfile Syntax](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#dockerfile-syntax)
>    - [Dockerfile Demo](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#dockerfile-demo)
>    - [Caching and Layers](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#caching-and-layers)
>    - [Using Alpine](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#using-alpine)
> - [Docker Compose](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#docker-compose)
> - [Version Control](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#version-control)
>    - [Implementing in Dockerfile](https://github.com/leeyawnz/DevSecOps/tree/main/Docker#implementing-in-dockerfile)
> - [Docker Registries](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#docker-registries)
>    - [Pushing Images to Docker Hub](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#pushing-images-to-docker-hub)
> - [Debugging Docker Containers](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#debugging-docker-containers)
>    - [Docker Inspect](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#docker-inspect)
>    - [Docker Logs](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#docker-logs)
>    - [Docker Exec](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#docker-exec)
>    - [Managing Unused Docker Resources](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#managing-unused-docker-resources)

</br>

## Understanding Docker
Docker is created to containerize applications. The idea behind Docker is that it allows developers/operations to agree on a runtime environment with all the necessary dependencies that the application requires and we are able to save these information into an Image. This image makes sure that the environment remains the same in development and deployment. With the help of Docker, we are also able to share this image easily with anyone.

Docker is centered around being lightweight and promotes consistency when developing and running an application.

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

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
```
systemctl start docker
```
```
systemctl enable docker
```

Check if Docker is installed by running the command below or by checking the version.
```
sudo docker run hello-world
```
```
docker --version
```
[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

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
 
[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

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

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

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

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

</br>

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

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

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

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

</br>

## Docker Network
Using docker network, we can control how containers are linked together. Containers in the same network can communicate with each other with just container names.

### Types of Networks
> - Bridge
> - Host
> - None
> - Overlay
> - Macvlan
> 
> By default, all containers will be attached to the default bridge network
```
docker network ls
```
> We can check all existing and newly created networks with this command.
```
docker network create <network-name>
```
> We can create a new bridge network using this command.
```
docker run --net <network-name> <image-id/image-name>
```
> Using this command, we can link the container to the specific network during the run command.
```
docker network connect <network-id> <container-id>
```
> This command connects the container to a specific network.
```
docker network disconnect <network-id> <container-id>
```
> This command disconnects the container from a specific network

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

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
> RUN - command triggers while building the Docker image \
> CMD - command triggers after the image has been built and launched, can be overwritten \
> ENTRYPOINT - default command that will run regardless, cannot be overwritten \
> EXPOSE - exposing a container port
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
> Builds an image with name and tag of website:latest using the Dockerfile in the pwd (reference by the dot). \
> \
> This built image will have all the files from your local machine being pushed inside, so whoever has this image would have access to the files that you have built.

### Caching and Layers
When building an image, Docker is able to utilize caching to check for differences in the new image you're building. So bear in mind when buildng an image, think about what changes and what does not and utilize caching so that building an image would be faster.

### Using Alpine
We can utilize the alpine version of an image to make it even more lightweight and this would build images quicker as well. We can find the alpine version in the relevant official Docker Hub images. This is important when using Dockerfile to build your images.
> Comparing nginx:latest and nginx:alpine images, the difference in the image size is almost 105MB!

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

</br>

## Docker Compose
To see how docker compose works, we must first see how the basic docker run command looks like.
```
docker run -d --name webserver -p 8080:80 --net web-network -v $(pwd):/usr/share/nginx/html nginx:latest
```
If you need to run multiple containers, you would require to type the commands multiple times and may also run into typo errors and this is where docker compose comes in.

To start, we need to download the Docker Compose package
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```
sudo chmod +x /usr/local/bin/docker-compose
```
```
docker-compose --version
```

We can now use Docker Compose. We can see Docker Compose in action by first creating a docker-compose.yml file.
```
vi docker-compose.yml
```
Inside the docker-compose.yml file, we can type
```
version: "3.9"

services:
  webserver:
    image: "nginx:latest"
    volume:
      - .:/usr/share/nginx/html
    ports:
      - "8080:80"
    networks:
      - web_network
  redis:
    image: "redis:alpine"
    
networks:
  web_network:
    driver: none
```
> Using this docker-compose.yml format, we can deploy multiple Docker containers by simply adding more containers under the services section.

To execute this docker-compose.yml file, we can execute this command.
```
docker-compose up
```
> We can run multiple docker-compose.yml file using
> ```
> docker-compose -f docker-compose.yml -f docker-compose-admin.yml up -d
> ```

To remove containers relating to that specific docker-compose.yml, we can run this command
```
docker-compose down
```

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

</br>

## Version Control
Docker is one of the tools used in the CI/CD pipeline to provide value as quick as possible to end-users. The most important thing about DevOps is Version Control. If an error is detected, version control kicks in to rollback to the last working version so that users will still be able to use the application without any issues. So it comes as no surprise that Docker has its form of Version Control.

### Implementing in Dockerfile
To implement version control for your Dockerfile, make sure that you use the full version tagname after your image name after the FROM syntax.
> FROM node:10.16.1-alpine \
>  \
> By specifying the specific version in your Dockerfile, you would be able to know for which base image you application has no problems running on. If you constantly use the node:latest tag, if the newest version of node is not compatible with your existing code base, you would not know which node version to revert to.

### Implementing in Docker tags
```
docker tag website:latest website:version-1
```
> This command actually creates 2 images of similar image ids with different tags \
> e.g  \
> \
> website:latest \
> website:version-1 \
> \
> If version-2 exists, the latest tag will have the same image id as version-2 and so on. The latest tag will point to the latest version

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

</br>

## Docker Registries
Docker Registries is a highly scalable server side application that stores and distributes Docker images. It is used in the CI/CD Pipeline. 

There are private and public Docker Registries. To maintain privacy, we can use private registries. Else, we can store our images publicly.

Docker Registry Providers:
- Docker Hub
- quay.io
- Amazon ECR

### Pushing Images to Docker Hub
We can store both private and public images here.

First, we need to create an account on Docker Hub and create a new repository at the Docker Hub official website.

After creating a repository, we will need to sign in on the terminal before using the Docker command provided to push an image to the Docker repository.
```
docker login
```
```
docker push username/repo-name:tagname
```

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

</br>

## Debugging Docker Containers
### Docker Inspect
Using the docker inspect command, we can gain access to information of a specific container.
```
docker inspect <container-id/container-name>
```
> We can pass the command through a grep pipe to find the specific information that you may need as well
> ```
> docker inspect <container-id/container-name> | grep info
> ```

### Docker Logs
Using the docker logs command, we can gain access on all activities of a container. We can link this to the ELK stack as well.
> ```
> docker logs <container-id/container-name>
> ```

### Docker Exec
Using this command, we can gain access into the container. This allows us to check if a Docker container has been properly configured.
```
docker exec -it <container-id/container-name> /bin/bash
```
> If an error occurs, we can use docker inspect and under cmd, we can see the shell type

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Docker/README.md#table-of-contents)

## Managing Unused Docker Resources
As the number of images, containers and networks grow, some of them will no longer be used and we are able to remove them to make space for other resources. We can do this by pruning.
```
docker image prune
```
> This command removes all images not referenced by any containers.
```
docker container prune
```
> This command removes all stopped containers.
```
docker volume prune
```
> This command removes any unused local volumes that are not referenced by any containers.
```
docker network prune
```
> This command removes any unused networks that are not referenced by any containers.
```
docker system prune
```
> This command removes any unused resources (images, containers, networks, volumes) that are not referenced by any containers.
