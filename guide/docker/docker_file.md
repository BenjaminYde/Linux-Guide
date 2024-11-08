# The Dockerfile

## What is a Dockerfile?

A Dockerfile is a script containing a set of instructions used to create a Docker image. It defines the base image, configurations, dependencies, and application files required to build a fully functional container. Dockerfiles help automate and standardize the container creation process, ensuring consistent and reproducible results.

![Img](static/DockerFileOverview.png)

Creating a Dockerfile is as easy as creating a new file named “Dockerfile” with your text editor of choice and defining some instructions. The name of the file
does not really matter. Dockerfile is the default name but you can use any filename that you want (and even have multiple dockerfiles in the same folder)

## Anatomy of a Dockerfile

A Dockerfile consists of instructions, one per line, executed in the order they appear. Some common instructions include:

- **FROM**: he specified image is usually obtained from a public or private Docker registry. By default, Docker will look for the specified image on Docker Hub (https://hub.docker.com/), but you can also specify a different registry or a local image. The FROM instruction should be the first instruction in a Dockerfile.
- **RUN**:  Executes a command during the image build process. The command can be any valid shell command or script. The RUN instruction creates a new layer in the image, and the resulting state of the container file system is saved as a new image layer. You can chain multiple RUN instructions together to create a multi-step build process.
- **COPY**: Copies files or directories from the host system into the image. The source file or directory is specified as a path relative to the build context, and the destination is specified as a path within the image file system. The COPY instruction can copy multiple files or directories at once, and supports copying entire directories recursively.
- **ADD**: Similar to COPY, but with additional features. The ADD instruction can copy files or directories from the build context like COPY, but it can also fetch remote files or extract archives. However, the ADD instruction can be less predictable than COPY, and is not recommended for copying local files or directories.
- **WORKDIR**: Sets the working directory for subsequent instructions. The WORKDIR instruction creates a new directory within the image file system and sets it as the current working directory for any RUN, CMD, ENTRYPOINT, COPY, and ADD instructions that follow.
- **ENV**: Sets environment variables within the image. The ENV instruction sets one or more key-value pairs as environment variables within the container. These environment variables can be used by processes running within the container.
- **ARG**: Defines a variable that users can pass at build-time to the builder with the docker build command. The ARG instruction defines a variable that can be used within the Dockerfile as a shell variable. The value of the ARG variable can be passed in at build time using the --build-arg flag when running the docker build command.
- **VOLUME**: Declares a mount point with the specified name and prepares the volume for use with Docker
- **USER**: Sets the user or UID that should be used to run the container created from the image. This instruction can be used to ensure that the container is not run with root privileges, which can improve security.
- **LABEL**: Adds metadata to an image in the form of key-value pairs. This information can be used to provide additional information about the image or to help with organization and management. For example, labels could be used to specify the version of the software contained in the image, the maintainer of the image, or the purpose of the image. Labels can be viewed with the "docker inspect" command.
- **EXPOSE**: Informs Docker that the container will listen on specified network ports at runtime. The EXPOSE instruction does not actually publish the port to the host system, it just documents the intended usage of the container. To publish ports, you need to use the "-p" or "-P" options with the "docker run" command.
- **CMD**: Specifies the default command to execute when the container starts. The CMD instruction can be used to specify the command and any arguments that should be run in the container. If the user specifies a command when running the container, that command will override the CMD instruction. You can only have one CMD instruction in a Dockerfile.
- **ENTRYPOINT**: Configures the container to run as an executable, allowing it to accept arguments. The ENTRYPOINT instruction specifies the command to run when the container starts. Unlike CMD, the ENTRYPOINT command cannot be overridden by a command specified when running the container. Any arguments passed to the container are passed as arguments to the ENTRYPOINT command. You can also specify additional arguments for the ENTRYPOINT command using the "CMD" instruction.

### Example of a Dockerfile

```dockerfile
# Use the official Node.js base image
FROM node:14

# Set the working directory
WORKDIR /usr/src/app

# Copy package.json and package-lock.json into the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the application source code into the working directory
COPY . .

# Expose the application port
EXPOSE 8080

# Start the application
CMD ["npm", "start"]
```

## Build a Docker image from a Dockerfile

To build a Docker image from a Dockerfile, navigate to the directory containing the Dockerfile and run the following command:

```sh
docker build <target-docker-folder> 
```

Build a Docker image from a Dockerfile that is located via a filepath of url.<br/>
More info [here](https://docs.docker.com/engine/reference/commandline/build/)

#### -t 

give the image a name and optional a tag 

```sh
docker build -t <my-image-name> <target-docker-folder> 
```

#### --rm

When you build a Docker image, the build process may involve creating multiple intermediate containers for each instruction in the Dockerfile. These intermediate containers are used to cache the results of each layer, which can significantly speed up subsequent builds by reusing these cached layers. However, these intermediate containers can also consume a considerable amount of disk space.

with this command, you instruct Docker to automatically remove the intermediate containers once the build has successfully finished. <br/>
This helps to keep your system clean and conserve disk space.

```sh
docker build -t <my-image-name> --rm <target-docker-folder> 
```

#### -f

name of the dockerfile to build (default is `Dockerfile`).

```sh
docker build -f <my-dockerfile-name> <target-docker-folder> 
```

## .dockerignore file

A .dockerignore file is a special file used in Docker projects to specify files and directories that should be excluded from the build context when building a Docker image. It functions similarly to a .gitignore file, which is used to exclude files and directories from being tracked by Git.

The purpose of a .dockerignore file is to help reduce the size of the Docker build context and prevent unnecessary files from being included in the Docker image, which can slow down the build process and result in larger images. By excluding files that are not needed for the application to run, you can optimize the build process and create smaller, more efficient Docker images.

Here's an example of a simple .dockerignore file:

```
# Ignore all log files
*.log

# Ignore all .git files and directories
.git
.gitignore

# Ignore node_modules directory
node_modules

# Ignore temporary files
*.tmp
*.swp
```