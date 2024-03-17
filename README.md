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
-  Restart Policies in Docker - no, unless-stopped( it is similar to the always flag the only difference is once the container is stopped manually it will not restart automatically even after restarting the docker daemon, until we start the container manually again.), 
   on-failure(Restart the container if it exits due to an error), always (Always restart the container if it stops. If it's manually stopped, it's restarted only when the Docker daemon restarts or the container itself is manually restarted)
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
- #docker build. -t <nameof image:tag>            - adding an tag to image while creating image
- #docker image tag <imageid> <nameof image:tag>      - addition of tag to existing image
- #docker image tag <old image name>:<old tag>  <new image name>:<new tag>  - remain an tag to existing image
- #docker commit <container id>  <newimagename>        - create an new image with updated container information.
- #docker image history <nameof image>   - list layers of image
- #docker image inspect <name of image>  - details about docker image
- #docker image prune   - delete only dangling images + unreferenced by any container
- #docker image prune -a    - delete all images not referenced by any container 

