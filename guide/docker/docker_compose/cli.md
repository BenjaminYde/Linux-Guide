# CLI

## Compose CLI Reference

All commands of docker compose can be found [here](https://docs.docker.com/engine/reference/commandline/compose_up/)

## Docker Hub

### docker compose pull

Pull service images.

```shell
docker compose pull --ignore-pull-failures
```

This command pulls the latest images for the services defined in the docker compose.yml file from the remote registry, and ignores images with pull failures (--ignore-pull-failures). This is useful for updating images without rebuilding them 

#### --ignore-pull-failures
Pull what it can and ignore images with pull failures.

## Information

### docker compose ps

List active containers.

```shell
docker compose ps -a
```

This command lists all containers, including stopped ones (-a). It provides a quick overview of the current state of the containers managed by Docker Compose.

#### -q
Display only container IDs.

#### -a
Show all containers, including stopped ones.

#### -f
Follow log output (live stream).

#### --tail=NUM
Output the last NUM lines.


### docker compose logs

View output from containers.

```shell
docker compose logs -f --tail=50 myservice
```

This command displays the log output from the myservice container. It follows the log output in real-time (-f), and only shows the last 50 lines (--tail=50). This is useful for monitoring and debugging applications.

### docker compose ls

List running compose projects.

```shell
docker compose ls -a
```

#### --all or -a
Show all compose projects, including stopped ones.

## Work With Compose

### docker compose up

Build, (re)create, start, and attach to containers for a service.

```shell
docker compose up -d --build
```
This command reads the **docker compose.yml** file, builds the images for the defined services if they don't exist or if the --build flag is used, creates and starts the containers in the background (-d flag), and detaches from them. This is a convenient way to start all services and their dependencies.

#### -d
Detach and run containers in the background.
#### --build
Build images before starting containers.

#### --scale SERVICE=NUM 
Scale the specified service to the desired number of replicas.

### docker compose run
Run a one-off command on a service.

```shell
docker compose run --rm myservice python manage.py migrate
```

This command runs the python manage.py migrate command inside a new myservice container and removes the container after the command has finished executing (--rm). This is useful for running one-off tasks or scripts that should not interfere with the primary service container.

#### --rm
Remove container after run.

#### --no-deps
Do not start linked services.

### docker compose down

Stop and remove containers, networks, images, and volumes.

```shell
docker compose down --rmi all -v
```

This command stops and removes containers, networks, all images used by any service (--rmi all), and named volumes (-v).        
It is useful for cleaning up the environment and starting fresh.

#### --rmi all
Remove all images used by any service.

#### -v
Remove named volumes.

### docker compose build

Build or rebuild services.

```shell
docker compose build --no-cache myservice
```
This command builds (or rebuilds) the myservice image without using cache (--no-cache).     
It is helpful when you want to force a new build from scratch or when you've made changes to the Dockerfile or the context.

#### --force-rm
Always remove intermediate containers.

#### --no-cache
Do not use cache when building the image.

#### --pull
Always attempt to pull a newer version of the image.

### docker compose stop
Stop services.

```shell
docker compose stop myservice
```

This command stops the myservice container without removing it. You can later start the container again using docker compose start.

#### SERVICE
Specify the service(s) to stop.

### docker compose start
Start existing containers for a service.

```shell
docker compose start myservice
```

This command starts the existing myservice container that was previously stopped. It is useful for resuming services without recreating containers.

#### SERVICE
Specify the service(s) to start.

### docker compose rm
Remove stopped containers.

```shell
docker compose rm -f -v
```

This command forcefully removes stopped containers (-f) and any anonymous volumes attached to them (-v). It is used for cleaning up stopped containers that are no longer needed.

#### -f
Don't ask to confirm removal.

#### -v
Remove any anonymous volumes attached to containers.

### docker compose kill

When you run docker-compose kill, it sends a **SIGKILL** signal to all the containers defined in your docker-compose.yml file, causing them to stop immediately. This is a more aggressive way of stopping the containers than using docker-compose down, which sends a **SIGTERM** signal to the containers and allows them to shut down gracefully.

```shell
docker compose kill -s SIGINT
```

#### --remove-orphans
If a container is stopped or removed outside of Docker Compose's control, it becomes an orphan container. This means that the container still exists on the system, but is not being managed by Docker Compose, and thus will not show up in `docker-compose ps`. This option will remove any orphan containers that are attached to the project-specific network.

#### --signal or -s
SIGNAL to send to the container. Default is SIGKILL

## Interaction

### docker compose exec

Execute a command in a running container.

```shell
docker compose exec myservice ls /app
```

This command executes the ls /app command inside the running myservice container. It allows you to run arbitrary commands inside a specific container, which is helpful for debugging or managing the application within the container.

#### -T
Disable pseudo-TTY allocation.

#### SERVICE COMMAND
Specify the service and command to be executed.