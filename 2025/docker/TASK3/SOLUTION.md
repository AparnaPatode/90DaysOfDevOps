
## Explore Docker Terminologies and Components 

## Document Key Terminologies


### 1. Docker Engine
The core part of Docker, responsible for running and managing containers. It consists of:

- Docker Daemon (dockerd) – Runs in the background, managing images, containers, and networks.
- Docker CLI – Command-line tool (docker) to interact with the Docker Daemon.
- REST API – Allows other applications to communicate with Docker.

### 2. Docker Image
A blueprint/template for creating containers. It contains:

- Application code
- Dependencies
- System libraries
- Environment configurations

The Following is the command which is used to build the docker image.

    docker build -t your_image_name -f path/to/Dockerfile .

- ***Docker build***: Initiates the build process.
- ***-t your_image_name***  Gives the image you’re creating a name and, if desired, a tag.
- ***path/to/Dockerfile*** : Gives the location of the Dockerfile. Give the right path if it’s not in the current directory. 
- ***(.) DOT*** represents the current workdir.

Verify the Docker Image 

    docker images

Remove an Image from Docker

    docker rmi <image-id>

Searching for a specific image on Docker Hub

    docker search ubuntu

### 3. Docker Container
A lightweight, executable instance of a Docker image. Containers are isolated but share the host OS kernel.

    docker run -d -p 8080:80 nginx

Stop all running containers

    docker stop $(docker ps -aq)

Delete the container

    docker rm  <containerId/Name>


### 4. Dockerfile
A text file containing instructions to build a Docker image.

    # Use base image
    FROM node:18

    # Set working directory
    WORKDIR /app

    # Copy files to container
    COPY . .

    # Install dependencies
    RUN npm install

    # Start the app
    CMD ["node", "server.js"]
    


Build an image from the Dockerfile:

    
    docker build -t my-app .
    

### 5. Docker Hub

It is a cloud based repository that is used for finding and sharing the container images.

    
    docker image tag <old-img-name> <new-img-name>
    docker push my-app  # Push image to Docker Hub
    docker pull mysql  # Download an image from Docker Hub
    

### 6. Docker Compose

Docker Compose will run a multi-container application written in YAML format. The YAML file contains all of the configurations required to deploy containers using Docker Compose, which is integrated with Docker Swarm, and gives instructions for constructing and deploying containers. Docker Compose creates containers that run on a single host.


Run it with:

    
    docker-compose up -d
    

### 7. Docker Volumes
Used for persistent storage in Docker containers.

    
    docker volume create my_data
    docker run -v my_data:/app/data my-app
    
To remove a volume, we can use the following command -

    
    docker volume prune
    


### 8. Docker Network

The Docker network is a virtual network created by Docker to enable communication between Docker containers. If two containers are running on the same host they can communicate with each other without the need for ports to be exposed to the host machine.

#### Types:

- Bridge (default): Containers can communicate on the same host.
- Host: Shares host networking.
- Overlay: Used in multi-host Docker setups.
- none: IP addresses won’t be assigned to containers. These containments are not accessible to us from the outside or from any other        container.


To list all the Docker Networks, you can use the list command.

    
    docker network ls
    

Using Docker ***Network Create command***

    
    docker network create my_network
    docker run --network=my_network nginx


Using the ***Network Inspect command***, you can find out the details of a Docker Network.

    
    sudo docker network inspect <network-name>
    

Using the ***Connect*** command, you can connect a running Docker Container to an existing Network.

    
    sudo docker network connect <network-name> <container-name or id>
    


The ***disconnect*** command can be used to remove a Container from the Network.

    
    sudo docker network disconnect <network-name> <container-name>
    

You can remove a Docker Network using the ***rm*** command.

    
    sudo docker network rm <network-name>
    

To remove all the unused Docker Networks, you can use the ***prune*** command. 

    
    sudo docker network prune
    

### 9. Docker Orchestration (Kubernetes, Swarm)
For managing multiple containers across multiple hosts.

- Docker Swarm – Native clustering tool.
- Kubernetes – Industry-standard orchestration platform.


### 10. Docker Registry
A private or public storage for Docker images.

- Docker Hub (public)
- AWS ECR, Google Container Registry, Azure Container Registry (private)








