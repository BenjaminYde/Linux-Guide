# Install

## Install Docker Engine

Docker Compose is a tool / plugin that works with Docker Engine.    
Make sure docker engine is installed. You can follow the steps [here](../docker_install.md).

## Install Compose Plugin On Linux

1. Install the package:
  ```shell
  sudo apt update
  sudo apt install docker-compose-plugin
  ```
2. Verfify the installation
  ```shell
  docker compose version
  ```

## Install Compose On Windows

Docker Desktop includes Docker Compose along with Docker Engine and Docker CLI which are Compose prerequisites.
If you have already installed Docker Desktop, you can check which version of Compose you have by selecting About Docker Desktop from the Docker menu.