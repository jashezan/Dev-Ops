# Working with Containers

Containers are a way to package software in a format that can run isolated on a shared operating system. Containers are lightweight, standalone, executable packages of software that include everything needed to run an application: code, runtime, system tools, system libraries, and settings. Containers isolate software from its surroundings, for example differences between development and staging environments and help reduce conflicts between teams running different software on the same infrastructure.

## Todo 
- Starting and stopping containers
- publishing ports
- viewing logs
- Executing commands in a running container
- Removing containers
- Persisting data with volumes
- Sharing Source Code with Volumes

## Running a Container in the Background

```bash
docker run -d -p 8000:3000 --name sample-app-sky-blue sample-app
```
- `-d` flag runs the container in the background
- `-p` flag is for port mapping, `8000:3000` means map the port 8000(1st) of the host to the port 3000(2nd) of the container.
- `--name` flag is for naming the container (optional) (if not provided, docker will generate a random name) - `sample-app-sky-blue` is the name of the container here.
- `sample-app` is the name of the image

## See logs of a container

```bash
docker logs sample-app-sky-blue # (one time logs)
# or 
docker logs -f sample-app-sky-blue # -f flag is for following the logs (continuously logging)
```

## Publishing Ports

```bash
docker run -d -p 8000:3000 --name sample-app-sky-blue sample-app
```
- `-p` flag is for port mapping, `8000:3000` means map the port 8000(1st) of the host to the port 3000(2nd) of the container.

## Executing commands in a running container

```bash
docker exec -it sample-app-sky-blue /bin/bash
# or docker exec -it 4b3e7b7b6b5b /bin/bash # (if you don't know the name of the container)
```
- `-i` flag is for interactive mode
- `-t` flag is for terminal mode
- `sample-app-sky-blue` is the name of the container

> Note: You can also run any command in the container by replacing `/bin/bash` with the command you want to run like ls, cat, find, grep.

### Difference between `docker exec` and `docker run`
- `docker exec` runs a command in a running container
- `docker run` runs a command in a new container

## Stopping a container

```bash
docker stop sample-app-sky-blue
# or docker stop 4b3e7b7b6b5b # (if you don't know the name of the container)
```
- `sample-app-sky-blue` is the name of the container
- `4b3e7b7b6b5b` is the container id
- `docker stop` stops the container

## Starting a container

```bash
docker start sample-app-sky-blue
# or docker start 4b3e7b7b6b5b # (if you don't know the name of the container)
```
- `sample-app-sky-blue` is the name of the container
- `4b3e7b7b6b5b` is the container id
- `docker start` starts the container

### Difference between `docker start` and `docker run`
- `docker start` starts a stopped container
- `docker run` starts a new container

## Removing a container

```bash
docker rm sample-app-sky-blue
# or docker rm 4b3e7b7b6b5b # (if you don't know the name of the container)
```
- `sample-app-sky-blue` is the name of the container
- `4b3e7b7b6b5b` is the container id
- `docker rm` removes the container
- `docker rm -f` removes the container forcefully (if the container is running)
- `docker rm -f $(docker ps -aq)` removes all the containers
- `docker rm -f $(docker ps -aq)` removes all the containers forcefully

> **Note:** By removing a container, you are not removing the image. The image will still be there.

> **Note:** By removing a container, the data inside the container will be lost if it is not persisted with a volume.

## Persisting data with volumes

Volumes are a storage outside the container that can be used to persist data. Volumes are used to store data generated by and used by Docker containers. Volumes are stored in a part of the host filesystem which is managed by Docker (`/var/lib/docker/volumes/` on Linux). Non-Docker processes should not modify this part of the filesystem.

### Creating a volume
```bash
docker volume create sample-app-volume
```
- `sample-app-volume` is the name of the volume
- `docker volume create` creates a volume
- `docker volume ls` lists all the volumes
- `docker volume inspect sample-app-volume` gives the details of the volume
- `docker volume rm sample-app-volume` removes the volume
- `docker volume rm $(docker volume ls -q)` removes all the volumes

### Volume Details
```bash
docker volume inspect sample-app-volume
```
Output:
```json
[
    {
        "CreatedAt": "2021-08-29T14:00:00Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/sample-app-volume/_data",
        "Name": "sample-app-volume",
        "Options": {},
        "Scope": "local"
    }
]
```
- `CreatedAt` is the time when the volume was created
- `Driver` is the type of driver used to create the volume (local in this case). 
  - There is also a `local` driver which is used to create volumes on the host filesystem. 
  - There are other drivers like `nfs`, `efs`, `azure`, `gcp`, etc which are used to create volumes on the cloud.
- `Labels` are the metadata of the volume (if any) (empty in this case) e.g. `com.example.some-label: "some-value"`
- `Mountpoint` is the location where the volume is stored on the host filesystem
- `Name` is the name of the volume
- `Options` are the options used to create the volume (if any) (empty in this case) e.g. `type: "nfs"`
- `Scope` is the scope of the volume (local in this case)
  - (local means the volume is available only on the host where it was created)
  - (global means the volume is available on all the hosts in the swarm)


### Usage of volumes
```bash
docker run -d -p 8000:3000 -v /app/node_modules -v $(pwd):/app --name sample-app-sky-blue sample-app
# or docker run -d -p 8000:3000 -v /app/node_modules -v $(pwd):/app --name sample-app-sky-blue sample-app
```
- `-v` flag is for volume mapping, `-v /app/node_modules` means create a volume for the `/app/node_modules` directory of the container.
- `-v $(pwd):/app` means map the current directory of the host to the `/app` directory of the container.
- `sample-app` is the name of the image
- `sample-app-sky-blue` is the name of the container
- `$(pwd)` is the current directory of the host
- `/app` is the directory inside the container
- But here is a problem, the `/app/node_modules` directory of the container is being replaced by the volume. To avoid this, we can use a named volume.

### Using a named volume
```bash
docker run -d -p 8000:3000 -v sample-app-volume:/app/node_modules -v $(pwd):/app --name sample-app-sky-blue sample-app
```
- `-v sample-app-volume:/app/node_modules` means create a volume named `sample-app-volume` for the `/app/node_modules` directory of the container.
- `-v $(pwd):/app` means map the current directory of the host to the `/app` directory of the container.
- `sample-app-volume` is the name of the volume

> **Note:** By removing a container, you are not removing the volume. The volume will still be there.

### Problem with volumes
- Volumes are stored on the host filesystem, so if you remove the volume, the data inside the volume will be lost.
- To avoid this, you can use a bind mount.
> **Note:** By creating volume from command line will create the data directory but it be under root user. so its better to create the data directory in Dockerfile and then create the volume. 

### Using a bind mount
```bash
docker run -d -p 8000:3000 -v $(pwd)/app:/app --name sample-app-sky-blue sample-app
```
- `-v $(pwd)/app:/app` means map the `app` directory of the current directory of the host to the `/app` directory of the container.
- `$(pwd)/app` is the `app` directory of the current directory of the host (bind mount)


## Sharing Source Code with Volumes

### Sharing source code with volumes
```bash
docker run -d -p 8000:3000 -v $(pwd):/app --name sample-app-sky-blue sample-app
```
- `-v $(pwd):/app` means map the current directory of the host to the `/app` directory of the container.
- `sample-app` is the name of the image
- `sample-app-sky-blue` is the name of the container

### Sharing source code with bind mount
```bash
docker run -d -p 8000:3000 -v $(pwd)/app:/app --name sample-app-sky-blue sample-app
```
- `-v $(pwd)/app:/app` means map the `app` directory of the current directory of the host to the `/app` directory of the container.
- `sample-app` is the name of the image

### Sharing source code with named volume
```bash
docker run -d -p 8000:3000 -v sample-app-volume:/app --name sample-app-sky-blue sample-app
```
- `-v sample-app-volume:/app` means create a volume named `sample-app-volume` for the `/app` directory of the container.

## Copying files from host to container
```bash
docker cp file.txt sample-app-sky-blue:/app/file.txt
# or docker cp file.txt 4b3e7b7b6b5b:/app/file.txt # (if you don't know the name of the container)
```
- `file.txt` is the file on the host
- `sample-app-sky-blue` is the name of the container
- `:/app/file.txt` is the path inside the container
- `4b3e7b7b6b5b` is the container id

## Copying files from container to host
```bash
docker cp sample-app-sky-blue:/app/file.txt file.txt
# or docker cp 4b3e7b7b6b5b:/app/file.txt file.txt # (if you don't know the name of the container)
```
- `sample-app-sky-blue` is the name of the container
- `:/app/file.txt` is the path inside the container
- `file.txt` is the file on the host
- `4b3e7b7b6b5b` is the container id

## Copying files between containers
```bash
docker cp sample-app-sky-blue:/app/file.txt sample-app-sky-red:/app/file.txt
# or docker cp 4b3e7b7b6b5b:/app/file.txt 4b3e7b7b6b5b:/app/file.txt # (if you don't know the name of the container)
```
- `sample-app-sky-blue` is the name of the source container
- `:/app/file.txt` is the path inside the source container
- `sample-app-sky-red` is the name of the destination container
- `:/app/file.txt` is the path inside the destination container

