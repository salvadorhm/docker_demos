# 🐳 Docker Basic Commands Cheatsheet

This document provides a basic guide to essential Docker commands, categorized for easy reference. Docker is a platform that enables developers to package applications into containers—standardized executable components combining application source code with the operating system (OS) libraries and dependencies required to run that code in any environment.

---

![Docker Cheat Sheet](/images/docker.svg)

## 📦 Containers

Containers are lightweight, portable, and self-sufficient units that encapsulate an application and its dependencies.

### `docker run`
* **Description:** Creates and starts a new container from an image. It's often the first command you'll use to get an application running in Docker.
* **Basic Usage:** `docker run [IMAGE]`
* **Examples:**
    * `docker run ubuntu`: Runs a container from the latest Ubuntu image.
    * `docker run --name my-nginx nginx`: Runs a container named `my-nginx` from the `nginx` image.
    * `docker run -p 8080:80 my-web-app`: Runs a container and maps host port 8080 to container port 80, useful for web applications.
    * `docker run -P my-web-app`: Maps all exposed container ports to random host ports.
    * `docker run --hostname my-host my-image`: Sets the hostname inside the container.
    * `docker run -v /host/path:/container/path my-image`: Mounts a host directory as a volume inside the container.
    * `docker run -it my-image bash`: Runs a container in interactive mode with a pseudo-TTY, often used to get a shell inside the container.
    * `docker run --network host my-app`: Runs a container using the host's network stack (the container will share the host's network interfaces and IP address).

### `docker ps`
* **Description:** Lists running containers. Similar to `ps` on Linux, but for Docker containers.
* **Basic Usage:** `docker ps`
* **Example:**
    * `docker ps`: Shows all currently running containers.
    * `docker ps -a`: Shows all containers (running and stopped).

### `docker rm [CONTAINER_ID_OR_NAME]`
* **Description:** Removes one or more stopped containers.
* **Basic Usage:** `docker rm [CONTAINER_ID_OR_NAME]`
* **Example:**
    * `docker rm my-nginx`: Removes the container named `my-nginx`.
    * `docker rm $(docker ps -aq)`: Removes all stopped containers.

### `docker container prune`
* **Description:** Removes all stopped containers.
* **Basic Usage:** `docker container prune`
* **Example:**
    * `docker container prune`: Prompts for confirmation and then removes all stopped containers.

---

## ⚙️ Manage Images

Docker images are read-only templates used to create containers. They contain the application code, libraries, dependencies, and all other files needed for the application to run.

### `docker pull [IMAGE:TAG]`
* **Description:** Downloads an image from a Docker registry (e.g., Docker Hub) to your local machine.
* **Basic Usage:** `docker pull [IMAGE:TAG]`
* **Example:**
    * `docker pull ubuntu:latest`: Pulls the latest Ubuntu image.
    * `docker pull my-repo/my-image:1.0`: Pulls a specific version of an image from a private repository.

### `docker images`
* **Description:** Lists all images stored locally.
* **Basic Usage:** `docker images`
* **Example:**
    * `docker images`: Displays a list of all local Docker images.

### `docker rmi [IMAGE_ID_OR_NAME]`
* **Description:** Removes one or more images from the local storage. You cannot remove an image if a container is using it.
* **Basic Usage:** `docker rmi [IMAGE_ID_OR_NAME]`
* **Examples:**
    * `docker rmi ubuntu:latest`: Removes the Ubuntu latest image.
    * `docker rmi 1234567890ab`: Removes the image with the specified ID.

### `docker build`
* **Description:** Builds a new image from a Dockerfile. The Dockerfile contains instructions for assembling the image.
* **Basic Usage:** `docker build -t [IMAGE_NAME:TAG] .`
* **Example:**
    * `docker build -t my-custom-app:1.0 .`: Builds an image named `my-custom-app` with tag `1.0` from the Dockerfile in the current directory.

### `docker save [IMAGE] > [NAME.tar]`
* **Description:** Saves one or more images to a tar archive (a single file). Useful for transferring images between machines without a registry.
* **Basic Usage:** `docker save [IMAGE] > [NAME.tar]`
* **Example:**
    * `docker save my-image:latest > my-image.tar`: Saves `my-image:latest` to `my-image.tar`.

### `docker load -i [NAME.tar]`
* **Description:** Loads images from a tar archive into the Docker image store.
* **Basic Usage:** `docker load -i [NAME.tar]`
* **Example:**
    * `docker load -i my-image.tar`: Loads the image from `my-image.tar`.

### `docker image prune`
* **Description:** Removes unused (dangling) images. Dangling images are those that are not tagged and not referenced by any container.
* **Basic Usage:** `docker image prune`
* **Example:**
    * `docker image prune`: Prompts for confirmation and removes dangling images.
    * `docker image prune -a`: Removes all unused images (including those with tags but not associated with a container).

---

## 🔄 Manage Containers

These commands help you interact with and manage the lifecycle of your Docker containers.

### `docker exec -it [CONTAINER_NAME] [COMMAND]`
* **Description:** Executes a command inside a running container. `-it` is often used to get an interactive shell.
* **Basic Usage:** `docker exec -it [CONTAINER_NAME] [COMMAND]`
* **Example:**
    * `docker exec -it my-nginx bash`: Opens a bash shell inside the `my-nginx` container.

### `docker cp [SOURCE_PATH] [DESTINATION_PATH]`
* **Description:** Copies files/folders between a container and the local filesystem.
* **Basic Usage:**
    * `docker cp [HOST_PATH] [CONTAINER_NAME]:[CONTAINER_PATH]`: Copy from host to container.
    * `docker cp [CONTAINER_NAME]:[CONTAINER_PATH] [HOST_PATH]`: Copy from container to host.
* **Example:**
    * `docker cp my_file.txt my-container:/app/`: Copies `my_file.txt` from the host to `/app/` inside `my-container`.
    * `docker cp my-container:/app/log.txt .`: Copies `log.txt` from `/app/` inside `my-container` to the current host directory.

### `docker rename [OLD_NAME] [NEW_NAME]`
* **Description:** Renames an existing container.
* **Basic Usage:** `docker rename [OLD_NAME] [NEW_NAME]`
* **Example:**
    * `docker rename old-web-server new-web-server`: Renames the container.

### `docker commit [CONTAINER_NAME] [NEW_IMAGE_NAME:TAG]`
* **Description:** Creates a new image from the changes made to a running container. This is useful for saving the current state of a container.
* **Basic Usage:** `docker commit [CONTAINER_NAME] [NEW_IMAGE_NAME:TAG]`
* **Example:**
    * `docker commit my-container my-modified-image:v1`: Creates `my-modified-image:v1` from `my-container`.

### `docker start [CONTAINER_NAME]`
* **Description:** Starts one or more stopped containers.
* **Basic Usage:** `docker start [CONTAINER_NAME]`
* **Example:**
    * `docker start my-nginx`: Starts the stopped `my-nginx` container.

### `docker stop [CONTAINER_NAME]`
* **Description:** Stops one or more running containers. Docker sends a SIGTERM signal to the container's main process, allowing it to shut down gracefully.
* **Basic Usage:** `docker stop [CONTAINER_NAME]`
* **Example:**
    * `docker stop my-nginx`: Stops the running `my-nginx` container.

---

## ℹ️ Info & Stats

These commands provide information and real-time statistics about your Docker environment and running containers.

### `docker inspect [CONTAINER_NAME]`
* **Description:** Returns low-level information about Docker objects (containers, images, volumes, networks, etc.) in JSON format.
* **Basic Usage:** `docker inspect [CONTAINER_NAME]`
* **Example:**
    * `docker inspect my-nginx`: Displays detailed information about the `my-nginx` container.

### `docker port [CONTAINER_NAME]`
* **Description:** Lists port mappings for a container.
* **Basic Usage:** `docker port [CONTAINER_NAME]`
* **Example:**
    * `docker port my-web-app`: Shows the port mappings for `my-web-app`.

### `docker top [CONTAINER_NAME]`
* **Description:** Displays the running processes of a container. Similar to the `top` command in Linux.
* **Basic Usage:** `docker top [CONTAINER_NAME]`
* **Example:**
    * `docker top my-nginx`: Shows the processes running inside `my-nginx`.

### `docker stats`
* **Description:** Displays a live stream of resource usage statistics for running containers (CPU, memory, network I/O, block I/O).
* **Basic Usage:** `docker stats`
* **Example:**
    * `docker stats`: Shows real-time resource usage for all running containers.

