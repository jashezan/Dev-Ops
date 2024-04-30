# Vidly-Docker
# Docker cheet sheet
* **Building docker image command**

        docker build -t `image name` `path of the Dockerfile`

* **Show list of docker images in local machine**
        
        docker images / docker image ls

* **Run docker image command**
  
        docker run `image name`

* **Pull an image from docker hub**
  
        docker pull `image name` 

* **Run image if the image is present in local machine run it otherwise pull the image from docker hub and run it**  
  
        docker run `image name`

* **Print all the running container / stoped container**
  
        docker ps / docker ps -a

* **Start a container with interactive mode**
  
        docker run -it `image name`

* **Start a container that is recently closed**
  
        docker start -i `stoped container process id`

* **Start a new bash session in same container** 

        docker exec -it `running container process id` bash
        
* **Start a new bash session in same container with a particuler user login** 

        docker exec -it -u `user name` `running container process id` bash

* **Remove all stoped container** 

        docker container prune

* **Remove all dangl images**
  
        docker image prune

* **Remove docker image**

        docker image rm `name of the image` / `image id`

* **Add tag in build time**

        docker build -t `image name`:`tag` `Path of the docker file`

* **Add tag after build**

        docker image tag `image name`:`Current tag / latest` `image name`:`new tag`

* **Explicitly set latest tag**

        docker image tag `image id` `image name`:`latest`

* **Docker login**

        docker login

* **Push the image into docker hub**

        docker push `image name`:`tag`

* **Save image**

        docker image save -o `zip file name` `image name`:`tag`

* **Docker image load**

        docker image load -i `zip file of the image`

* **Run Docker image and start shell**

        docker run -it `image name` sh

* **Run a container in background / detached mode**

        docker run -d `image name`

* **Run a container in background / detached mode with a name specified**

        docker run -d --name `name` `image name`

* **Check logs of a running container**

        docker logs `running container id`

* **Publishing a port**

        docker run -d -p `port of your local machine that you want to serve the app` : `port of container where app is running` --name `name` `image name`

* **Execute a command in a running container**

        docker exec `running container name / container id` `commands that you want o ececute`

* **Stop a running container**

        docker stop `container name / container id`

* **Start a recently stoped container**

        docker start `container name / container id`

* **remove a container**

        docker container rm `container name` / docker rm `container name`

* **Create a volume**

        docker volume create `volume name`

* **map a volume to docker container**

        docker run -d -p `here you have to map you port` -v `volume name`:`path of a folder that you want to map with volume (example: /app/data)` `docker image name`

* **Copy file from docker container to host**

        docker cp `container id`:`file path that we want to copy` `path where we put that file`

* **Copy file from host to container**

        docker cp `file path that we want to copy` `contener id`:`path where we want to copy`

* **map local direcotry with container directory**

        docker run -d -p `here you have to map you port` -v `$(pwd)`:`path of the root of the project (example /app )` `docker image name`

* **Remove all running and stoped container at once**

        docker container rm -f $(docker container ls -aq)

* **Remove all images at once**

        docker image rm $(docker image ls -q)

* **Starting application using docker-compose**

        docker-compose up

* **Build images using docker-compose**

        docker-compose build
        
* **Build images using docker-compose without using cache**

        docker-compose build --no-cache

* **Running a container with detached mode using docker compose**

        docker-compose up -d

* **Stop a container using docker-compose**

        docker-compose down

* **Print docker network**

        docker network ls

* **Viewing logs using docker compose**

        docker-compose logs
