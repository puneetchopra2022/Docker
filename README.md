# Docker
Docker Learning 

**Introduction to Docker**

-  Why do we need Docker - Hardware compatibility, OS compatibility, Libraries Dependencies
-  What can docker Do - Run an application on an Isolated container, that has its own network interfaces, libraries, and processors like virtual machines except for all containers that share the same OS kernels.( Hardware > OS > Docker >Conatiner)
-  Virtual machine VS Docker  - Bootup time, Storage, Utilization.
-  Docker Edition - Community Edition(free) , Enterprise Edition (paid).
-  Docker installation - Windows(docker desktop) , Linux( docker)
-  #systemctl status docker    -command to check docker daemon service running status
-  #systemctl start docker - command to manual start docker daemon
-  #systemctl stop docker - command to stop docker daemon
-  #systemctl enable docker  -command to ensure docker daemon service starts after restart of base machine Hardware
-  #docker version - check the version of the docker
-  A disadvantage of docker - data inside a docker container is not persistent on restart or stop has been made on the container level
-  Docker image: A Docker image is a file that contains all necessary dependencies & Configuration which is used to run an application.
-  Docker Container: is a running instance of an image
-  #docker pull <imagename>  -command to pull image
-  #docker image ls  -command to list images
-  #docker run <imagename>   - command to launch an container
-  #docker ps  - list all active container list
-  #docker ps -a - list all active & exit container list
-  #docker stop <containerid.name>   - stop a container
-  #docker rm <container id/name>  - remove an container
-  #docker rmi <image name>    - delete an image from disk
-  #docker logs <container ID>   - check log of container
-  #docker run --name <containername>  <imagename>    - command to run container with specific name
-  #docker inspect <container ID/Name>    -   get details about active container
-  #docker run -d <imagename>    - run container in detached mode
-  Expose in docker: This means on which port your application will listen to
-  Publish in docker: This means For a container port to be accessible through the docker host, we need to publish it.
-  #docker run --expose 8765 nginx
-  #docker run -d -p 8080:80 <imagename>   - run an container with publish port
-  #docker exec -it <conatiner name> bash    - exec into container
-  #docker run -d <imagename> command - command to override the container default command
-  Restart Policies in Docker - no, unless-stopped( it is similar to the always flag the only difference is once the container is stopped manually it will not restart automatically even after restarting the docker daemon, until 
   we start the container manually again.), on-failure(Restart the container if it exits due to an error), always (Always restart the container if it stops. If it's manually stopped, it's restarted only when the Docker daemon 
   restarts or the container itself is manually restarted)
-  #docker run --restart unless-stopped <imagename>
-  #docker system df    -command to check disk space
-  #docker system df -v   - command to show container & image vise size
-  #docker run -d  --rm <imagename>   command to delete an container after exit
-  #docker rename <oldcontainername>  <newcontainername>        - command to rename container
-  #docker top <conatiner ID> — command gives details of Process id belonging to the container
-  #docker ps --size  – command gives information on the current size of the docker container
-  #apt install  net-tools ------ to install ifconfig package
-  #apt install  procps  ----- to install the TOP command package 

**Image Creation, Management & Registry**

- A docker file is built to form an image that holds a set of instructions to run an application.
- #docker build.     - build an image
- docker instructions - FROM, CMD, RUN, COPY, ADD , EXPOSE, HEALTHCHECK(HEALTHCHECK --interval=5s CMD ping -c 1 172.17.0.2), WORKDIR, ENTRYPOINT(ENTRYPOINT [‘’/bin/ping ’’])
- Reason why every docker image start with FROM : FROM instruction initializes a new build stage and sets the Base Image for subsequent instructions. As such, a valid Dockerfile MUST start with a FROM instruction
- #docker build . -t <nameofimage>:tag            - adding an tag to image while creating image
- #docker image tag <imageid> <nameofimage>:tag      - addition of tag to existing image
- #docker image tag <oldimagename>:<oldtag>  <newimagename>:<newtag>  - remain an tag to existing image
- #docker commit <containerid>  <newimagename>        - create an new image with updated container information.
- #docker image history <nameofimage>   - list layers of image
- #docker image inspect <nameofimage>  - details about docker image
- #docker image prune   - delete only dangling images + unreferenced by any container
- #docker image prune -a    - delete all images not referenced by any container
- Docker Registry - The concept of Registry comes in place as it's not recommended to have custom images on a local repository on a local disk it is available on the registry that is easily accessible to anyone  - Docker Hub, AWS 
  ECR, Docker Registry, Docker Trusted registry
- Docker Hub repo push & pull
       : Create an Docker Hub account
       : #docker login
       : #docker image tag <currentimagename>:tag  puneet2022/learning:<imagenamewhichneedtobereflectinHUBrepo>
       : #docker push puneet2022/learning:<imagenamewhichneedtobereflectinHUBrepo>
       : #docker pull puneet2022/learning:<imagenamewhichneedtobereflectinHUBrepo>
- #docker search <nameofimage>   - Command to search a specific image
- #docker save <imagename>    >   <imagename>.tar    - command to save the image into tar to export to a different system
- #docker load -i < imagename.tar                  - command to extract an image from tar
- Concept of docker layer: Each instruction belongs to a single layer, the container top layer is a writable layer, and every container uses the same image until the write has been made on the container.
- #docker run -dt -P <nameof image >  -  this command all expose of port on random publish ports

**Networking**

- Docker network Drivers  – Community Edition (Bridge, None, Host)  Enterprises Edition (Bridge, Host, none, Macvlan, overlay)
- Bridge network   - it creates software bridge container tag with bridge network can communicate with each other and provide isolation to another container
- Host network - it removes the network isolation between the docker host & docker container, i.e container created with publish port 80 can be reached with port 80 outside the world as well
- None network -  networking is completely disabled in this drive type
- #docker network ls    - list all available network drivers
- #docker inspect network <drivername>    - details of network driver
- #docker run -dt --network <nameofdriver>   <imagename>   - run container on specific container.
- #docker network create --driver bridge <nameofcustombridgenetwork>    - create a user define bridge network
- #docker network create --driver bridge <nameofcustombridgenetwork>  --gateway 172.0.0.1 --subnet 172.0.0.0/24  - create an user define bridge network with custom subnet IP range

**Storage & Logging Driver**
- #docker info   - check active storage Driver & logging Driver
- Storage Drive: Works on the principle of copy on write, which means a single copy has been used until the write has been made. (cd /var/lib/docker/overlay2), almost all linux based system are using overlay2 drivers
- logging Driver - json-file(the default driver, storing logs in JSON files.) ,awslogs ,syslog( Forwards logs to a syslog server) ,splunk , local, none
- #docker info -  to check login driver
- #docker container inspect <containerID>  -  to check the logging driver in running container
- #docker run -dt --log-driver <specifytypeofloggingdriveryouwant>  <imagename>   -  way to change logging driver to container
- update logging driver for all containers following changes in the daemon.json file located under /etc/docker

**Volumes**
- Docker Volume - data inside a container is not persisted so the way to make data available even after termination of the container is via the local mount point and bind Mount ( When you use bind mount, a file or directory on 
  the host machine is mounted into a container ) Volume can be mounted on multiple containers simultaneously & volume cant be deleted automatically.
- #docker volume ls    - list active volume.
- #docker volume create <nameofvol>   - manual create an persistence volume from local docker host volume
- #docker volume inspect  <name of volume>
- #docker run -dt -v <nameofvolume>:<mountingdirofcontainer>   <imagename>    - command to manual mount an local volume host machine to container directory
- #docker run -dt -v  <pathoflocalfile>:<pathofcontainerwhereoutputfile> <imagename>   - command to manual mount file on container i.e bind mount
- #docker run -dt -v /home/docker/index.html:/usr/share/nginx/html/index.html  nginx    - command to manual mount file on container i.e bind mount
- #docker run -dt -v /home/docker/:/var/log   nginx sleep 500000    - command to manual mount directroy on container i.e bind mount

       
