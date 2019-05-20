- [Images](#images)
  - [List](#list)
  - [Remove](#remove)
  - [Run](#run)
    - [Run Options](#run-options)
- [Containers](#containers)
  - [List](#list-1)
  - [Stop](#stop)
  - [Kill](#kill)
  - [Remove](#remove-1)
  - [Shell Access](#shell-access)
  - [Show Tasks (Processes)](#show-tasks-processes)
- [Dockerfile](#dockerfile)
  - [Directives](#directives)
  - [Build](#build)
- [Publish and Pull Image](#publish-and-pull-image)
  - [Publish](#publish)
  - [Pull and Run](#pull-and-run)
- [Services](#services)
  - [docker-compose.yml](#docker-composeyml)


* **Docker for Mac/Linux/Windows** − It allows to run Docker containers.
* **Docker Engine** − It is used for building Docker images and creating Docker containers.
* **Docker Hub** − This is the registry which is used to host various Docker images.
* **Docker Compose** − This is used to define applications using multiple Docker containers.





# Images

* An image is an **executable package** that includes everything needed to run an application (the code, a runtime, libraries, environment variables, and configuration files).

## List
```sh
docker image ls -a
```

## Remove
```sh
docker image rm <image id>

# Remove all
docker image rm $(docker image ls -a -q)
```

## Run
```sh
docker run <image-name>
```

### Run Options
* **-it**
  * interactive
* **-d**
  * detached run (run in background)
* **-e**
  * set environment variable (KEY=VALUE)
* **-p local-port:container-port** 
  * publish container port to local port
* **-v local-dir:container-dir** 
  * bind disk volumes between container and local filesystem
* **--name \<my-container-name\>**
  * assign a name to a container




# Containers
* A container is a runtime instance of an **image**.
* A container runs natively on Linux and shares the kernel of the host machine with other containers.

![vm-vs-docker.png](./vm-vs-docker.png)

## List
```sh
docker container ls -a
```

## Stop
```sh
docker container stop <hash>
```

## Kill
```sh
# Force stop
docker container kill <hash>
```

## Remove
```sh
docker container rm <hash>

# remove all
docker container rm $(docker container ls -a -q)

# force remove
docker container rm <hash> -f
```

## Shell Access
```sh
docker container exec --it <my-container-name> bash
```

## Show Tasks (Processes)
```sh
docker container top <my-container-name>
```




# Dockerfile
* Dockerfile defines what goes on in the environment inside your container.

## Directives
* **FROM**
  * Base image
  * **Ex**: FROM nginx:latest
* **WORKDIR**
  * Container working directory
  * **Ex**: WORKDIR /usr/share/nginx/html
* **COPY**
  * Copy files
  * **Ex**: COPY *.html .
* **RUN**
  * Run command
  * **Ex**: RUN npm i
* **ENV**
  * Set environment variable
  * **Ex**: ENV NODE_ENV production
* **EXPOSE**
  * Container port definition.
  * **Ex**: EXPOSE 3000
* **CMD**
  * Run application when the container launched
  * **Ex**: CMD ["npm", "start"]


## Build
* This generates image.
  
```sh
docker build -t mysuperapp:v0.0.1 .
```





# Publish and Pull Image

## Publish
```sh
# login
docker login

# tag the image
docker tag <image-name> username/repository:tag

# push tagged image
docker push username/repository:tag
```

## Pull and Run
```sh
docker run username/repository:tag
```



# Services
* In a distributed application, different pieces of the app are called “services”.
* A service only runs one image, but it codifies the way that image runs
  * what ports it should use
  * how many replicas of the container should run so the service has the capacity it needs
  * and so on
* Scaling a service changes the number of container instances running that piece of software, assigning more computing resources to the service in the process.
* To be done with **docker compose** and **docker-compose.yml** file

## docker-compose.yml