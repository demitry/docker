#docker 
# Essential Docker for ASP.NET Core MVC
by [Adam Freeman](https://www.amazon.com/Adam-Freeman/e/B001IU0SNK/ref=dp_byline_cont_book_1) (Author)
- what it is, and why you should be using it for your ASP.NET Core MVC applications  
 - create a development platform for ASP.NET Core MVC
- Use Docker to test, deploy and manage ASP.NET Core MVC containers 
- Use Docker Swarms to scale up applications to cope with large workloads
## Ch 1

Containers work best for MVC applications that are  stateless, such that a series of HTTP requests from a single client can be handled by more than one instance  of the application, running in different containers. 
This doesn’t mean the MVC application cannot have any  state data, 
but it does mean that the state data needs to be stored so it can be accessed from any container,  such as by using a database. (I describe how to create an MVC application that accesses a containerized  database in Chapter 4.)

### Working with Images

- docker build
	- processes a Docker file and creates an image.  
- docker images
	- lists the images that are available on the local system. The -q argument  returns a list of unique IDs that can be used with the docker rmi command to remove  all images.  
- docker pull
	- downloads an image from a repository. 
- docker push 
	- publishes an image to a repository. You may have to authenticate with  the repository using the docker login command. 
- docker tag 
	- associate a name with an image.  
- docker rmi 
	- removes images from the local system. 
	- The -f argument can be used to remove images for which containers exist.

### Docker Images for ASP.NET Core MVC Projects

microsoft/aspnetcore:1.1.1 
- This Linux image contains version 1.1.1 of the .NET Core runtime  and the ASP.NET Core packages. This image is used to deploy  applications.  
microsoft/dotnet:1.1.1-runtimenanoserver  
- This Windows image contains version 1.1.1 of the .NET Core  runtime. This image is used to deploy applications to Windows  Server.  
microsoft/aspnetcore-build:1.1.1 
- This Linux image contains version 1.1.1 of the.NET Core Software Development Kit. It is used to create development environments in containers.  
mysql:8.0.0  
- This Linux image contains version 8 of the MySQL database server
haproxy:1.7.0
- This image contains the HAProxy server, which can be used as a load balancer.
dockercloud/haproxy:1.2.1
- This image contains the HAProxy server, configured to respond  automatically to containers starting and stopping

### Dockerfile Commands

FROM 
- This command specifies the base image. For ASP.NET Core MVC projects:
  - microsoft/aspnetcore:1.1.1 (for deployment) or
  - microsoft/aspnetcore-build:1.1.1 (for development) images.

WORKDIR 
- This command changes the working directory for subsequent commands in the Docker file.

COPY
- This command adds files so they will become part of the file system of containers that are created from the image.

RUN
- This command executes a command as the Docker file is processed. 
- It is commonly used to download additional files to include in the image or to run commands that configure the existing files.

EXPOSE
- This command exposes a port so that containers created from the image can receive network requests.

ENV
- This command defines environment variables that are used to configure containers created from the image.

VOLUME 
- This command denotes that a Docker volume should be used to provide the contents of a specific directory.

ENTRYPOINT
- This command specifies the application that will be run in containers created from the image.

### Docker Containers Quick Reference  

Containers are created from an image and used to execute an application in isolation. 
A single image can be  used to create multiple containers that run alongside each other, which is how applications are scaled up to  cope with large workloads. 
You can create containers using custom images or prebuilt images from a public  repository such as Docker Hub.

```
docker create -p 3000:80 --name exampleApp3000 apress/exampleapp
docker start exampleApp3000

or

docker run -p 3000:80 --name exampleApp4000 apress/exampleapp
```

### Arguments for create/run commands

```
-e, --env 		- sets an environment variable.
--name 			- assigns a name to the container.
--network 		- connects a container to a software-defined network.
-p, --publish 	- maps a host operating system port to one inside the container.
--rm 			- tells Docker to remove the container when it stops.
-v, --volume 	- is used to configure a volume that will provide the contents for a directory in the container’s file system.
```

### Essential Commands for Working with Containers

```
docker create   - creates a new container.
docker start    - starts a container.
docker run      - creates and starts a container in a single step.
docker stop     - stops a container.
docker rm       - removes a container.
docker ps       - lists the containers on the local system. 
docker ps -a        The -a argument includes stopped containers. 
docker ps -q        The -q argument returns a list of unique IDs, which can be used to operate on multiple containers with the docker start, docker stop, and docker rm commands.
docker logs     - inspects the output generated by a container.
docker exec     - executes a command in a container or starts an interactive session.
```

### Volumes

Volumes allow data files to be stored outside of a container, which means they are not deleted when the  container is deleted or updated.

Volumes are defined using the VOLUME command in Docker files

```
VOLUME /var/lib/mysql
```

This tells Docker that the files in the /var/lib/mysql folder should be stored in a volume. This is useful  only when a named volume is created and applied when configuring the container. 

Volumes are created  using the docker volume create command, like this:

```
docker volume create --name productdata
```

The --name argument is used to specify a name for the volume, which is then used with the -v argument  to the docker create or docker run command like this:  
```
docker run --name mysql -v productdata:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=mysecret -e bind-address=0.0.0.0 mysql:8.0.0
```
See ch 5


docker volume create 
- This command creates a new volume.
docker volume ls
- This command lists the volumes that have been created. 
- The -q argument returns a list of unique IDs, which can be used to delete multiple volumes using the docker volume rm command.
docker volume rm
- This command removes one or more volumes

### Docker Software-Defined Networks Quick Reference

Software-defined networks are used to connect containers together, using networks that are created and  managed using Docker. Software-defined networks are described in Chapter 5.

Create Software-defined networks:
```
docker network create backend
```

This command creates a software-defined network called backend. Containers can be connected to the  network using the --network argument to the docker create or docker start command, like this:  
```
docker run -d --name mysql -v productdata:/var/lib/mysql --network=backend -e 
MYSQL_ROOT_PASSWORD=mysecret -e bind-address=0.0.0.0 mysql:8.0.0
```
Containers can also be connected to software-defined networks using the docker network connect  command, like this:  
```
docker network connect frontend productapp1
```
This command connects the container called productapp1 to the software-defined network called  frontend.

### Commands for Working with Software-Defined Networks

docker network create
- creates a new software-defined network.
docker network connect
- connects a container to a software-defined network.
docker network ls 
- lists the software-defined networks that have been created, including the ones that Docker uses automatically. 
- The -q argument returns a list of unique IDs, which can be used to delete multiple networks using the docker network rm command.
docker network rm 
- removes a software-defined network. 
- There are some built-in networks that Docker creates and that cannot be removed.

### Docker Compose Quick Reference
Docker Compose is used to describe complex applications that require multiple containers, volumes,  and software-defined networks. The description of the application is written in a compose file, using the  YAML format. Docker Compose and compose files are described in Chapter 6

#### Essential Configuration Keywords Used in Compose Files
version 
- specifies the version of the compose file schema. At the time of writing the latest version is version 3.
volume
- is used to list the volumes that are used by the containers defined in the compose file.
networks
- is used to list the volumes that are used by the containers defined in the compose file. 
- The same keyword is used to list the networks that individual containers will be connected to.
services
- is used to denote the section of the compose file that describes containers.
image
- is used to specify the image that should be used to create a container.
build
- is used to denote the section that specifies how the image for a container will be created.
context
- specifies the context directory that will be used when building the image for a container.
dockerfile
- specifies the Docker file that will be used when building the image for a container.
environment
- define an environment variable that will be applied to a container.
depends_on 
- specify dependencies between services. Docker doesn’t have insight into when applications in containers are ready, so additional steps must be taken to control the startup sequence of an application (as described in Chapter 6).

Docker files are processed using the docker-compose build command

```
docker-compose -f docker-compose.yml build
```

The containers, networks, and volumes in a compose file are created and starting using the  docker-compose up command.

```
docker-compose up
```
### Essential Commands for Docker Compose

docker-compose build 
- processes the contents of the compose file and creates the images required for the services it contains.

docker-compose up
- creates the containers, networks, and volumes defined in the compose file and starts the containers.

docker-compose stop 
- stops the containers created from the services in the compose file. 
- The containers, networks, and volumes are left in place so they can be started again.

docker-compose down 
- stops the containers created from the services in the compose file and removes them, along with the networks and volumes.

docker-compose scale
- changes the number of containers that are running for a service.

docker-compose ps
- lists the containers that have been created for the services defined in the compose file.
