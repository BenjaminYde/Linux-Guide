# Docker Swarm

## Orchestration

The portability and reproducibility of a containerized process provides an opportunity to move and scale our containerized applications across clouds and datacenters. Containers effectively guarantee that those applications run the same way anywhere, allowing us to quickly and easily take advantage of all these environments. Additionally, as we scale our applications up, we need some tooling to help automate the maintenance of those applications, enable the replacement of failed containers automatically, and manage the rollout of updates and reconfigurations of those containers during their lifecycle.

Tools to manage, scale, and maintain containerized applications are called orchestrators, and the most common examples of these are **Kubernetes** and **Docker Swarm**.

## About Docker Swarm

Docker Swarm is a native container orchestration tool provided by Docker that allows you to create, manage, and scale services across multiple Docker nodes. It uses the standard Docker API, making it easy to integrate with your existing Docker workflow. With Docker Swarm, you can deploy your application across a cluster of Docker nodes, ensuring high availability, fault tolerance, and load balancing.

### Key Components of Docker Swarm

- **Swarm**: A swarm is a group of Docker nodes that are participating in the cluster. It consists of one or more manager nodes and worker nodes.

- **Manager Nodes**: Manager nodes are responsible for maintaining the cluster state, orchestrating services, and managing the swarm. There is always at least one manager node in a swarm, and it can be configured for high availability by having multiple manager nodes.

- **Worker Nodes**: Worker nodes are responsible for running containers. They receive tasks from manager nodes and execute them. Worker nodes communicate with the manager nodes to report the status of the tasks.

- **Services**: A service is a high-level abstraction of your application, which is defined by a Docker image and various configuration options, such as the desired number of replicas, ports, and network settings.

- **Tasks**: A task is a single instance of a container running on a worker node. Tasks are created and managed by the manager nodes and executed by the worker nodes.

- **Load Balancing**: Docker Swarm provides built-in load balancing for services. Services can be exposed on a specific port or through a routing mesh, which automatically routes traffic to the appropriate container.

## Deployment: Docker Stack

Swarm never creates individual containers like we did in the previous step of this tutorial. Instead, all Swarm workloads are scheduled as services, which are scalable groups of containers with added networking features maintained automatically by Swarm. Furthermore, all Swarm objects can and should be described in manifests called stack files. These YAML files describe all the components and configurations of your Swarm app, and can be used to easily create and destroy your app in any Swarm environment. These are very similar to **docker-compose.yml** files.

## Docker Compose vs Docker Stack

Docker Compose and Docker Stack are both tools for managing multi-container applications in Docker, but they have different scopes and use cases. Here's a comparison between the two:

#### Docker Compose 

- Docker Compose is a tool for defining and running multi-container Docker applications on a single host. It is mainly used for local development and testing.
- It uses a YAML file (usually named docker-compose.yml) to define services, networks, and volumes for an application.
- Docker Compose does not provide built-in support for clustering or orchestration; it's focused on single-host environments.
- It does not offer features like automatic scaling, load balancing, or rolling updates out-of-the-box.

#### Docker Stack

- Docker Stack is a feature in Docker Swarm mode, which is a native clustering and orchestration solution for Docker. It's used for deploying and managing multi-container applications across multiple hosts in a Swarm cluster.
- Docker Stack also uses a YAML file (usually named docker-compose.yml), but with some additional, Swarm-specific options.
- It provides built-in support for clustering, orchestration, and high availability, allowing you to deploy applications in distributed environments.
- Docker Stack offers features like automatic scaling, load balancing, rolling updates, and service resiliency out-of-the-box.

In summary, Docker Compose is suitable for single-host environments, such as local development and testing, while Docker Stack is designed for multi-host environments, like deploying and managing applications in a production setting using Docker Swarm. Both tools use similar syntax and file formats, which makes it easy to transition from Docker Compose to Docker Stack when moving from development to production.

## Docker Stack

Docker stack is a group of interrelated services that share dependencies, and can be orchestrated and scaled together in a Docker Swarm environment. A stack is defined using a Docker Compose file, and the services within the stack are managed collectively, making it an efficient tool for managing multi-service applications.

In short, the advantages:

- **Can use docker-compose.yaml** like with docker compose
- Cannot use build property int the .yaml file, this forces you to use **pre-build images**, which is good
  - Combine this with the Harbor container registry which contains all the images
  - Building on target devices is a bad practice, device can be slow + all the code needs to be on the device...
- Can **use external docker secrets** instead of .txt files which are not secure!
- **Easy to manage** docker stacks using the docker stack cli
- **Easy to scale services**

#### Using Docker stack

- Initialize the docker daemon as swarm.
```bash
docker swarm init
```
- Create the docker-compose.yaml
- Deploy the stack
```bash
docker stack deploy -c docker-compose.yml <stack-name>
```
- Manage the stack
  - `docker stack ls`
  - `docker stack services`
  - `docker stack rm`
  - ...
