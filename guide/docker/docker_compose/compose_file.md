# Compose File

## Application Model

The Compose specification allows one to define a platform-agnostic container based application. Such an application is designed as a set of containers which have to both run together with adequate shared resources and communication channels.

**Services** are components of an application. A Service is an abstract concept implemented on platforms by running the same container image (and configuration) one or more times.

Services communicate with each other through **Networks**. In this specification, a Network is a platform capability abstraction to establish an IP route between containers within services connected together.

Services store and share persistent data into **Volumes**, persistent data as a high-level filesystem mount with global options.

A **Secret** is sensitive data that SHOULD NOT be exposed without security considerations. Secrets are made available to services as files mounted into their containers or as external (when docker swarm is enabled, = recommended).

**Configs** are data that is dependent on the runtime or platform that can be required for some services. From a Service container point of view, Configs are comparable to Volumes, in that they are files mounted into the container. But the actual definition involves distinct platform resources and services, which are abstracted by this type.

> [!NOTE]  
> - More about **services** elements [here](https://docs.docker.com/compose/compose-file/05-services/)
> - More about **network** elements [here](https://docs.docker.com/compose/compose-file/06-networks/)
> - More about **volumes** elements [here](https://docs.docker.com/compose/compose-file/07-volumes/)
> - More about **secrets** elements [here](https://docs.docker.com/compose/compose-file/09-secrets/)
> - More about **configs** elements [here](https://docs.docker.com/compose/compose-file/08-configs/)
> - More about **deploy** elements [here](https://docs.docker.com/compose/compose-file/deploy/)

## Application Example

The following example illustrates Compose specification concepts with a concrete example application.

- An application split into a frontend web application and a backend service.
- The frontend is configured at runtime with an HTTP configuration file managed by infrastructure, providing an external domain name, and an HTTPS server certificate injected by the platform’s secured secret store.
- The backend stores data in a persistent volume.
- Both services communicate with each other on an isolated back-tier network, while frontend is also connected to a front-tier network and exposes port 443 for external usage.

```
(External user) --> 443 [frontend network]
                            |
                  +--------------------+
                  |  frontend service  |...ro...<HTTP configuration>
                  |      "webapp"      |...ro...<server certificate> #secured
                  +--------------------+
                            |
                        [backend network]
                            |
                  +--------------------+
                  |  backend service   |  r+w   ___________________
                  |     "database"     |=======( persistent volume )
                  +--------------------+        \_________________/
```

The example application is composed of the following parts:

- 2 services, backed by Docker images: webapp and database
- 1 secret (HTTPS certificate), injected into the frontend
- 1 configuration (HTTP), injected into the frontend
- 1 persistent volume, attached to the backend
- 2 networks

```yaml
services:
  frontend:
    image: awesome/webapp
    ports:
      - "443:8043"
    networks:
      - front-tier
      - back-tier
    configs:
      - httpd-config
    secrets:
      - server-certificate

  backend:
    image: awesome/database
    volumes:
      - db-data:/etc/data
    networks:
      - back-tier

volumes:
  db-data:
    driver: flocker
    driver_opts:
      size: "10GiB"

configs:
  httpd-config:
    external: true

secrets:
  server-certificate:
    external: true

networks:
  # The presence of these objects is sufficient to define them
  front-tier: {}
  back-tier: {}
```

Here a more visual example of what docker compose does:

![Img](./static/ArchitectureOverview.png)

## Basic Usage

1. Create your `docker-compose.yml`
2. Open the terminal and go into the directory where the compose file is located
3. Run `docker compose up` command from the same directory location
4. Check if the compose instance is runner with the command `docker compose ps` or `docker ps`
5. Stop and remove the containers `docker compose down`

## Compose-File Syntax

A docker-compose.yml file is organized into several **top-level** sections:

| Directive    | Use                                                                                                                                                 |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **version**  | Specifies the Compose file syntax version.                                                                                                          |
| **services** | In Docker a service is the name for a container. This section defines the containers that will be started as a part of the Docker Compose instance. |
| **networks** | You can change the settings of the default network, connect to an external network, or define app-specific networks.                                |
| **volumes**  | Mounts a linked path on the host machine that can be used by the container.                                                                         |
| **configs**  | Configs are data that is dependent on the runtime or platform that can be required for some services.                                               |
| **secrets**  | Secrets are made available to services as files mounted into their containers or as external                                                        |

Inside the **services** section you can use the following child element structures:

| Directive       | Use                                                                                                                                                                |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **image**       | Sets the image that will be used to build the container. Using this directive assumes that the specified image already exists either on the host or on Docker Hub. |
| **build**       | This directive can be used instead of image. Specifies the location of the Dockerfile that will be used to build this container.                                   |
| **restart**     | Tells the container to restart if the system restarts.                                                                                                             |
| **volumes**     | Mounts a linked path on the host machine that can be used by the container                                                                                         |
| **environment** | Define environment variables to be passed in to the Docker run command.                                                                                            |
| **depends_on**  | Sets a service as a dependency for the current block-defined container                                                                                             |
| **port**        | Maps a port from the container to the host in the following manner: host:container                                                                                 |
| **links**       | Link this service to any other services in the Docker Compose file by specifying their names here.                                                                 |

### Services

Each entry in the services section will create a separate container when docker-compose is run.     
At this point, the section contains a single container based on the official Alpine distribution:

```yaml
version: '3'

services:
  distro:
    image: alpine
    restart: always
    container_name: MyCoolContainer
    stdin_open: true # docker run -i
    tty: true        # docker run -t
```

- The `restart` directive is used to indicate that the container should always restart (after a crash or system reboot, for example).
- The `container_name` directive is used to override the randomly generated container name and replace it with a name that is easier to remember and work with.
- Docker containers exit by default if no process is running on them
- `tty` or Pseudo-TTYs are used to run commands inside a container. (similar to docker run with the -t option). The container will not exit until the session ends.
  - If we want to interact with the container, we can couple this with the `stdin_open` option. (similar to docker run with the -i option) This will allow us to run commands in the container using our terminal.
  - Never-ending commands were a hack a long time ago when no other options existed. However, in the latest versions of Docker, it is possible to keep containers running by starting a terminal session with them both in the foreground and in the background. `-it` is an alternative to `entrypoint: tail -f /dev/null`

#### Add Addition Services

From here you can begin to build an ecosystem of containers. You can define how they work together and communicate.

```yaml
version: '3'

services:
  distro:
    image: alpine
    restart: always
    container_name: MyCoolContainer

  database:
    image: postgres:latest
    container_name: postgres_db
    volumes:
      - ../dumps:/tmp/
    ports:
      - "5432:5432"
```

There are now two services defined:
- Distro
- Database

### Volumes

Storing PostgreSQL data directly inside a container is not recommended. Docker containers are intended to be treated as ephemeral: your application’s containers are built from scratch when running `docker-compose up` and destroyed when running `docker-compose down`. In addition, any unexpected crash or restart on your system will cause any data stored in a container to be lost.

For these reasons it is important to set up a persistent volume on the host that the database containers will use to store their data.

Add a volumes section to docker-compose.yml and edit the database service to refer to the volume:

```yaml
version: '3'

services:
  distro:
    image: alpine
    container_name: Alpine_Distro
    restart: always
    entrypoint: tail -f /dev/null

  database:
    image: postgres:latest
    container_name: postgres_db
    volumes:
      - data:/var/lib/postgresql
    ports:
      - "5432:5432"

volumes:
  data: {}
```

#### Volume Types

##### Normal Volume

A normal volume is a simple way to create a persistent storage space for a Docker container. By specifying only a container path in the volumes section of the docker-compose.yml file, Docker Engine will automatically create a new volume and attach it to the specified path inside the container. The data stored in this volume will persist across container restarts and removals, but the volume itself will not be linked to a specific location on the host machine.

```yaml
volumes:
  # Just specify a path and let the Engine create a volume
  - /var/lib/mysql
```

##### Path Mapping

Path mapping is the process of mapping a specific directory on the host machine to a path inside the container. This allows you to share data between the host and the container, which can be useful for tasks such as developing applications, storing configuration files, or logging. To create a path mapping, you specify both the host path and the container path, separated by a colon (:).

```yaml
volumes:
  # Specify a host path and map it to a container path
  - /opt/data:/var/lib/mysql
```

#### Named Volume

A named volume is a type of volume that has a specific name, making it easier to use across multiple containers and services. Named volumes are useful for sharing data between containers or for managing multiple volumes with more descriptive names. To create a named volume, you specify the volume name followed by a colon (:) and the container path. Named volumes are defined separately under the volumes key in the docker-compose.yml file.

```yaml
services:
  your_service:
    image: your_image
    volumes:
      # Use a named volume and map it to a container path
      - datavolume:/var/lib/mysql

volumes:
  # Define the named volume
  datavolume: {}
```

In this example, a named volume called datavolume is created and attached to the /var/lib/mysql path inside the container. The same named volume can be used by multiple services, allowing data sharing between them.

### Environment Variables

Environment variables are used to bring configuration data into your applications. This is often the case if you have some configurations that are dependent on the host operating system or some other variable things that can change.

#### Setting an environment variable

You can set environment variables in a container using the "environment" keyword, just like with the normal docker container run --environment command in the shell.

```yaml
web:
  environment:
    - NODE_ENV=production
```

#### Passing an environment variable

You can pass environment variables from your shell straight to a container by just defining an environment key in your Compose file and not giving it a value.
Here the value of NODE_ENV is taken from the value from the same variable in the shell which runs the Compose file.

```yaml
web:
  environment:
    - NODE_ENV
```

#### Using an .env file

Sometimes a few environment variables aren't enough and managing them in the Compose file can get pretty messy. That is what .env files are for. They contain all the environment variables for your container and can be added using one line in your Compose file.

```yaml
web:
  env_file:
    - variables.env
```

### Dependencies

Dependencies in Docker are used to make sure that a specific service is available before the dependent container starts. This is often used if you have a service that can't be used without another one e.g. a CMS (Content Management System) without its database.

```yaml
ghost:
        container_name: ghost
        restart: always
        image: ghost
        ports:
            - 2368:2368
        environment:
            - .
        depends_on: [db]
    db:
        image: mysql
        command: --default-authentication-plugin=mysql_native_password
        restart: always
        environment:
            MYSQL_ROOT_PASSWORD: example
```
Here we have a simple example of a Ghost CMS which depends on the MySQL database to work and therefore uses the depends_on command. The depends_on command takes an array of string which defines the container names the service depends on.

### Networking 

Networks define the communication rules between containers, and between containers and the host system. They can be configured to provide complete isolation for containers, which enables building applications that work together securely.

By default Compose sets up a single network for each container. Each container is automatically joining the default network which makes them reachable by both other containers on the network, and discoverable by the hostname defined in the Compose file.

### Ports

Exposing the ports in Compose works similarly as in the Dockerfile. We differentiate between two different methods of exposing the port:

#### Exposing the port to linked services:

```yaml
expose:
 - "3000"
 - "8000"
```

Here we publish the ports to the linked services of the container and not to the host system.

#### Exposing the port to the host system

```yaml
ports:
  - "8000:80"  # host:container
```

In this example, we define which port we want to expose and the host port it should be exposed to.

You can also define the port protocol which can either be UDP or TCP:

```yaml
ports:
  - "8000:80/udp"
```