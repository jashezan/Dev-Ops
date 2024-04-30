# Docker Introduction

## Instructions to run the container

1. Start with an OS
2. Install Node
3. Copy app files
4. Run the node index.js

## The Dockerfile

```Dockerfile
FROM node:alpine

COPY . /app

WORKDIR /app

CMD ["node", "index.js"]
```

## Explanation

```Dockerfile
FROM node:alpine 
```
- Base image - node:alpine
- Base image is the image on which our image is based
- Generally we have to install linux then node and then our app
- But,generally node image already comes with linux and node installed
- we can use node:ubuntu which is based on ubuntu
- or node:alpine which is based on alpine
- or node:debian which is based on debian
- we are using node:alpine because alpine is a very small and lightweight linux distro
- so, it will make our image size small
- and it is also secure and fast
- so, we have done 2 step of our process to run our app

```Dockerfile
COPY . /app
```
- Copy all the files from current directory to /app directory in the image
- . means current directory (out local directory)
- /app is the directory in the image (inside the container)
- so, we are copying all the files from our local directory to the /app directory in the image
- so, we have done 3rd step of our process to run our app

```Dockerfile
WORKDIR /app
```
- Set the working directory to /app
- this working directory is inside the container
- its like opening the terminal in /home/user directory in out local computer
- when we open the terminal in /home/user directory then we can directly run the commands in that directory
- so, we are setting the working directory to /app directory in the image
- all the commands will assume that they are running in /app directory
- so all the commands below will be executed in relative to /app directory

```Dockerfile
CMD ["node", "index.js"]
```
- Run the command node index.js
- this command will run when the container starts
- CMD means command to run when the container starts inside the container
- the array after CMD is the command to run and its arguments
- the 1st element is to run node command and 2nd element is to run index.js file with node command
- so, we have done 4th step of our process to run our app 

## Make the image

```bash
docker build -t hello-docker .
```
- build the image with the name(tag) hello-docker with `-t` option
- . means the current directory which contains the Dockerfile and app files to copy
- but the docker image will not be in current directory
- it will be in the docker image registry
- but we can refer to the image with the name hello-docker
- so, it will build the image with the name hello-docker
- The docker image is not a single file
- It is a collection of files and directories of the image and its layers and metadata and configuration and environment variables and etc.
- Even though it is a collection of files and directories, we can refer to it with a single name hello-docker
- Later we can discuss about where the image is stored and how to see the image and how to see the files and directories of the image
- It is like creating a blueprint of the image
- so, we have created the blueprint of our image
- we have not run the app yet
- So the image contains alpine linux OS, node and our app files
- Even after containing these much things, the image size is very small (around 100MB)
- we have yet to create the container from the image.

> **Note:** Don't forget the dot(.) at the end of the command

## See the image

```bash
docker images 
# or docker image ls, because its equivalent to docker images
# docker image ls lists images
# docker images xyz lists images with the name xyz. 
# So you usually get empty list for a docker images ls, because ls is treated as name. 
# By the way, we can use a wildcard `docker images postgr*` to list images with the name starting with postgr
```

Output:
```bash
$ docker images                 
REPOSITORY               TAG       IMAGE ID       CREATED              SIZE
hello-docker             latest    0027ac2b288b   About a minute ago   141MB
```

- list all the images
- we can see the image hello-docker in the list
- we can see the size of the image
- we can see the tag of the image
- we can see the image id
- we can see when the image was created
- we can see the status of the image
- we can see the repository of the image

> **Note:** The image id will be different for you  

Now we can run the image in any computer that has docker installed. They can run the node application even though they don't have node installed. This is the power of docker.

## Run the container built from the image

We can run the container from any directory in our computer. We don't need the app files to run the container. The app files are already copied to the image.

- The container is the running instance of the image
- Container is a special kind of process in the host OS
- The container is isolated from the host OS
- The container is isolated from other containers running in the same host OS.
- The container is isolated from the network. It has its own network.
- The container is isolated from the file system. it have its own file system
- The container is isolated from the processes running in the host OS

```bash
docker run hello-docker
```

Output:
```bash
Hello Docker
```

- run the container from the image hello-docker
- the container will run the command node index.js
- the output of the command will be printed in the terminal
- the output of the command is Hello Docker
- so, the container is running the node application in the alpine linux OS

## See the running containers

```bash
docker ps
# or docker container ls, because its equivalent to docker ps
# docker ps lists running containers
# docker ps -a lists all containers including stopped containers
```

## See the logs of the container

```bash
docker logs <container-id>
# or docker logs <container-name> 
# docker logs <container-id> prints the logs of the container
```
- we can see the logs of the container
- we can see the output of the command node index.js
- we can see the output Hello Docker

## Push the image to the docker hub

```bash
docker login
```
- login to the docker hub
- enter the username and password
- if you have 2 factor authentication enabled, then you have to enter the OTP
- you will get a success message if you are logged in successfully
- you will get an error message if you are not logged in successfully
- you will get a warning message if you are already logged in

```bash
docker tag hello-docker <username>/hello-docker
```
- tag the image with the username
- the username is the username of the docker hub account
- tag means giving a name to the image with the username

```bash
docker push <username>/hello-docker
```
- push the image to the docker hub
- the image will be pushed to the docker hub account
- the image will be stored in the docker hub registry

## Pull the image from the docker hub (From another computer)

```bash
docker pull <username>/hello-docker
```
- pull the image from the docker hub
- the image will be pulled from the docker hub registry
- the image will be stored in the local computer
- the image will be stored in the local docker image registry with the name <username>/hello-docker
- the image will be stored in the local docker image registry with the tag latest

## Run the container from the image (From another computer)

```bash
docker run <username>/hello-docker
```

Output:
```bash
Hello Docker
```

- run the container from the image <username>/hello-docker
- the container will run the command node index.js
- the output of the command will be printed in the terminal
- the output of the command is Hello Docker
- so, the container is running the node application in the alpine linux OS

## Exit the container
```bash
exit
```
- exit the container
- the container will be stopped

## Stop the container
```bash
docker stop <container-id>
# or docker stop <container-name>
# docker stop <container-id> stops the container
```
- stop the container
- the container will be in the stopped state

## Remove the container
```bash
docker rm <container-id>
# or docker rm <container-name>
# docker rm <container-id> removes the container
```
- remove the container
- the container will be removed from the list of containers
- the container will be removed from the host OS

## Remove all stopped container
```bash
docker container prune
```
- remove all the stopped containers from the host OS
- the stopped containers are the containers that are not running

## Remove the image
```bash
docker rmi <image-id>
# or docker rmi <image-name>
# docker rmi <image-id> removes the image
```
- remove the image
- the image will be removed from the list of images
- the image will be removed from the local docker image registry
- the image will be removed from the host OS

## Remove all unused Docker Images
```bash
docker image prune
```
- remove all the dangling images from the local docker image registry
- the dangling images are the images that are not used by any container
- the dangling images are the images that are not tagged

## Restart the container
```bash
docker start -i <container-id>
# or docker start -i <container-name>
# docker start -i <container-id> starts the container in the interactive mode
# First 3 characters of the container id is enough
```
- start the container in the interactive mode
- the container will be started
- the container will be in the (`-i`) interactive mode
- the container will be in the (`-t`) terminal mode
- the container will be in the alpine linux OS
- the container will be in the root directory

## Retag the docker image
```bash
docker image tag sample-app:latest sample-app:1.0
```
- Its better to avoid using `latest` tag
- because it is not clear which version of the image is the latest
- so, we can tag the image with the version number
- we can tag the image with the version number 1.0 or any name like dev, prod, test, etc.
- retag the image with the new tag
- the image will be tagged with the new tag

## Summary

- We have created a docker image with the node application
- We have run the container from the image
- We have seen the logs of the container
- We have pushed the image to the docker hub
- We have pulled the image from the docker hub
- We have run the container from the image in another computer

