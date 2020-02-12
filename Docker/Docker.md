# Docker

[TOC]

Suggested Tutorial: [**[YOUTUBE]**](https://www.youtube.com/watch?v=fqMOX6JJhGo&t)

## What & Why

#### Importance

- Compatibility/Dependency among different OS & components.
- Long setup time by hand.
- Different Dev/Test/Prod Env.

#### Container

- An isolated environment.

#### Docker

- Docker can be run on different Linux Platform as they are all based on the same kernel in this case.
- Docker just utilize the underline Kernel which is able to work with all OSs above.
- Who does not share the same kernel? Windows.
  - When running docker on Windows, we are not really running the application on Windows' kernel, but on a Linux virtual machine under the hood.

#### Difference between Virtual Machine and Containers

**VM**: 

- Hardware Infra => Hypervisor => VM1, VM2, ...
- Higher utilization of underlying resources.
  - Consume higher disk space
- Heavy, usually be of gigabyte. (minutes to boot up)

**Container**: 

- Hardware Infra => OS => Docker => Container1, Container2, ...
- Lower utilization of underlying hardware
- Lightweight:) => Boot up faster(seconds).
- Every container gets a IP address but it's only accessible within the docker host.
- The docker container has its own isolated filesystem.
  - Once we delete a container, the files inside it gets blown away.
  - If we want to maintain the data, we need to map a directory **outside the container in the docker host** to a directory **inside the container**.

**Compare**:

- Containers share the same OS(little isolation). VMs share the hardware directly(complete isolation from each other).
- Unlike VM, containers are not meant to host an OS, but to run a specific task/process.

#### Images & Containers

- Docker Image: Package/Template
- Container: Running images in an isolated env.

## Learn Docker

#### Editions

- Community Edi.
- Enterprise Edi.
  - Image Security and so on.

####  Use Docker in Non-Linux Platform

- Use Linux VM
- Install Docker Desktop Distribution

#### Where to find images

`hub.docker.com`

#### Nvidia-Docker

Based on docker, nvidia-docker can leverage GPUs.

#### Quick Start

```shell
docker pull hello-world
docker run hello-world
```

#### Commands

![](https://1.bp.blogspot.com/-CBvM4xdqeVo/XHvu18li9bI/AAAAAAAAGH4/Ltbb4TMaRMwGTiCRaNmfZA65iSEkOa9dgCLcBGAs/s1600/dockercommand.png)

[[Credit]](https://marcus116.blogspot.com/2019/03/cheatsheets-docker-commands-diagram.html)

```shell
sudo docker version # U'll only get part of the information without a sudo prefix.

sudo docker run docker/whalesay cowsay Hello-World!

# Docker Run Command
# docker run xxx
# If xxx is not installed it will go to the hub.docker.com to install it:)

# List containers
docker ps

# See all containers running or not.
docker ps -a

# Details of a container
docker inspect NAME

# Stop a container
docker stop NAME/ID

# Remove a container permanently
docker rm NAME/ID

# List images
docker images

# Remove an image permanently
docker rmi NAME # Must ensure no one is using(dependent to) it.

# Download an image without running it
docker pull NAME

# Execute command in the docker container
docker exec NAME COMMAND

# Attach / Detach
docker run NAME # Attached to current console
# ctrl-C to exit.

docker run -d NAME # Run the container in a detached mode.

docker attach NAME/ID

# Specify the version using `tag`
docker run redis:4.0 # Using 4.0v
# If you don't specify your tag, the default tag will be the latest.

# By def., the container is not able to listen to the std input even attached to current console.
# If we want it to listen to our input, we must map the std input to the docker container using `-i` parameter. (i means "interative")

# Just the same as the input setting, use `-t` for some prompt in your program.

# Mapping directories(Volume Mapping)
docker run -v DOCKER_HOST_DIR:CONTAINER_DIR NAME
```

#### How Can Users access to docker?

1. Every docker container has an internal IP addr which is only accessible **within the docker host**.
2. Map current port to the docker container.

```shell
docker run -p 80:5000 NAME
# LOCAL 80 to DOCKER 5000
```



#### CheatSheat

![](https://i.pinimg.com/originals/8e/4c/27/8e4c27798d7c4d2cb448d836a7fa317a.png)

[[Credit]](https://www.pinterest.com/pin/66709638212926836/)

## Learn by Examples

#### Start a container

```shell
docker run -it --rm\
NAME\
bash
# -it : interactive mode
# --rm: delete it after using
# bash: the shell used in interactive mode
```

#### Run a detached container while viewing its output

```shell
docker run -d ubuntu:18.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
docker container log
```

#### Stop a container then restart it

```shell
docker container stop
docker container start
# Or just use: docker container restart
```

#### Interation in detached mode

```shell
docker run -dit ubuntu
docker container ls # Or docker ps
docker exec -i ID COMMAND
# Generally, we can use `docker exec  zsh`
```

#### Export containers

```shell
docker ps -a # Find out the container you want to export
docker export ID > export_name.tar
```

#### Import snapshots

```shell
cat export_name.tar | docker import - folder/name:version
# e.g. : `cat ubuntu.tar | docker import - test/ubuntu:v1.0`
docker ps
```
