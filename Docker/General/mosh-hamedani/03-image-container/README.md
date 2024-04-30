# Building Images

In this section, we will learn how to build images using Dockerfiles.
- Creating Dockerfiles
- Versioning Images
- Sharing Images
- Saving and Loading Images
- Reducing the Image Size
- Speeding Up Builds

## Image vs Container

| Fields | Image | Container |
| --- | --- | --- |
| Definition | A read-only template with instructions for creating a Docker container | A runnable instance of an image |
| State | Immutable | Mutable |
| Creation | Created using a Dockerfile | Created using `docker run` command |
| Storage | Stored in the Docker registry | Stored in the Docker host |
| Modification | Can be modified by creating a new image | Can be modified by committing changes |
| Sharing | Can be shared using the Docker registry | Can be shared using `docker commit` command |
| Size | Smaller | Larger |
| Lifecycle | Longer | Shorter |
| Purpose | Used to create containers | Used to run applications |
| Example | Ubuntu image | Ubuntu container |
| Command to see list | `docker images` | `docker container ls` |
| Command to get | `docker pull` | `docker run` |
| Command | `docker push` | `docker commit` |
| Command to remove | `docker rmi` | `docker rm` |
| Command | `docker save` | `docker export` |
| Command | `docker load` | `docker import` |
| Command | `docker build` | `docker exec` |
| Command | `docker tag` | `docker start` |

### Image Contents
- A cut down OS
- Third-party libraries
- Application code
- Application dependencies
- Application configuration
- Application runtime environment
- Environment variables

### Container Contents
- Provides an isolated environment (similar to a Virtual Machine)
- Can be run, started, stopped, moved, and deleted
- Is just a special type of process [special because it has its own filesystem, networking, and isolated process tree]
- Multiple independent containers can derived from the same image