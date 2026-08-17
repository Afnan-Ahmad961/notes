# Docker

> Auto-generated summaries from articles and videos. Last updated: 2026-08-18.

## Table of Contents

<!-- INDEX START -->
- [Mastering Docker: A Comprehensive Guide](#mastering-docker-a-comprehensive-guide) — *2026-08-18 03:00*
<!-- INDEX END -->

---


## Mastering Docker: A Comprehensive Guide

*Added: 2026-08-18 03:00*

**Source:** [https://www.youtube.com/watch?v=exmSJpJvIPs&t=7436s](https://www.youtube.com/watch?v=exmSJpJvIPs&t=7436s)

## Contents

- [Overview](#overview)
- [Why Docker?](#why-docker)
- [What is Docker?](#what-is-docker)
- [Docker Images vs. Containers](#docker-images-vs-containers)
- [Setting Up Docker Desktop](#setting-up-docker-desktop)
- [Docker Hub](#docker-hub)
- [Essential Docker Commands](#essential-docker-commands)
- [Image Layering and Versions](#image-layering-and-versions)
- [Port Binding](#port-binding)
- [Troubleshooting Docker Containers](#troubleshooting-docker-containers)
- [Docker vs. Virtual Machines](#docker-vs-virtual-machines)
- [Docker Networking Fundamentals](#docker-networking-fundamentals)
- [Dockerizing an Application with Dockerfile](#dockerizing-an-application-with-dockerfile)
- [Docker Compose](#docker-compose)
- [Publishing Docker Images to Docker Hub](#publishing-docker-images-to-docker-hub)
- [Docker Volumes for Data Persistence](#docker-volumes-for-data-persistence)
- [Advanced Docker Networking](#advanced-docker-networking)

## Overview

This lecture provides an end-to-end, in-depth guide to Docker, covering its core concepts, practical usage, and advanced features. By the end, learners will understand how to integrate Docker into their software development process, manage applications with containers, and deploy them efficiently.

## Why Docker?

Software development often faces the "It works on my machine" problem, especially in large teams or when deploying to production. This arises due to:

*   **Manual Dependency Installation:** Installing numerous dependencies (e.g., Node.js, MongoDB) manually on different systems (developer machines, production servers) is error-prone and time-consuming.
*   **Version Mismatches:** Applications might depend on specific versions of software (e.g., Node.js v16). If a new team member installs a newer version (e.g., Node.js v20), the application might break or behave unexpectedly.
*   **Environment Inconsistencies:** Differences in operating systems, system configurations, or CLI commands between development and production environments can lead to bugs.

Docker addresses these issues by providing a standardized, isolated, and portable way to package applications and their dependencies.

## What is Docker?

Docker is a platform that helps build and manage **containers**. To understand Docker, it's crucial to grasp two core concepts: Docker Images and Docker Containers.

*   **Docker Container:** A single, bundled unit that combines an application with all its dependencies into an isolated environment. It's a running instance of a Docker Image.
    *   **Portability:** Containers can be shared across different systems and operating systems (Mac, Windows, Linux) without compatibility issues.
    *   **Lightweight:** Containers have minimal overhead, making them quick to create, update, and delete. Their small size (compared to virtual machines) allows multiple containers to run efficiently on a single machine.
    *   **Isolated Environments:** Containers provide isolated environments, allowing different versions of the same dependency (e.g., Node.js v16 and v20) to run simultaneously on the same host machine for different applications.

## Docker Images vs. Containers

The relationship between a Docker Image and a Docker Container is analogous to that of a Class and an Object in object-oriented programming:

*   **Docker Image:** A static, executable file that contains instructions for building a container. It's a blueprint or a template.
    *   Images do not consume system resources (like CPU or RAM) when not running. Their size is generally small.
    *   A single Docker Image can be used to create multiple Docker Containers.
*   **Docker Container:** A running instance of a Docker Image.
    *   Containers are the actual running environments that use system resources.
    *   When you "share a container," you are technically sharing the Docker Image, which then allows others to create their own containers from it.

**Practical Example: Running an Ubuntu Container**

1.  **`docker run -it ubuntu`**: This command runs an Ubuntu container in interactive mode (`-it`), allowing access to its terminal.
2.  **Image Pull:** If the `ubuntu` image isn't locally available, Docker pulls it from Docker Hub.
3.  **Container Creation:** A container is created from the image.
4.  **Isolated Environment:** Inside the Ubuntu container's terminal, you can create files, directories, and run commands. These changes are isolated from your host machine's file system.
5.  **Exit:** Exiting the container's terminal stops the container.

## Setting Up Docker Desktop

Docker Desktop is an application that includes the Docker engine, CLI client, Docker Compose, and Kubernetes.

1.  **Download:** Visit `docker.com` and download Docker Desktop for your operating system (Mac, Windows, Linux).
2.  **Installation:** Follow the installation wizard. On Windows, it might require a system restart.
3.  **Verification:**
    *   Open Docker Desktop application; it should show containers and images tabs.
    *   Open your terminal/command prompt and run `docker --version` to check the installed Docker version.
    *   Run `docker` to see a list of available Docker commands.

## Docker Hub

Docker Hub (`hub.docker.com`) is a cloud-based registry service that hosts public and private Docker images. It's like GitHub for Docker images. You can find official images for various technologies (e.g., Ubuntu, MySQL, MongoDB) and push your own custom images.

## Essential Docker Commands

These commands are fundamental for managing Docker images and containers.

*   **`docker pull <image_name>[:<tag>]`**: Downloads a Docker image from Docker Hub to your local system. If no tag is specified, it defaults to `latest`.
    *   Example: `docker pull hello-world`
    *   Example: `docker pull ubuntu`
*   **`docker images`**: Lists all Docker images stored locally on your system. It shows image ID, tag, size, and creation date.
*   **`docker run <image_name>[:<tag>]`**: Creates and starts a new container from a specified image.
    *   If the image is not found locally, Docker will automatically pull it from Docker Hub.
    *   Example: `docker run hello-world` (creates, runs, and stops a container that prints a message).
    *   Example: `docker run -it ubuntu` (creates and runs an Ubuntu container in interactive mode, allowing terminal access).
        *   `-it`: Interactive and pseudo-TTY allocation, allowing interaction with the container's terminal.
*   **`docker ps`**: Lists all currently running containers.
*   **`docker ps -a`**: Lists all containers, including those that have stopped (`-a` for all).
    *   Shows container ID, image, command, creation time, status, ports, and name.
*   **`docker start <container_id_or_name>`**: Starts an existing, stopped container.
*   **`docker stop <container_id_or_name>`**: Stops a running container.
*   **`docker rm <container_id_or_name>`**: Removes a stopped container.
*   **`docker rmi <image_id_or_name>[:<tag>]`**: Removes a Docker image.
    *   An image cannot be removed if it is being used by any container (even stopped ones). You must remove the container first.

## Image Layering and Versions

*   **Tags/Versions:** Docker images can have different versions or variants, referred to as "tags" (e.g., `mysql:latest`, `mysql:8.0`).
*   **Layering:** Docker images are built in layers. When you pull different versions of the same image, Docker reuses common layers that already exist on your system, only downloading the unique layers for the new version. This makes pulling new versions efficient.
    *   Example: Pulling `mysql:latest` and then `mysql:8.0` will reuse many underlying layers.

## Port Binding

By default, Docker containers have their own isolated network stack and ports, separate from the host machine. **Port binding** (or port mapping) allows you to map a port on your host machine to a port inside a Docker container.

*   **Purpose:** To allow external access to services running inside a container.
*   **Command:** Use the `-p` (or `--publish`) flag with `docker run`.
    *   Syntax: `docker run -p <host_port>:<container_port> <image_name>`
    *   Example: `docker run -p 8080:3306 mysql` maps host port 8080 to container port 3306.
*   **Important Note:** A host port can only be mapped to one container at a time. If you try to map host port 8080 to a second container while it's already in use, you'll get an error. The container's internal port (e.g., 3306 for MySQL) can be the same across multiple containers, but their host mappings must be unique.

## Troubleshooting Docker Containers

When containers encounter issues, these commands help diagnose problems:

*   **`docker logs <container_id_or_name>`**: Displays the logs generated by a container. This is crucial for understanding what happened during container startup or runtime errors.
*   **`docker exec -it <container_id_or_name> <command>`**: Executes a command inside a running container.
    *   Example: `docker exec -it my_mysql_container bash` opens a bash shell inside the `my_mysql_container`, allowing you to inspect files, environment variables, or run other commands within its environment.
    *   Exiting the `exec` session does not stop the container.

## Docker vs. Virtual Machines

| Feature          | Docker Containers                               | Virtual Machines (VMs)                                    |
| :--------------- | :---------------------------------------------- | :---------------------------------------------------------- |
| **Architecture** | Virtualizes the application layer.              | Virtualizes the entire hardware, including the OS kernel.   |
| **Kernel**       | Shares the host OS kernel.                      | Each VM has its own guest OS kernel.                        |
| **Overhead**     | Very low overhead.                              | High overhead (due to running a full guest OS).             |
| **Size**         | Small (MBs).                                    | Large (GBs).                                                |
| **Speed**        | Fast startup and execution.                     | Slower startup and execution.                               |
| **Isolation**    | Process-level isolation.                        | Hardware-level isolation.                                   |
| **Compatibility**| Initially Linux-centric, now cross-platform via virtualization (e.g., WSL2 on Windows, HyperKit on Mac). | Highly compatible across different host OS, as they virtualize the kernel. |

**Key Takeaway:** Docker is generally preferred for application deployment due to its lightweight nature, speed, and efficient resource utilization, especially for microservices architectures. VMs are better for running multiple different operating systems on a single host or for strong hardware isolation.

## Docker Networking Fundamentals

Docker networking defines how containers communicate with each other, the host machine, and the outside world.

*   **`docker network ls`**: Lists all Docker networks on your system.
*   **`docker network create <network_name>`**: Creates a custom Docker network.
    *   By default, custom networks use the `bridge` driver.

**Practical Example: Node.js App with MongoDB and Mongo Express**

This example demonstrates connecting a Node.js application to a MongoDB database and its UI (Mongo Express), all running in Docker containers.

1.  **Node.js Application:** A sample Node.js app (`server.js`) that connects to MongoDB to store and retrieve user data.
2.  **Required Services:**
    *   **MongoDB:** The database itself (Docker image: `mongo`).
    *   **Mongo Express:** A web-based UI for MongoDB (Docker image: `mongo-express`).
3.  **The Problem:** How do the Node.js app, Mongo container, and Mongo Express container communicate?
    *   Without a shared network, they would need to communicate via host ports, which can be complex.
4.  **The Solution: Custom Docker Network:**
    *   Create a custom network: `docker network create mongo-network`.
    *   Run both `mongo` and `mongo-express` containers within this `mongo-network`.
    *   Containers within the same custom network can communicate with each other directly using their container names as hostnames, without needing port binding between them.

**Running Mongo Container:**

```bash
docker run -d \
  -p 27017:27017 \
  --name mongo \
  --network mongo-network \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=qwerty \
  mongo
```

*   `-d`: Detached mode (run in background).
*   `-p 27017:27017`: Map host port 27017 to container port 27017.
*   `--name mongo`: Assign a custom name to the container.
*   `--network mongo-network`: Connect the container to the `mongo-network`.
*   `-e`: Set environment variables (e.g., root username and password for MongoDB).

**Running Mongo Express Container:**

```bash
docker run -d \
  -p 8081:8081 \
  --name mongo-express \
  --network mongo-network \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty \
  -e ME_CONFIG_MONGODB_SERVER=mongo \
  mongo-express
```

*   `ME_CONFIG_MONGODB_SERVER=mongo`: Mongo Express connects to the `mongo` container using its container name as the hostname, thanks to the shared `mongo-network`.
*   Access Mongo Express UI at `localhost:8081`. Default login: `admin` / `pass`.

**Connecting Node.js App:** The Node.js application can now connect to the `mongo` container using the connection string `mongodb://admin:qwerty@localhost:27017/`.

## Dockerizing an Application with Dockerfile

**Dockerizing an application** is the process of converting your application into a Docker image, which can then be run as a Docker container. This image can be shared and deployed.

A **Dockerfile** is a text file that contains a set of instructions for Docker to build an image.

**Key Dockerfile Instructions:**

*   **`FROM <base_image>[:<tag>]`**: Specifies the base image for your application. This forms the first layer of your image.
    *   Example: `FROM node:16` (uses Node.js version 16 as the base).
*   **`ENV <key>=<value>`**: Sets environment variables within the image.
    *   Example: `ENV MONGO_DB_USERNAME=admin`
*   **`WORKDIR <path>`**: Sets the working directory for any subsequent `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, or `ADD` instructions.
    *   Example: `WORKDIR /app`
*   **`COPY <source_path> <destination_path>`**: Copies files or directories from your host machine (build context) into the Docker image.
    *   Example: `COPY . .` (copies all files from the current directory to the working directory in the image).
*   **`RUN <command>`**: Executes commands during the image build process. These commands create new layers in the image.
    *   Example: `RUN npm install` (installs Node.js dependencies).
*   **`EXPOSE <port>`**: Informs Docker that the container listens on the specified network port at runtime. It's documentation; it doesn't actually publish the port.
    *   Example: `EXPOSE 5050`
*   **`CMD ["executable", "param1", "param2"]`**: Provides default commands for an executing container. There can only be one `CMD` instruction in a Dockerfile. It's the command that runs when the container starts.
    *   Example: `CMD ["node", "server.js"]`

**Example Dockerfile for a Node.js App:**

```dockerfile
# Use an official Node.js runtime as a parent image
FROM node:16

# Set environment variables
ENV MONGO_DB_USERNAME=admin
ENV MONGO_DB_PASSWORD=qwerty

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to install dependencies
COPY package*.json ./

# Install app dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Expose the port the app runs on
EXPOSE 5050

# Command to run the application
CMD ["node", "server.js"]
```

**Building the Docker Image:**

```bash
docker build -t <your_dockerhub_username>/test-app:1.0 .
```

*   `-t`: Tags the image with a name and optional tag (version).
*   `.`: Specifies the build context (current directory, where the Dockerfile is located).

**Running the Application Container from the Image:**

```bash
docker run -p 5050:5050 <your_dockerhub_username>/test-app:1.0
```

## Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications. It allows you to configure all your application's services (containers) in a single YAML file, then manage them with a single command.

**Benefits:**

*   **Simplified Configuration:** Define all container settings (images, ports, networks, environment variables, volumes) in one `docker-compose.yml` file.
*   **Orchestration:** Start, stop, and rebuild all services with a single command.
*   **Reproducibility:** Ensures consistent environments across different machines.

**`docker-compose.yml` Structure:**

```yaml
version: '3.8' # Optional, but common to specify Docker Compose file format version
services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: qwerty
    # volumes: # To be added for persistence
    #   - mongo-data:/data/db
  
  mongo-express:
    image: mongo-express
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: qwerty
      ME_CONFIG_MONGODB_SERVER: mongo # Connects to the 'mongo' service by name
    depends_on:
      - mongo # Ensures mongo starts before mongo-express

  # Example for our Node.js app
  test-app:
    build: . # Build from Dockerfile in current directory
    ports:
      - "5050:5050"
    environment:
      MONGO_DB_USERNAME: admin
      MONGO_DB_PASSWORD: qwerty
      MONGO_DB_URL: mongodb://admin:qwerty@mongo:27017/ # Connects to 'mongo' service
    depends_on:
      - mongo

# volumes: # To be added for persistence
#   mongo-data:
```

**Key Points:**

*   **`services`**: Defines the individual containers that make up your application.
*   **`build: .`**: Tells Docker Compose to build an image from the Dockerfile in the current directory.
*   **`image: <image_name>`**: Specifies an existing image to use.
*   **`depends_on`**: Defines dependencies between services, ensuring they start in the correct order.
*   **Default Network:** Docker Compose automatically creates a default network for all services defined in the `docker-compose.yml` file, allowing them to communicate by their service names.

**Docker Compose Commands:**

*   **`docker compose -f <file_name>.yml up -d`**: Builds (if necessary), creates, and starts all services defined in the YAML file in detached mode.
*   **`docker compose -f <file_name>.yml down`**: Stops and removes all services, networks, and volumes (if not explicitly defined as external) created by `up`.

## Publishing Docker Images to Docker Hub

To share your custom Docker images with others or deploy them, you can push them to Docker Hub.

1.  **Sign Up/Login:** Create an account on `hub.docker.com` and log in.
2.  **Create Repository:** On Docker Hub, create a new repository (e.g., `test-application`). The repository name will typically be `your_dockerhub_username/repository_name`.
3.  **Tag Your Image:** Before pushing, your local image needs to be tagged with the full repository name.
    *   Example: If your local image is `test-app:1.0` and your Docker Hub repo is `devapnacollege/test-application`, you'd tag it:
        `docker tag test-app:1.0 devapnacollege/test-application:1.0`
4.  **Login from Terminal:** Authenticate your Docker CLI with your Docker Hub account.
    *   `docker login` (enter username and password, or use device confirmation).
5.  **Push Image:** Push the tagged image to Docker Hub.
    *   `docker push devapnacollege/test-application:1.0`

Once pushed, others can pull your image using `docker pull devapnacollege/test-application:1.0`.

## Docker Volumes for Data Persistence

By default, data stored inside a container's file system is lost when the container is stopped or deleted. **Docker Volumes** provide a way to persist data generated by Docker containers.

*   **Concept:** Volumes are extra storage spaces on the host system that are managed by Docker (or sometimes directly by the host OS) and mounted into containers. Any data written to the mounted path inside the container is stored in the volume, ensuring persistence.
*   **Benefits:**
    *   **Data Persistence:** Data survives container restarts, deletions, and even if the host machine is rebooted.
    *   **Data Sharing:** Multiple containers can share the same volume.
    *   **Backup/Restore:** Easier to back up and restore data.

**Types of Volume Mounts:**

1.  **Named Volumes (Docker-managed):**
    *   **Creation:** `docker volume create <volume_name>`
    *   **Mounting:** `docker run -v <volume_name>:<container_path> <image_name>`
    *   **Characteristics:** Docker manages the location on the host. Preferred for most use cases, especially in production.
    *   Example: `docker run -v my-data:/app/data ubuntu`
2.  **Anonymous Volumes (Docker-managed):**
    *   **Mounting:** `docker run -v <container_path> <image_name>`
    *   **Characteristics:** Docker creates and manages an unnamed volume. Useful for temporary storage where the specific location on the host doesn't matter.
3.  **Bind Mounts (Host-managed):**
    *   **Mounting:** `docker run -v <host_path>:<container_path> <image_name>`
    *   **Characteristics:** You explicitly specify a path on the host machine to mount into the container. The host OS manages the files. Useful for development (e.g., live code changes) or when you need direct control over the host location.
    *   Example: `docker run -v /Users/youruser/Desktop/data:/test-data ubuntu`

**Integrating Volumes with Docker Compose:**

To use volumes with Docker Compose, define them in the `volumes` section of your `docker-compose.yml` file.

```yaml
version: '3.8'
services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: qwerty
    volumes:
      - mongo-data:/data/db # Mounts the named volume 'mongo-data' to /data/db in the container

  mongo-express:
    image: mongo-express
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: qwerty
      ME_CONFIG_MONGODB_SERVER: mongo
    depends_on:
      - mongo

volumes:
  mongo-data: # Defines a named volume
```

**Volume Management Commands:**

*   **`docker volume ls`**: Lists all Docker volumes.
*   **`docker volume inspect <volume_name>`**: Shows detailed information about a volume.
*   **`docker volume rm <volume_name>`**: Removes a volume.
*   **`docker volume prune`**: Removes all unused local volumes (by default, targets anonymous volumes).

## Advanced Docker Networking

Docker networking governs how containers interact.

*   **`docker network ls`**: Lists all networks. Docker provides three default networks: `bridge`, `host`, and `none`.
*   **Network Drivers:** Define how a network operates.

1.  **Bridge Network (Default):**
    *   **Behavior:** Containers attached to a bridge network can communicate with each other on the same host and with the outside world (via port binding).
    *   **Types:**
        *   **Default Bridge Network:** Automatically created by Docker. Containers need to use IP addresses or linked names to communicate.
        *   **Custom Bridge Networks:** Created explicitly by the user (`docker network create`). Containers within a custom bridge network can communicate directly by their service/container names, which is highly convenient for multi-container applications (as seen with Mongo and Mongo Express).
    *   **Use Case:** Most common for applications running multiple containers on a single host that need to communicate.

2.  **Host Network:**
    *   **Behavior:** A container using the host network shares the host's network stack. It doesn't have its own IP address or isolated network space.
    *   **Use Case:** When a container needs direct access to the host's network interfaces, or for performance-critical applications where network isolation overhead is undesirable.

3.  **None Network:**
    *   **Behavior:** A container using the `none` network is completely isolated from other containers and the host network. It has no network interfaces.
    *   **Use Case:** For containers that don't require network access, or for security-sensitive tasks where complete network isolation is needed.

**Practical Usage:**
*   **Bridge networks** (especially custom ones) are the most frequently used for typical multi-container applications.
*   **Host networks** are used in specific scenarios requiring direct host network access.

---
