# Docker Documentation
This is my own personal documentation of the common Docker commands. <br>As most OS are Linux in DevOps, this in the context of Linux (Ubuntu).

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

## Getting an Image (Example: Nginx)
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


</br>

## Exposing Port
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

</br>

## Managing Containers
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
