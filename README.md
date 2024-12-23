# Docker

<details>
  <summary>Introduction to Docker</summary>

- Why do we need Docker - when application developer develop an application they need to look for Hardware compatibility, OS compatibility,            Libraries Dependencies before to deploy application on server & they need to do same for each installation which is kind of tricky job.
- What can docker Do - Run an application on an Isolated container, that has its own network interfaces, libraries, and processors like virtual        machines except for all containers that share the same OS kernels.(Hardware > OS > Docker > Container).
- Virtual machine VS Docker  - Bootup time, Storage, memory Utilization.
- Docker Edition - Community Edition(free) , Enterprise Edition (paid).
- Docker installation - Windows(docker desktop) , Linux ( docker).
- #systemctl status docker    -command to check docker daemon service running status.
- #systemctl start docker - command to manual start docker daemon.
- #systemctl stop docker - command to stop docker daemon.
- #systemctl enable docker  -command to ensure docker daemon service starts after restart of base machine Hardware.
- #docker version - check the version of the docker.
- #sudo journalctl -u docker   - this command is useful in case when docker service fails to start & need to investigate docker logs.
- A disadvantage of docker - data inside a docker container is not persistent on restart or stop has been made on the container level.
- Docker image: A Docker image is a file that contains all necessary dependencies & Configuration which is used to run an application.
- Docker Container: is a running instance of an image.
- #docker pull <imagename>  - command to pull image.
- #docker image ls  -command to list images.
- #docker run < imagename >   - command to launch an container.
- #docker ps  - list all active container list or ps means process status.
- #docker ps -a - list all active & exit container list.
- #docker stop < containerid.name >   - stop a container before to remove a container.
- #docker rm < container id/name >  - remove an container.
- #docker rmi < image name >    - delete an image from disk.
- #docker logs < container ID >   - check log of container.
- #docker run --name < containername >  < imagename >    - command to run container with specific name.
- #docker inspect < container ID/Name >    -   get details about active container.
- #docker run -d < imagename >    - run container in detached mode. 
- Expose in docker: informs Docker that the container will listen on the specified port,Works only for communication between containers in the same    network.
- Publish in docker: This means container port is accessible through the docker host, when we  publish it.
- #docker run --expose < port > nginx
- #docker run -d -p 8080:80 < imagename >   - run an container with publish port.
- #docker exec -it < container name > bash    - exec into container.
- #docker run -d < imagename > command - command to override the container default command.
- Restart Policies in Docker - no, unless-stopped( it is similar to the always flag the only difference is once the container is stopped manually it   will not restart automatically even after restarting the docker daemon, until we start the container manually again.), on-failure(Restart the        container if it exits due to an non-zero exit code. Exit code :- 0 means no error), always (Always restart the container if it stops. If 
  it's manually stopped, it's restarted only when the Docker daemon restarts or the container itself is manually restarted).
- #docker run --restart unless-stopped <imagename> .
- #docker system df    - command is used to display information about disk usage by Docker. It provides details on the size of images, containers,      local volumes, and the overall disk space being used
- #docker system df -v   - command provides a detailed breakdown of Docker's disk usage, including information about individual images,                containers, volumes, and build cache.
- #docker run -d  --rm < imagename >  -  command to delete an container after exit.
- #docker rename < oldcontainername >  < newcontainername >        - command to rename container.
- #docker top < container ID > — command gives details of Process id belonging to the container.
- #docker ps --size  – command gives information on the current size of the docker container writable layer + size of the image.
- #apt install  net-tools ------ to install ifconfig package.
- #apt install  procps  ----- to install the TOP command package.
  
</details>

<details>
  <summary>Image Creation, Management & Registry</summary>
  
- A docker file is built to form an image that holds a set of instructions to run an application. #vim Dockerfile
- #docker build.  - build an image
- docker instructions - FROM, CMD[“executable”, “parameter1”, “parameter2”], RUN, COPY, ADD , EXPOSE, HEALTHCHECK(HEALTHCHECK --interval=5s CMD ping -c 1 172.17.0.2), WORKDIR, ENTRYPOINT(ENTRYPOINT [‘’/bin/ping ’’]), ENV , LABEL , ARG ,      USER.
- Reason why every docker image start with FROM : FROM instruction initializes a new build stage and sets the Base Image for subsequent instructions. As such, a valid Dockerfile MUST start with a FROM instruction
- COPY & ADD are both docker instructions that server similar purpose, they let you copy files or directory a specific location into Docker Image,ADD Allow you to extract a component of tar file from Source directly to destination docker location.
- #docker build . -t < nameofimage >:tag     - adding an tag to image while creating image
- #docker image tag < imageid > < nameofimage >:tag      - addition of tag to existing image
- #docker image tag < oldimagename >:< oldtag >  <newimagename>:<newtag>  - rename an tag to existing image
- #docker commit < containerid >  < newimagename >        - create an new image with updated container information.
- #docker image history < nameofimage >   - list layers of image
- #docker image inspect < nameofimage >  - details about docker image
- #docker image prune   - delete only dangling images + unreferenced by any container
- #docker image prune -a    - delete all images not referenced by any container
- Docker Registry - The concept of Registry comes in place as it's not recommended to have custom images on a local repository on a local disk it is   available on the registry that is easily accessible to anyone  - Docker Hub, AWS ECR, Docker Registry, Docker Trusted registry
- Docker Hub repo push & pull
       : Create an Docker Hub account
       : #docker login
       : #docker image tag < currentimagename >:tag  puneet2022/learning:< imagenamewhichneedtobereflectinHUBrepo >
       : #docker push puneet2022/learning:< imagenamewhichneedtoberzeflectinHUBrepo >
       : #docker pull puneet2022/learning:< imagenamewhichneedtobereflectinHUBrepo >
- #docker search < nameofimage >   - Command to search a specific image
- #docker save < imagename >    >   < imagename >.tar    - command to save the image into tar to export to a different system
- #docker load -i < imagename.tar   - command to extract an image from tar
- Concept of docker layer: Each instruction belongs to a single layer, the container top layer is a writable layer, and every container uses the same image until the write has been made on the container.
- #docker run -dt -P < nameofimage >  -  this command publish port on random publish ports
- how to reduce docker image size: Make use of lower size of base image: alpine image, minimum number of layers in image, use COPY instructions after RUN instructions. https://devopscube.com/reduce-docker-image-size/, Multi stage Built
- Distroless images are a type of Docker image that includes only the essential components needed to run an application, without including an entire operating system or package manager. They are smaller, more secure, and faster to start up     compared to traditional images like debian, ubuntu, or alpine.  Advantage : Small Size, Without Shell its harder for attackers to exploit vulnerabilities, Faster startup time
- Example of Distroless images : gcr.io/distroless/python3 , gcr.io/distroless/java , gcr.io/distroless/java , gcr.io/distroless/base
</details>



 

**Networking**

- [x] Docker network Drivers  – Community Edition (Bridge, None, Host)  Enterprises Edition (Bridge, Host, none, Macvlan, overlay)
- [x] Bridge network   - it creates software bridge container tag with bridge network can communicate with each other and provide isolation to another container
- [x] Host network - it removes the network isolation between the docker host & docker container, i.e container created with publish port 80 can be reached with port 80 outside the world as well
- [x] None network -  networking is completely disabled in this drive type
- [x] Note : If you need to be have Specific IP range for Container then you either need to modify Bridge network configuration or modify under global /etc/docker/daemon.json file 
- [x] #docker network ls    - list all available network drivers
- [x] #docker inspect network < drivername >    - details of network driver
- [x] #docker run -dt --network < nameofdriver >   < imagename >   - run container on specific container.
- [x] User-defined bridge network - You can create a user-defined bridge network that is superior to the default bridge network (DNS-based name resolution, the container can be attached and detached on the fly)
- [x] #docker network create --driver bridge < nameofcustombridgenetwork >    - create a user define bridge network
- [x] #docker network create --driver bridge < nameofcustombridgenetwork >  --gateway 172.0.0.1 --subnet 172.0.0.0/24  - create an user define bridge network with custom subnet IP range

**Storage & Logging Driver**
- #docker info   - check active storage Driver & logging Driver
- Storage Drive: Works on the principle of copy on write, which means a single copy has been used until the write has been made. (cd /var/lib/docker/overlay2), almost all linux based system are using overlay2 drivers
- logging Driver - json-file(the default driver, storing logs in JSON files.) ,awslogs ,syslog( Forwards logs to a syslog server) ,splunk , local, none
- #docker info -  to check login driver
- #docker container inspect < containerID >  -  to check the logging driver in running container
- #docker run -dt --log-driver < specifytypeofloggingdriveryouwant >  < imagename >   -  way to change logging driver to container
- update logging driver for all containers following changes in the daemon.json file located under /etc/docker

**Volumes**
- Docker Volume - data inside a container is not persisted so the way to make data available even after termination of the container is via the local mount point and bind Mount ( When you use bind mount, a file or 
  directory on the host machine is mounted into a container ) Volume can be mounted on multiple containers simultaneously & volume cant be deleted automatically.
- #docker volume ls    - list active volume.
- #docker volume create < nameofvol >   - manual create an persistence volume from local docker host volume
- #docker volume inspect  < name of volume >
- #docker run -dt -v < nameofvolume >:< mountingdirofcontainer >   <imagename>    - command to manual mount an local volume host machine to container directory
- #docker run -dt -v  < pathoflocalfile >:< pathofcontainerwhereoutputfile > < imagename >   - command to manual mount file on container i.e bind mount
- #docker run -dt -v /home/docker/index.html:/usr/share/nginx/html/index.html  nginx    - command to manual mount file on container i.e bind mount
- #docker run -dt -v /home/docker/:/var/log   nginx sleep 500000    - command to manual mount directroy on container i.e bind mount

**Docker Orchestration**
- Orchestration: managing the life cycle of a container, especially in a large dynamic environment, Provisioning, upscaling, Downscaling, health check of Container -Docker swarm, Kubernetes, Mesos, AWS EKS.
- #docker swarm init --advertise-addr <Ipaddressofeth0>  - command to run on master Node to be swarm master.
- #docker swarm join --token manager/worker - Command to add docker swarm manager or swarm worker to manager node, it will give an instruction on how to Join.
- #docker swarm leave --force  – command to leave from swarm cluster for a specific node
- #docker node ls    - list the available worker node in the cluster
- #docker service create --replica=1 <nameofimage>            -  to create an container/service in swarm cluster
- #docker service ls   -command to list services
- #docker service rm < nameofservice >    - command to remove services
- #docker service scale < nameofservice >=3     -upscale replicas
- #docker service update --replica=3 < nameofservice >
- #docker service ps    - command to list service running on which node
- #docker service create --dt --name < nameofservice > --mode global  <nameofimage>    -command to deploy an service globally on all Node
- #docker service create --name < nameofservice> --replica=3 --publish 8080:80 < nameofimage > – command to launch service with exposed port
- #docker node update --availability drain <nameofswarm Node>
- #docker node update --availability active <nameofnode>
- #docker inspect node < nameofnode >   – command to check node details
- #docker node update --label-add region=mumbai <nodename>
- #docker service create --name myconstraint --constraint node.labels.region==mumbai --replicas 3 nginx
- Locking swarm Cluster - swarm cluster consists of a lot of sensitive data - TLS key used to encrypt communication b/w swarm nodes, RAft logs  , /var/lib/docker/swarm/certificates
- #docker swarm update --autolock=true   - to lock an swarm cluster
- #docker swarm unlock    - to unlock a swarm cluster
- Docker Compose - Docker Compose is a tool for defining and running multi-container applications. 
- #docker-compose config   -   in standalone cluster 
- #docker-compose up  -  in standalone cluster 
- #docker-compose down  - in standalone cluster
- #docker stack deploy --compose-file <naemoffile> <nameofservice>
- #docker stack ls  - in swarm cluster 
- #docker stack ps  - in swarm cluster 
- #docker stack service - in swarm cluster  
- #docker stack rm <nameofservice>  -  in swarm cluster 
- Overlay network driver : The overlay network driver creates a distributed network among multiple Docker daemon hosts. This network sits on top of (overlays) the host-specific networks, allowing containers connected to 
  it to communicate securely when encryption is enabled.
- #docker network create --driver overlay --name <nameofnetwork>  - this command will create an overlay network driver
- #docker network create --opt encrypted --driver overlay --name <nameofnetwork> -  this command will create an overlay network driver with ensure communication is secure
- Split Brain Problem:
- Quorum : recommandation to have odd number of manager node to maintained HA , An N manager cluster tolerates the loss of at most (N-1)/2 managers.
- Universal control plan  -  its enterprise on-prem application solution to manage, deploy, and monitor docker containers in a single place.

**Docker Secret**
- a secret is a blob of data, such as a password, SSH private key, SSL certificate, or another piece of data that should not be transmitted over a network or stored unencrypted in a Dockerfile or in your application's source code,You can use Docker secrets to centrally manage this data and securely transmit it to only those containers that need access to it. Secrets are encrypted during transit and at rest in a Docker swarm. A given secret is only accessible to those services which have been granted explicit access to it, and only while those service tasks are running.
- #docker secret create my_secret -   — When creating a secret, the command accepts input from the command line
- #docker secret create my_secret /path/to/secret/file    - manually typing input with a keyboard is both error-prone and not practical when combined with automated flows. Therefore, we can also use the contents of a  file to create a secret
- #docker secret ls    ---- to list all secret
- #docker secret inspect   <nameofsecret>
- #docker service create --name <nameofservice> --secret <nameofsecret>   <imagename> 
- #docker container exec -it <container ID> ls -l /run/secrets  




<details>
  <summary>Getting Hand-Dirty with Practices</summary>
===================Installation of Docker========================


```bash
#!/bin/bash 
echo "##############################Run the following command to uninstall all conflicting packages#######################################"
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
echo "#########################################Set up Docker's apt repository##############################################################"
sudo apt-get update -y
sudo apt-get install ca-certificates curl -y
sudo install -m 0755 -d /etc/apt/keyrings -y
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update -y
echo "###############################################################installation of Docker#####################################################################"
sudo apt-get install docker-ce -y
echo "####################Starting Docker############################################"
systemctl enable docker
systemctl start docker
systemctl status docker

===============================================


========Basic DockerFile=============
FROM ubuntu
RUN apt-get update -y
RUN apt-get install -y nginx
WORKDIR /var/www/html
COPY index.nginx-debian.html  .
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"] 

0R

FROM ubuntu
RUN apt-get update -y
RUN apt-get install -y nginx
WORKDIR /usr/share/nginx/html
COPY index.html  /usr/share/nginx/html
EXPOSE 80
ADD compressedfile.tar.gz /usr/share/nginx/html
CMD ["nginx", "-g", "daemon off;"] 

====================================================

==============Heathcheck Docker file example========
#HEALTHCHECK :
    #  HEALTHCHECK –interval=5s CMD ping -c 1 172.17.0.2
    #   --interval=DURATION(default:30s)
    #   --timeout=DURATION(default:30s)
    #   --start-period=Duration(default=0s)
    #   --retires=N(default:3)

From busybox
HEALTHCHECK --interval=3s CMD ping -c 1 172.17.0.5

#####
#docker build .
#docker run -dt <imageid> sleep 5000

=================================================================

=======EntryPoint DockerFile example =======
FROM busybox
ENTRYPOINT [‘’/bin/ping ’’]


#docker build .
#docker run -dt <imageid> -c 1 8.8.8.8
==============================================

=======Example of Building an Python APP==========
# Use an official Python runtime as a parent image
FROM python:3.8-slim
# Set the working directory in the container
WORKDIR /app
# Copy the current directory contents into the container at /app
COPY . /app
# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
# Run script.py when the container launches
CMD ["python", "script.py"]

OR

FROM ubuntu
WORKDIR /app
RUN apt-get update && \
    apt-get install -y python3 python3-pip && \
    pip install -r requirements.txt && \
    cd devops
COPY requirements.txt /app
ENV DEBAIN_FRONTEND=noninteractive
COPY devops /app
ENTRYPOINT ["python3"]
CMD ["manage.py", "runserver", "0.0.0.0:8000"]

====================================================

===============Multistage Python App=================

# Stage 1: Build stage
FROM python:3.8-slim AS build-stage
# Set the working directory in the build stage
WORKDIR /app
# Copy only the requirements file to the build stage
COPY requirements.txt .
# Install dependencies in the build stage
RUN pip install --no-cache-dir -r requirements.txt

# Stage 2: Runtime stage with Distroless image
FROM gcr.io/distroless/python3:debug AS runtime-stage
# Set the working directory in the runtime stage
WORKDIR /app
# Copy only the necessary files from the build stage
COPY --from=build-stage /usr/local/lib/python3.8/site-packages /usr/local/lib/python3.8/site-packages
COPY --from=build-stage /app .
# Specify the entry point for the Distroless image
ENTRYPOINT ["python", "script.py"]

OR

$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
Reference GithUb https://github.com/LondheShubham153/python-multistage-docker


# ------------------- Stage 1: Build Stage ------------------------------
FROM python:3.9 AS backend-builder

# Set the working directory to /app
WORKDIR /app

# Copy the contents of the backend directory into the container at /app
COPY backend/ .

# Install dependencies specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# ------------------- Stage 2: Final Stage ------------------------------

# Use a slim Python 3.9 image as the final base image
FROM python:3.9-slim

# Set the working directory to /app
WORKDIR /app

# Copy the built dependencies from the backend-builder stage
COPY --from=backend-builder /usr/local/lib/python3.9/site-packages/ /usr/local/lib/python3.9/site-packages/

# Copy the application code from the backend-builder stage
COPY --from=backend-builder /app /app

# Expose port 5000 for the Flask application
EXPOSE 5000

# Define the default command to run the application
CMD ["python", "app.py"]

=========================================================================

==============Multistage Docker example =================
# Build Stage 
FROM node:22-alphine3.20 as build
WORKDIR /app
COPY package*.json ./
RUN npm install 
RUN npm install --production 
RUN npm install -g serve
COPY . .
RUN npm run build

#Stage 2
FROM node:22-alphine3.20
WOKDIR /app
COPY --from=build /app/build /app/build
EXPOSE 3000
CMD ["serve", "-s","build"]

====================================================

==================Java based application ===============
# Stage 1: Build an application 
FROM maven:3.8.4-openjdk-11 AS builder 
WORKDIR /app
COPY pom.xml  .
COPY src  ./src
RUN mvn package 


# Stage 2: Create the final image 
From openjdk:11-jre-slim
WORKDIR /app
COPY --from=builder /app/target/*.jar ./app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]

=========================================================

=================Golang based App ========================
# Stage1 
FROM ubuntu AS build
RUN apt-get update && apt-get install -y golang-go
ENV GO111MODULE=off
COPY . .
RUN CGO_ENABLED=0 go build -o /app .

############################################
# HERE STARTS THE MAGIC OF MULTI STAGE BUILD
############################################

FROM scratch
# Copy the compiled binary from the build stage
COPY --from=build /app /app
# Set the entrypoint for the container to run the binary
ENTRYPOINT ["/app"]


$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
From golang:1.22 AS build
WORKDIR /src
COPY go.mod go.mod
#download dependencies
RUN go mod download

#copy source file into image
COPY . .

#build the final executable
RUN CGO_ENABLED=0 go build -o /app  .


FROM alpine:latest
COPY --from=build /app /app

#run the application
EXPOSE 8000
CMD ["/app"]




$$$$$$$$$$$$$$$$$$$$$$$$$$$$
From golang:1.22 AS build
WORKDIR /src
COPY go.mod go.mod
#download dependencies
RUN go mod download

#copy source file into image
COPY . .

#build the final executable
RUN CGO_ENABLED=0 go build -o /app  .


FROM scratch
COPY --from=build /app /app

#run the application
EXPOSE 8000
CMD ["/app"]



$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
# Stage 1 
FROM node:21 As backend-builder 

#setup the working Dir
WOKDIR /app

#code copy 
COPY .  .

RUN npm i

#stage 2 
FROM node:21-slim

#setup the working dir 
WOKDIR /app

#copy the above stage as compressed stage 
COPY --from=backend-builder /app .
COPY .env.sample .env
EXPOSE 5000
CMD ["npm", "start"]

================================

==========DokcerFile with USER intructions =======

# Start with a base image
FROM ubuntu:20.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Update and install necessary packages
RUN apt-get update && \
    apt-get install -y \
    curl \
    sudo && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Add a new user and group
RUN groupadd -r appgroup && useradd -r -g appgroup -m appuser

# Create a directory and adjust permissions
RUN mkdir -p /app && chown -R appuser:appgroup /app

# Set the working directory
WORKDIR /app

# Copy application files to the container
COPY app/ .

# Change user to the non-root user
USER appuser

# Command to run the application
CMD ["bash", "-c", "echo Hello from appuser!"]

====================================================================


=========Docker File with LABEL as example ===================
# Start with a base image
FROM python:3.9-slim

# Add metadata using LABEL instruction
LABEL maintainer="John Doe <johndoe@example.com>"
LABEL version="1.0"
LABEL description="A simple Python application"

# Set the working directory
WORKDIR /app

# Copy application files to the container
COPY app/ .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Command to run the application
CMD ["python", "app.py"]

=================================================






