# Docker CLI

## Docker command options

Docker options are additional flags that can be added to Docker commands to modify their behavior. For example, the `-p` option can be used with the `docker run` command to specify the ports that should be exposed on the container. Another example is the `--rm` option, which can be used with the `docker run` command to automatically remove the container when it stops running.

#### example syntax

```sh
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

```sh
docker run --rm -d -it --name <my-image-name> <image>
```

## Docker Hub

### docker login

> [!CAUTION]
> TODO

### docker pull

Download an image from a registry.
More info [here](https://docs.docker.com/engine/reference/commandline/pull/)

If no tag is provided, Docker Engine uses the `:latest` tag as a default. This example pulls the `ubuntu:latest` image:

```sh
docker pull ubuntu:22.04
```

### docker push

> [!CAUTION]
> TODO


## Docker Image And Container Info

### docker version

We usually start by finding the installed version of docker that we are working on

``` shell
docker version
```

### docker images

Lists all images with their image id, repo, tags and size.
More info [here](https://docs.docker.com/engine/reference/commandline/images/)

```sh
docker images
```

### docker ps 

List all running docker containers 

#### -a
List all containers, including stopped
 
### docker container ls 

List all running docker containers 

#### -a
List all containers, including stopped

## Work With Images And Containers

### docker build 

Build a Docker image from a Dockerfile that is located via a filepath of url.
More info [here](https://docs.docker.com/engine/reference/commandline/build/)

<ins>docker build from path context</ins>

```sh
docker build <target-docker-folder> 
```

#### -t 
give the image a name and optional a tag 
```sh
docker build -t <my-image-name> <target-docker-folder> 
```

#### --rm
remove intermediate containers after a successful build
```sh
docker build -t <my-image-name> --rm <target-docker-folder> 
```

#### -f
name of the dockerfile to build (default is `Dockerfile`).
```sh
docker build -f<my-dockerfile-name> <target-docker-folder> 
```

### docker run

This command is used to create and start a new container from a Docker image. It combines the docker create and docker start commands into a single step. 

If you specify the image by name, Docker will search for the image with that name in your local Docker registry. If it is not found locally, Docker will attempt to download the image from a remote registry (such as Docker Hub) before running the container. It is also possible to specify the image id but it is more readable to specify the image name.
More info [here](https://docs.docker.com/engine/reference/commandline/run/)

```sh
docker run [OPTIONS] <image-name> [ARGS]
```

#### --name
Specify the name of the container.

#### --no-pull
If you don't want Docker to automatically download the image from a remote registry if it's not found locally.

#### --rm
Used to automatically remove the container when it is stopped. This can be useful to avoid accumulating stopped containers on your system. 

#### -p or --publish
Pulish a container's port(s) to the host.
The example exposes port 8080 of the image to port 80 on the host.

```sh
docker run -p 8080:80 <image-name>
```

#### -d or --detach
When you run a Docker container with the `docker run` command, the container runs in the foreground by default, which means that the terminal that you used to start the container is attached to the container's input and output streams. This is useful for debugging and troubleshooting, but it's not ideal for long-running containers, as it ties up the terminal and prevents you from running other commands.

With this option, the container runs in the background, and Docker returns the container ID. You can use this ID to interact with the container later, if necessary.

### docker images 

List all images 

#### list images that contain name

```sh
docker images --format '{{.Repository}}:{{.Tag}}' | grep 'benjamin'
```

### docker start 

This command is used to start a stopped container. When you start a container with docker start, it will use the same configuration options that were specified when the container was created with docker run. 

### docker stop 

This command is used to stop a running container. When you stop a container with docker stop, Docker sends a SIGTERM signal to the container process, allowing it to perform any necessary cleanup tasks before shutting down. 

### docker rm 

This command is used to remove a stopped container. When you remove a container with docker rm, all of the container's data and configuration options are deleted. 

#### remove images that contain name

```sh
docker images --format '{{.Repository}}:{{.Tag}}' | grep 'benjamin' | xargs docker rmi -f
```

### docker tag

> [!CAUTION]
> TODO

## Container Interaction

### docker attach 

You can attach to the container's input and output streams.

```sh
docker attach <container-name>
```

### docker exec

Execute a command in a running container

```sh
docker exec <container-name> <my-command>
```

#### -i
This flag enables interactive mode, which allows you to interact with the container's STDIN (standard input). When this flag is used, Docker will keep the STDIN of the container open, allowing you to send input to the container and receive output from it. (input)

#### -t 
This flag allocates a pseudo-TTY (terminal), which allows you to enter and view output from the container as if you were working in a terminal. Without this flag, the output from the container would be printed in the console without any formatting or color-coding. (output)

## Cleanups

### docker image prune

> [!CAUTION]
> TODO

### docker container prune

> [!CAUTION]
> TODO
