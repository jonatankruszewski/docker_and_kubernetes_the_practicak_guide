# Docker and Kubernetes

## Section 1: Getting Started

### What is docker?

Container: A standarized unit of software. A package of code and dependencies to run that code.
The container always yelds the same results. Containers are standalone.
Docker simplifies the creation and management of containers.

### Why Docker & Containers?

Why would we want independent, standarized, application packages?

- Same development environment as production
- Same development environment across team
- Avoids clashing tools / versions

### VM vs Docker

VMS

- VM: own operating system. Redundant duplication, performance can be slow.
- Boot times might be long.
- Reproducing on another server might be tricky.

Docker:

- Emulated container support, + docker Engine
- You can recreate a container by declaring a file.
- Low impact on OS, very fast, minimal disk space usage
- Still encapsulating
- Sharing re building and distribution is easy

### Setup

- If the requirements are met, you can use Docker Desktop. Otherwise Docker toolbox (WIN, MAC)
- On linux, install docker engine.

### Getting our Hands dirty

```sh
docker build .
# grab the sha 
docker run -p 3000:3000 #sha. After running, the terminal will freeze.
docker ps
docker stop fantasy_name
```

### Images and containers: What and why?

- Containers hold the environment for a software. Run instances of the images.
- Images are the blueprints. Shearable package

```sh
docker pull node
```

downloads the latest image of node

RUN executes when an image is created
CMD executes when a container is spinned

So far, after updating the code, to view the changes I need to stop the container and spin a new one.

The instructions (dockerFIle) was run as a standalone

### 27. Managing images and containers

Images

- Tag means name ```-t```
- List ```docker images```
- Analyze ```docker image inspect```
- remove ```docker rmi, docker prune```

Containers
Containers are named, images are tagged

docker ps - shows running containers
docker ps -a shows running and exited containers
docker run starts a new container.
But I Can respin an old container.
I can search for stopped containers. ```docker start idOrNameOfContainer```
Starting a container allows to interact with the terminal (run doesn't)
So there is a weird state that I can't see the logs

Docker run => Foreground
Docker start => Background

Being attached means to listen to the output of the container

### 29. understanding attached & detached containers

To run a container detached:

```sh
docker run -p 8000:80 -d imageName
```

To attach to a detached container:

```sh
docker container attach CONTAINER_NAME
```

To check the logs:

```sh
docker logs CONTAINER_NAME # adding -f attaches too
```

allows output from the container, but not input

```sh
docker run
```

allows output and input from the container and terminal exposed

```sh
docker run -it
```

To reattach to a container output and inpute

```sh
docker start -ai CONTAINER_NAME
```

### 32. Deleting images & containers

To remove containers (you can't remove a running container, you need to stop it first):

```sh
docker rm CONTAINER_1 CONTAINER_2
```

docker remove images. Only if they are not used by a container (including stopped)

```sh
docker rmi imageName
```

To remove all unused images:

```sh
docker image prune
```

To remove a container automatically when it exits, add ```--rm``` to the docker run command.

### 34.A look behind the scenes: inspecting images

To inspect

```sh
docker image inspect IMAGE_NAME
```

### 35. Copying files

```sh
docker cp ORIGIN CONTAINER_NAME:/CONTAINER_FOLDER
```

Also it is possible to copy from the container to the PC by swapping paths

### 36. Naming and tagging containers

### 49. Volumes

```sh
docker run -d -p 3000:3000 --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes 
docker volume rm
docker volume prune
```

```sh
docker run -d -p 3000:3000 --rm --name feedback-app -v feedback:/app/feedback -v "$(pwd):/app" feedback-node:volumes 
```

```sh
docker run -d --rm -p 3000:80 --name feedback-app -v feedback:/app/feedback -v "$(pwd):/app" -v app/node_volumnes feedback-node:volumes
```

Volumes help persist data.
Folders on your host computer mapped to folders inside your docker container.
Changes on one of the folder are accesible to the other, botwh ways.
Volumes persis it a container shuts down.
A container can read and write data from it.
we can see the volumes with docker volumes command.
If you stop a container with an anonymous container, it will not be shown on docker volumes.
A named volume is also mapped to a path in your computer, managed by docker.
Named volumes won/t be deleted when the container is shut down.
Named volumes are great for data which should persist but which you don't need to edit directly.
Named volumes can't be created inside a docker file.
Needs to be created with the docker run command.
syntax: -v nameIWant:PathToDockerToLink. Won't be deleted when the container shuts down.
Anonymous volumes are closely attached to running containers.

If you are running a container with an anonymous volume, it will detach automatically with --rm.
Without the --rm, the anonymous volume won't be removed.
But running the container will run a new anonymous volume, pilling up old volumnes.
They can be cleared with docker volume prune.

If I restart the container, the data will be there.

Bind mounts
Good for editable data (by the developer)
The path must be absolute.

In a bind mount, when there are files collisions, Docker won't override the host machine with the container (safety)
The content machine will override the path in the docker container.

To solve the node_modules problem, to avoid being emptied by the host if the host didn't run npm install, is to use a more specific anonymous volume.

The more specific path 

```sh
docker run --rm -d -p 3000:80 --name feedback-app -v feedback:/app/feedback -v "$(pwd):/app" -v app/node_modules feedback-node:volumes
```

To have hot reload, just use nodemon.
A named volume can be shared across containers.
An anonymous volumne can't be reused, even on same image.
A named volume can be reused for same container.

### 56. RO volumes

You can add :ro to a volume to make it read only.

### Communication Docker with Hosts, APIS

Apis just work
Host, replace localhost with host.docker.internal
