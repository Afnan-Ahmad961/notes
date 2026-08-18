# Docker

> Auto-generated summaries from articles and videos. Last updated: 2026-08-18.

## Table of Contents

<!-- INDEX START -->
<!-- INDEX END -->

---


## Docker

*Added: 2026-08-18 17:22*

**Source:** [https://www.youtube.com/watch?v=exmSJpJvIPs](https://www.youtube.com/watch?v=exmSJpJvIPs)

## Contents

- [Docker: Overview](#docker-overview)
- [Docker: Why Docker?](#docker-why-docker)
- [Docker: Docker Containers](#docker-docker-containers)
- [Docker: Docker Images](#docker-docker-images)
- [Docker: Setting Up Docker Desktop](#docker-setting-up-docker-desktop)
- [Docker: Docker Hub](#docker-docker-hub)
- [Docker: Essential Docker Commands](#docker-essential-docker-commands)
  - [Docker: `docker pull`](#docker-docker-pull)
  - [Docker: `docker images`](#docker-docker-images)
  - [Docker: `docker run`](#docker-docker-run)
  - [Docker: `docker ps` and `docker ps -a`](#docker-docker-ps-and-docker-ps--a)
  - [Docker: `docker start` and `docker stop`](#docker-docker-start-and-docker-stop)
  - [Docker: `docker rmi` and `docker rm`](#docker-docker-rmi-and-docker-rm)
- [Docker: Versions and Tags in Docker Images](#docker-versions-and-tags-in-docker-images)
- [Docker: Detached Mode (`-d`) and Custom Names (`--name`)](#docker-detached-mode--d--and-custom-names---name)
- [Docker: Image Layering](#docker-image-layering)
- [Docker: Port Binding (`-p`)](#docker-port-binding--p)
- [Docker: Troubleshooting Containers](#docker-troubleshooting-containers)
  - [Docker: `docker logs`](#docker-docker-logs)
  - [Docker: `docker exec`](#docker-docker-exec)
- [Docker: Docker vs. Virtual Machines](#docker-docker-vs-virtual-machines)
- [Docker: Practical Use Case - Dockerizing a Node.js Application with MongoDB](#docker-practical-use-case---dockerizing-a-nodejs-application-with-mongodb)
- [Docker: Docker Networking](#docker-docker-networking)
  - [Docker: `docker network ls`](#docker-docker-network-ls)
  - [Docker: `docker network create`](#docker-docker-network-create)
- [Docker: Setting up MongoDB and Mongo Express Containers](#docker-setting-up-mongodb-and-mongo-express-containers)
- [Docker: Docker Compose](#docker-docker-compose)
  - [Docker: Docker Compose File (`docker-compose.yaml`)](#docker-docker-compose-file--docker-composeyaml-)
  - [Docker: `docker compose up` and `docker compose down`](#docker-docker-compose-up-and-docker-compose-down)
- [Docker: Dockerizing Your Own Application with a Dockerfile](#docker-dockerizing-your-own-application-with-a-dockerfile)
  - [Docker: Dockerfile Instructions](#docker-dockerfile-instructions)
    - [Docker: `FROM`](#docker-from)
    - [Docker: `ENV`](#docker-env)
    - [Docker: `WORKDIR`](#docker-workdir)
    - [Docker: `COPY`](#docker-copy)
    - [Docker: `RUN`](#docker-run)
    - [Docker: `CMD`](#docker-cmd)
    - [Docker: `EXPOSE`](#docker-expose)
  - [Docker: Building an Image from a Dockerfile (`docker build`)](#docker-building-an-image-from-a-dockerfile--docker-build-)
- [Docker: Publishing Docker Images to Docker Hub](#docker-publishing-docker-images-to-docker-hub)
  - [Docker: `docker login` and `docker push`](#docker-docker-login-and-docker-push)
- [Docker: Docker Volumes](#docker-docker-volumes)
  - [Docker: Data Persistence Issue](#docker-data-persistence-issue)
  - [Docker: What are Volumes?](#docker-what-are-volumes)
  - [Docker: Practical Example of Volume Mounting](#docker-practical-example-of-volume-mounting)
  - [Docker: Volumes with Docker Compose](#docker-volumes-with-docker-compose)
  - [Docker: Managing Volumes](#docker-managing-volumes)
    - [Docker: `docker volume ls`](#docker-docker-volume-ls)
    - [Docker: `docker volume create`](#docker-docker-volume-create)
    - [Docker: Attaching Volumes to Running Containers](#docker-attaching-volumes-to-running-containers)
      - [Docker: Named Volumes](#docker-named-volumes)
      - [Docker: Anonymous Volumes](#docker-anonymous-volumes)
      - [Docker: Bind Mounts](#docker-bind-mounts)
    - [Docker: `docker volume prune`](#docker-docker-volume-prune)
- [Docker: Advanced Docker Networking](#docker-advanced-docker-networking)
  - [Docker: Network Drivers](#docker-network-drivers)
    - [Docker: Bridge Driver](#docker-bridge-driver)
    - [Docker: Host Driver](#docker-host-driver)
    - [Docker: None Driver](#docker-none-driver)

## Docker: Overview

Docker is a platform that helps developers build, ship, and run applications using containers. It addresses common problems in software development, especially in large teams and organizations, by providing a standardized way to package applications and their dependencies. This lecture covers Docker's core concepts, essential commands, practical use cases like containerizing applications, managing multiple containers with Docker Compose, and advanced topics such as volumes and networking.

## Docker: Why Docker?

Traditional software development often faces the "It works on my machine" problem. This occurs because:
- **Manual Dependency Installation:** Manually installing numerous dependencies (e.g., Node.js v16, MongoDB v4.2) on different machines (e.g., a new team member's macOS) is error-prone and time-consuming.
- **Version Mismatches:** Specific parts of an application might depend on exact versions of software. Installing a newer version (e.g., Node.js v20, MongoDB v6) can introduce bugs.
- **Environment Inconsistencies:** Differences in operating systems, CLI commands, or environment variables between development, testing, and production environments can lead to unexpected issues.

Docker solves these problems by providing a consistent and isolated environment for applications.

## Docker: Docker Containers

A Docker container is a single, bundled unit that packages an application along with all its dependencies.
- **Standardized Unit:** It's a self-contained unit that can be shared and deployed across different systems.
- **Replication:** Instead of individually installing dependencies, you share this single unit, ensuring the environment is replicated consistently.
- **Portability:** Containers can run on any machine, regardless of its operating system (Windows, macOS, Linux), as long as Docker is installed.
- **Lightweight:** Containers have minimal overhead, making them quick to create, update, and delete. Multiple containers can run on a single machine efficiently.
- **Isolation:** Containers provide isolated environments. For example, you can run two Node.js applications on the same machine, one using Node.js v16 and another using Node.js v20, by placing them in separate containers. Each container has its own set of dependencies, isolated from the host machine and other containers.

## Docker: Docker Images

A Docker image is an executable file that contains instructions for building a Docker container.
- **Blueprint:** An image is like a blueprint or a class, while a container is a running instance or an object created from that blueprint. A single image can be used to create multiple containers.
- **Sharing:** When sharing an application with teammates, you share the Docker image, not the container itself. Each team member then creates a container from that image on their local system.
- **Resources:** Images are static snapshots and generally small in size. Containers, being running instances, consume system resources (CPU, memory) to provide the operational environment.

## Docker: Setting Up Docker Desktop

Docker Desktop is the official application for running Docker on Windows, macOS, and Linux.
1.  **Download:** Visit `docker.com` and download Docker Desktop for your operating system.
2.  **Installation:** Run the installer. On Windows, it might require administrative permissions and a system restart.
3.  **Verification:** After installation, open your terminal or command prompt and run `docker --version` to check the installed Docker version (e.g., `27.5.1`). You can also run `docker` to see a list of available Docker commands.
4.  **Docker Desktop UI:** The Docker Desktop application provides a graphical interface to manage containers, images, and volumes.

## Docker: Docker Hub

Docker Hub (`hub.docker.com`) is a cloud-based registry service where you can find and share Docker images. It's similar to GitHub but for Docker images. You can access public images (like Ubuntu, MySQL, MongoDB) or push your own images to public or private repositories.

## Docker: Essential Docker Commands

### Docker: `docker pull`

The `docker pull <image_name>` command downloads a Docker image from Docker Hub to your local system.
-   Example: `docker pull hello-world` downloads the `hello-world` image.
-   If no tag (version) is specified, it defaults to the `latest` tag.

### Docker: `docker images`

The `docker images` command lists all Docker images available on your local system.
-   It shows the image name, tag, image ID, creation date, and size.
-   Images are also visible in the Docker Desktop UI under the "Images" tab.

### Docker: `docker run`

The `docker run <image_name>` command creates and starts a new container from a specified image.
-   If the image is not available locally, Docker will first pull it from Docker Hub.
-   Example: `docker run hello-world` creates and runs a container that prints a "Hello from Docker!" message and then stops.
-   **Interactive Mode (`-it`):** `docker run -it <image_name> <command>` runs a container in interactive mode, allowing you to access its terminal (e.g., `docker run -it ubuntu bash`). This lets you input commands and see output within the container's environment.
    -   Inside an interactive container, you can run commands like `ls`, `mkdir`, `pwd`, `env`.
    -   Typing `exit` will stop the container and return you to the host terminal.

### Docker: `docker ps` and `docker ps -a`

-   `docker ps`: Lists all currently *running* containers.
-   `docker ps -a`: Lists *all* containers, including those that have stopped or exited.
    -   This command shows container ID, image, command, creation time, status, ports, and name.
    -   Docker assigns a random name to containers by default if not specified.

### Docker: `docker start` and `docker stop`

-   `docker start <container_id_or_name>`: Starts an *existing* stopped container.
-   `docker stop <container_id_or_name>`: Stops a running container.

### Docker: `docker rmi` and `docker rm`

-   `docker rmi <image_id_or_name>`: Removes a Docker image from your local system.
    -   An image cannot be removed if it's being used by a container (even a stopped one). You must remove the container first.
-   `docker rm <container_id_or_name>`: Removes a Docker container.

## Docker: Versions and Tags in Docker Images

-   **Tags:** Tags are used to specify different versions or variants of a Docker image. For example, `mysql:latest` refers to the latest version of MySQL, while `mysql:8.0` refers to version 8.0.
-   **Pulling Specific Versions:** You can pull a specific version using `docker pull <image_name>:<tag>`, e.g., `docker pull mysql:8.0`.
-   **Layering and Efficiency:** When pulling different versions of the same image, Docker only downloads the layers that are unique to that version. Common layers that already exist locally are reused, making the process faster and more efficient.

## Docker: Detached Mode (`-d`) and Custom Names (`--name`)

-   **Detached Mode (`-d`):** Runs a container in the background, allowing you to continue using your terminal. By default, containers run in attached mode.
    -   Example: `docker run -d mysql`
-   **Custom Names (`--name`):** Assigns a custom, human-readable name to your container instead of a random one.
    -   Example: `docker run -d --name my-mysql-container mysql`
-   **Environment Variables (`-e`):** Sets environment variables inside the container. This is crucial for configuring applications (e.g., database passwords).
    -   Example: `docker run -d -e MYSQL_ROOT_PASSWORD=secret mysql`

## Docker: Image Layering

Docker images are composed of multiple read-only layers.
-   **Base Layer:** The foundational layer, often an operating system (like Debian or Ubuntu) or a runtime environment (like Node.js). These layers are generally immutable.
-   **Application Layers:** Subsequent layers add application code, dependencies, and configurations on top of the base layer.
-   **Container Layer:** When a container is created from an image, a thin, writable layer is added on top of the image layers. All changes made within the running container (e.g., creating files) occur in this layer.
-   **Efficiency:** When multiple images share the same base layers, Docker reuses those layers, saving disk space and speeding up image pulls.

## Docker: Port Binding (`-p`)

By default, containers have their own isolated network ports, separate from the host machine's ports. Port binding maps a port on the host machine to a port inside the container, allowing external access to the containerized application.
-   **Syntax:** `docker run -p <host_port>:<container_port> <image_name>`
-   Example: `docker run -d -p 8080:3306 --name my-mysql mysql` maps the host's port 8080 to the container's port 3306 (the default MySQL port).
-   **Important Note:** Each host port can only be bound to one container at a time. If you try to bind the same host port (e.g., 8080) to another container, it will result in an error ("port is already allocated"). You must use a different host port for each container if they need to be externally accessible.

## Docker: Troubleshooting Containers

### Docker: `docker logs`

The `docker logs <container_id_or_name>` command retrieves the logs from a container. This is essential for debugging and understanding what's happening inside a running container.
-   Example: `docker logs my-mysql-container`

### Docker: `docker exec`

The `docker exec -it <container_id_or_name> <command>` command allows you to run additional commands inside an already running container.
-   **Interactive Access:** Using `-it` with `bash` or `sh` (if `bash` is not available) gives you an interactive shell inside the container.
    -   Example: `docker exec -it my-mysql-container bash`
-   From within the container's shell, you can inspect files, check environment variables, or run other diagnostic commands.
-   Exiting the `exec` session does *not* stop the container.

## Docker: Docker vs. Virtual Machines

| Feature          | Docker                                            | Virtual Machine (VM)                               |
| :--------------- | :------------------------------------------------ | :------------------------------------------------- |
| **Isolation**    | Virtualizes the application layer                 | Virtualizes the entire hardware, including the OS kernel |
| **Kernel**       | Shares the host OS kernel                         | Each VM has its own guest OS kernel                |
| **Overhead**     | Low overhead, lightweight                         | High overhead, heavy                               |
| **Size**         | Typically in MBs                                  | Typically in GBs                                   |
| **Startup Time** | Fast (seconds)                                    | Slower (minutes)                                   |
| **Portability**  | Highly portable across different OS kernels (with some caveats for Windows/macOS) | Highly portable across different hardware          |
| **Compatibility**| Initially built for Linux, uses a VM on Windows/macOS to run Linux containers | Compatible with all underlying operating systems   |

Docker's main advantage is its lightweight nature and efficiency, making it ideal for microservices and rapid deployment. VMs provide stronger isolation and are suitable for running entirely different operating systems.

## Docker: Practical Use Case - Dockerizing a Node.js Application with MongoDB

This section demonstrates how to use Docker to run a Node.js application that connects to a MongoDB database, without installing MongoDB directly on the host machine.
1.  **Node.js Application:** A sample Node.js application (`server.js`) with Express.js and MongoDB client is set up. It has routes for `GET /users` (fetch users) and `POST /addUser` (add a new user). The server runs on port 5050 and connects to a database named `apna_college_db`.
2.  **Docker for Database:** Instead of installing MongoDB locally, we will use Docker containers for the database:
    *   **MongoDB Container:** For the actual MongoDB database.
    *   **Mongo Express Container:** A web-based user interface to manage the MongoDB database.
3.  **Docker Network:** To allow the MongoDB and Mongo Express containers to communicate directly without needing explicit port mappings or `localhost` addresses, a custom Docker network is created.

## Docker: Docker Networking

Docker networks allow containers to communicate with each other and with the outside world.
-   **Isolation:** By default, containers are isolated. To enable communication, they need to be part of the same network.
-   **Custom Networks:** You can create custom networks to group related containers.

### Docker: `docker network ls`

Lists all Docker networks on your system. It shows network ID, name, driver, and scope. Docker provides three default networks: `bridge`, `host`, and `none`.

### Docker: `docker network create`

Creates a new custom Docker network.
-   Example: `docker network create mongo-network` creates a network named `mongo-network`.
-   By default, custom networks use the `bridge` driver.

## Docker: Setting up MongoDB and Mongo Express Containers

1.  **MongoDB Container:**
    *   Image: `mongo`
    *   Options:
        *   `-d`: Detached mode.
        *   `-p 27017:27017`: Port binding (host port 27017 to container port 27017).
        *   `--name mongo`: Custom container name.
        *   `--network mongo-network`: Connects the container to the `mongo-network`.
        *   `-e MONGO_INITDB_ROOT_USERNAME=admin`: Sets the root username.
        *   `-e MONGO_INITDB_ROOT_PASSWORD=qwerty`: Sets the root password.
    *   Command: `docker run -d -p 27017:27017 --name mongo --network mongo-network -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=qwerty mongo`
2.  **Mongo Express Container:**
    *   Image: `mongo-express`
    *   Options:
        *   `-d`: Detached mode.
        *   `-p 8081:8081`: Port binding (host port 8081 to container port 8081).
        *   `--name mongo-express`: Custom container name.
        *   `--network mongo-network`: Connects the container to the `mongo-network`.
        *   `-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin`: Sets the admin username for Mongo Express.
        *   `-e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty`: Sets the admin password for Mongo Express.
        *   `-e ME_CONFIG_MONGODB_SERVER=mongo`: Specifies the MongoDB server hostname (the name of the MongoDB container within the same network).
        *   `-e ME_CONFIG_MONGODB_URL=mongodb://admin:qwerty@mongo:27017/`: The full MongoDB connection URL.
    *   Command: `docker run -d -p 8081:8081 --name mongo-express --network mongo-network -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty -e ME_CONFIG_MONGODB_SERVER=mongo -e ME_CONFIG_MONGODB_URL=mongodb://admin:qwerty@mongo:27017/ mongo-express`
3.  **Verification:**
    *   Access Mongo Express at `localhost:8081`. Use username `admin` and password `pass` (default for Mongo Express UI, not the MongoDB root password).
    *   Create the `apna_college_db` database and a `users` collection. Add a sample document.
    *   Run the Node.js application (`node server.js`).
    *   Send a `GET` request to `localhost:5050/getUsers`. The application should successfully fetch data from the MongoDB container, demonstrating connectivity.
    *   Send a `POST` request to `localhost:5050/addUser` to add new users, which will be visible in Mongo Express.

## Docker: Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications. It allows you to configure all your application's services (containers) in a single YAML file, simplifying their management.
-   **Problem:** Running multiple containers with many options (port bindings, environment variables, networks, names) via `docker run` commands can be complex and error-prone.
-   **Solution:** Docker Compose allows you to define all these configurations in a `docker-compose.yaml` file.
-   **Benefits:**
    *   **Structured Configuration:** All container configurations are in one readable, structured file.
    *   **Easy Management:** Start, stop, and rebuild all services with a single command.
    *   **Portability:** Share the `docker-compose.yaml` file, and anyone can spin up the entire application stack.

### Docker: Docker Compose File (`docker-compose.yaml`)

A `docker-compose.yaml` (or `.yml`) file defines your services (containers), networks, and volumes.
-   **`version`:** (Optional in newer Docker Compose versions) Specifies the Docker Compose file format version.
-   **`services`:** Defines the individual containers that make up your application. Each service has:
    *   `image`: The Docker image to use (e.g., `mongo`, `mongo-express`).
    *   `ports`: Port mappings (e.g., `- "27017:27017"`).
    *   `environment`: Environment variables (e.g., `MONGO_INITDB_ROOT_USERNAME: admin`).
    *   `container_name`: Custom name for the container.
    *   `networks`: Specifies which network the container should join.
-   **Default Network:** If no network is explicitly defined in the `docker-compose.yaml` file, Docker Compose automatically creates a default bridge network for all services defined in that file, allowing them to communicate by service name.

Example `docker-compose.yaml` for MongoDB and Mongo Express:

```yaml
version: '3.8'
services:
  mongo:
    image: mongo
    container_name: mongo
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: qwerty
  mongo-express:
    image: mongo-express
    container_name: mongo-express
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: qwerty
      ME_CONFIG_MONGODB_SERVER: mongo
      ME_CONFIG_MONGODB_URL: mongodb://admin:qwerty@mongo:27017/
```

### Docker: `docker compose up` and `docker compose down`

-   `docker compose -f <file_name>.yml up -d`: Builds, creates, and starts all services defined in the specified YAML file in detached mode.
    -   Example: `docker compose -f mongodb.yml up -d`
    -   If images are not local, Docker Compose will pull them.
    -   It creates a default network (e.g., `testapp_default`) for the services.
-   `docker compose -f <file_name>.yml down`: Stops and removes all containers, networks, and volumes defined in the YAML file.
    -   Example: `docker compose -f mongodb.yml down`

## Docker: Dockerizing Your Own Application with a Dockerfile

Dockerizing an application means converting your application code into a Docker image, from which Docker containers can be created. This image can then be shared and deployed.
-   **Dockerfile:** A `Dockerfile` is a text file that contains a set of instructions for Docker to build an image. It acts as a blueprint for your application's Docker image and containers.
-   **CI/CD:** In a real-world CI/CD pipeline (e.g., using Jenkins), the Dockerfile is used to automatically build images, which are then pushed to a public or private registry.

### Docker: Dockerfile Instructions

Common instructions used in a Dockerfile:

#### Docker: `FROM`

Specifies the base image for your Docker image. Every Docker image is built on top of a base image.
-   Example: `FROM node:16` uses Node.js version 16 as the base image. This ensures the necessary runtime environment is available in the container.
-   This demonstrates image layering: your application's image is built on top of the Node.js image, which itself might be built on top of a Linux distribution like Debian.

#### Docker: `ENV`

Sets environment variables inside the image.
-   Example: `ENV MONGO_DB_USERNAME=admin`

#### Docker: `WORKDIR`

Sets the working directory for any subsequent `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, or `ADD` instructions.
-   Example: `WORKDIR /app` means all subsequent commands will be executed relative to `/app` inside the container.

#### Docker: `COPY`

Copies files or directories from the host machine to the Docker image.
-   Syntax: `COPY <source_path_on_host> <destination_path_in_image>`
-   Example: `COPY . /app` copies all files from the current directory (where the Dockerfile is located) on the host to the `/app` directory in the image.

#### Docker: `RUN`

Executes commands during the image build process. These commands create new layers in the image.
-   Example: `RUN npm install` installs Node.js dependencies. This command would be necessary if `node_modules` is not copied directly into the image.

#### Docker: `CMD`

Specifies the default command to execute when a container is started from the image. There can only be one `CMD` instruction in a Dockerfile.
-   Syntax: `CMD ["executable", "param1", "param2"]` (exec form, preferred) or `CMD command param1 param2` (shell form).
-   Example: `CMD ["node", "server.js"]` runs the Node.js server when the container starts.

#### Docker: `EXPOSE`

Informs Docker that the container listens on the specified network ports at runtime. It's documentation, not a functional port mapping.
-   Example: `EXPOSE 5050`

### Docker: Building an Image from a Dockerfile (`docker build`)

To build a Docker image from a Dockerfile:
-   `docker build -t <image_name>:<tag> .`
    -   `-t`: Tags the image with a name and optional tag (version).
    -   `.`: Specifies the build context (the directory containing the Dockerfile and application code).
-   Example: `docker build -t test-app:1.0 .`
-   After building, the image will be listed by `docker images` and visible in Docker Desktop.
-   You can then run a container from your custom image: `docker run -p 5050:5050 test-app:1.0`

## Docker: Publishing Docker Images to Docker Hub

You can share your custom Docker images by pushing them to Docker Hub.
1.  **Sign Up/Log In:** Create an account or log in to Docker Hub (`hub.docker.com`).
2.  **Create Repository:** Go to "Repositories" and create a new repository (e.g., `your_username/test-application`).
3.  **Tag Image:** Ensure your local image is tagged with the correct repository name. If you built it as `test-app:1.0`, you might need to re-tag it: `docker tag test-app:1.0 your_username/test-application:1.0`. Or, build it directly with the full name: `docker build -t your_username/test-application:1.0 .`
4.  **Log In to Docker CLI:** Authenticate your Docker CLI with your Docker Hub account.

### Docker: `docker login` and `docker push`

-   `docker login`: Prompts for your Docker Hub username and password.
    -   Alternatively, Docker Desktop provides a browser-based login flow.
-   `docker push <image_name>:<tag>`: Pushes the local image to the specified Docker Hub repository.
    -   Example: `docker push your_username/test-application:1.0`
-   Once pushed, the image will be publicly (or privately, if configured) available on Docker Hub, and others can pull it using `docker pull your_username/test-application:1.0`.

## Docker: Docker Volumes

### Docker: Data Persistence Issue

By default, data stored inside a container's virtual file system is ephemeral. If a container is stopped, restarted, or deleted, all changes and data within it are lost. This is problematic for applications that need to store persistent data, like databases.

### Docker: What are Volumes?

Docker volumes are the preferred mechanism for persisting data generated by and used by Docker containers.
-   **Persistent Storage:** Volumes allow you to store data on the host machine, outside the container's writable layer.
-   **Managed by Docker:** In most cases, Docker manages the creation and lifecycle of volumes.
-   **Mounting:** A volume is "mounted" to a specific directory inside the container. Any data written to that directory in the container is actually stored in the volume on the host.
-   **Benefits:**
    *   **Data Persistence:** Data remains even if the container is stopped, restarted, or deleted.
    *   **Data Sharing:** Multiple containers can mount and share the same volume, allowing them to access the same data.
    *   **Backup/Restore:** Volumes can be backed up and restored easily.

### Docker: Practical Example of Volume Mounting

1.  **Create Host Directory:** Create a directory on your host machine (e.g., `~/Desktop/data`).
2.  **Run Container with Volume:**
    *   `docker run -it -v <host_absolute_path>:<container_path> ubuntu bash`
    *   Example: `docker run -it -v /Users/your_username/Desktop/data:/test-data ubuntu bash`
    *   This command mounts the host's `~/Desktop/data` directory to `/test-data` inside the Ubuntu container.
3.  **Verify Persistence:**
    *   Inside the container, navigate to `/test-data` and create some files (e.g., `index.html`, `server.js`).
    *   Check the host's `~/Desktop/data` directory; the files will appear there.
    *   Exit the container, stop it, and even delete it. The files on the host's `~/Desktop/data` directory will still persist.
    *   Start a new container with the same volume mount; the files will be accessible inside the new container.

### Docker: Volumes with Docker Compose

To use volumes with Docker Compose, you define them in your `docker-compose.yaml` file.
-   Add a `volumes` section under the service definition:
    ```yaml
    services:
      mongo:
        image: mongo
        # ... other configurations
        volumes:
          - /Users/your_username/Desktop/data:/data/db
    ```
    -   Here, `/data/db` is the default directory where MongoDB stores its data inside the container.
-   When you run `docker compose up`, Docker Compose will create and mount the specified volume, ensuring data persistence for your MongoDB container.

### Docker: Managing Volumes

#### Docker: `docker volume ls`

Lists all Docker volumes on your system. It shows the volume name and driver. Also visible in Docker Desktop under the "Volumes" tab.

#### Docker: `docker volume create`

Creates a new named volume.
-   Example: `docker volume create my-volume`
-   Named volumes are explicitly managed by Docker and are the preferred way for data persistence in production.
-   **Location:** On Linux, volumes are typically created in `/var/lib/docker/volumes/`. On Windows/macOS (which run Docker in a VM), they are created within the Docker Desktop VM.

#### Docker: Attaching Volumes to Running Containers

There are three primary ways to attach volumes to containers using the `-v` (volume) or `--mount` flags:

##### Docker: Named Volumes

-   **Syntax:** `docker run -v <volume_name>:<container_path> <image_name>`
-   Example: `docker run -v my-volume:/app/data my-app`
-   Docker manages the volume's storage location on the host. If `my-volume` doesn't exist, Docker creates it.
-   **Preferred Method:** Most common and recommended for production environments due to Docker's management and ease of backup/migration.

##### Docker: Anonymous Volumes

-   **Syntax:** `docker run -v <container_path> <image_name>`
-   Example: `docker run -v /app/temp my-app`
-   Docker creates an unnamed volume (a random ID) and mounts it. Useful for temporary data that needs to persist only as long as the container exists or for data that doesn't need explicit naming.

##### Docker: Bind Mounts

-   **Syntax:** `docker run -v <host_absolute_path>:<container_path> <image_name>`
-   Example: `docker run -v /home/user/app/src:/app/src my-app`
-   You explicitly specify a path on the host machine to mount into the container.
-   **Use Case:** Often used during development to mount source code into a container, allowing live code changes to be reflected without rebuilding the image.
-   **Management:** The host operating system manages the host directory, not Docker.

#### Docker: `docker volume prune`

Removes all unused local volumes (volumes not referenced by any container).
-   By default, it primarily targets anonymous volumes.
-   Example: `docker volume prune`
-   Useful for cleaning up disk space.

## Docker: Advanced Docker Networking

Docker networking defines how containers communicate with each other, the host machine, and external networks.

### Docker: Network Drivers

Docker provides several network drivers, each suited for different use cases. The three most common are:

#### Docker: Bridge Driver

-   **Default:** The default network driver if none is specified.
-   **Functionality:** Creates a virtual bridge on the host machine. Containers connected to this bridge can communicate with each other and with the host machine (via port binding) and the outside world.
-   **Types:**
    *   **Default Bridge Network:** Created automatically by Docker. Containers on this network can communicate by IP address.
    *   **Custom Bridge Networks:** Created explicitly by the user (e.g., `docker network create my-network`).
        *   **Key Advantage:** Containers on a custom bridge network can communicate with each other using their container names as hostnames, simplifying service discovery (e.g., `mongo-express` can connect to `mongo` using the hostname `mongo`). This is why we used it for the Node.js/MongoDB example.
-   **Use Case:** Most common for applications running in containers that need to communicate with other containers on the same host.

#### Docker: Host Driver

-   **Functionality:** Removes network isolation between the container and the Docker host. The container uses the host's network stack directly.
-   **No IP Address:** The container does not have its own IP address; it shares the host's IP address.
-   **Port Conflicts:** If the container tries to bind to a port that is already in use on the host, it will fail.
-   **Use Case:** When the container needs direct access to the host's network, or for performance-critical applications where the overhead of network virtualization is undesirable.

#### Docker: None Driver

-   **Functionality:** Completely isolates the container from all networks. The container has no network interfaces (only a loopback device).
-   **No Connectivity:** Cannot communicate with other containers, the host, or the outside world.
-   **Use Case:** For containers that perform batch jobs or require extreme security isolation and do not need any network connectivity.

---
