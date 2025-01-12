`docker run -it ubuntu `
checks if you have a ubuntu image locally
otherwise it downloads the ubuntu from hub.docker.com
it creates a container 
container behaves as a virtual machine but lighter and more portable
now you should be in the container
ctrl + d to stop the container

images behave as an os
the containers run the images
the containers are isolated from each other untill we want it to

`docker images `
or `docker image ls`
lists all the images on the mahine

`docker container ls`
lists all the docker running containers
-a : all containers

`docker start <container name>` : to start the container

`docker stop <container name>` to stop the container

`docker run` : new container

`docker run -it <image_name>`
new container with the given image 

docker run -it --name container_name image_name
give a name to your container

execute a command in a container
`docker exec <container name> <command to execute>`
but this command runs the command and return back to the host machine 

`docker exec - it <container name> bash`
it : interactive
it joins the host terminal with the container terminal


## Port Mapping
If you are running an application on a port in a container, you cannot access the application from the host machine at that port.
In order to do that we need to expose the ports

Command :
`docker run -it -p 1025:1025 image` 
-p : port mapping
this command exposes the port(1025) in the container to the port on the host machine

now we can access the port from our host machine

Note : we can mount the container port on some other port on host machine as well
ex : `docker run -it -p 9000:1025 image`


## Environment variables
Passing  environment variables to your docker container : 
`docker run -it -p 1025:1025 -e key=value -e key=value mailhog/mailhog`
mailhog/mailhog : image example

## Dockerization of a Node.js Application 
Create a file Dockerfile in your project
> [!warning]
> File name must be : Dockerfile

we need to make an image of the project so that other users can also use it using this image

You need a base image i.e. UBUNTU 
```Dockerfile
FROM ubuntu 
```
can also use node, then wont need to install node separately

now check how you can install node.js on ubuntu 
Ex :
```Dockerfile
RUN apt-get update
RUN apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get upgrade -y
RUN apt-get install -y nodejs
```
all the other commands will be run in the container
like installing curl, installing node v18, etc.

To copy package.json and other files from the project directory to the container
```Dockerfile
COPY package.json package.json
COPY package-lock.json package-lock.json
COPY main.js main.js
```
// COPY source destination
good practice to have the same names across both locations
`COPY . .` // to copy all the files 

to generate the node_modules within the container
```Dockerfile
RUN npm install
```

```Dockerfile
ENTRYPOINT [ "node", "main.js" ]
```
This command states that whenever the container is started, we need to run the following command

Final Dockerfile config file : 
```java
FROM ubuntu

RUN apt-get update
RUN apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get upgrade -y
RUN apt-get install -y nodejs

COPY package.json /app/package.json
COPY package-lock.json /app/package-lock.json

RUN cd app && npm install

COPY main.js /app/main.js

ENTRYPOINT [ "node", "app/main.js" ]
```
Note : the order in which you make config file matters. 
Add files first which do not change frequently and lastly the files updated more often, this helps in caching the files and takes less time to build the image after changes are made
This is called **layered caching** 
All layers after the updated file or command are built again

Now convert the whole thing into an image
`docker build -t youtube-nodejs .`
youtube-nodejs  : image name
. : path of the docker file config (. means in the same location)

now you can build your containers with the image as usual 


# More for Dockerfile : 

Ignore files : 
> [!warning]
> .dockerignore
 ignores files you do not want to copy like node_modules

use .dokerignore if you are using COPY . . command

Instead of mentioning /app/ everytime to copy or perform directory operations manually 
set the working directory 
`WORKDIR /app ` : tells docker that all the code below this layer will be executed in the /app directory 

## Publishing to Hub
hub.docker.com

create account
create repository
public
create 
build image with the name given in the docker hub

`docker push <new-image-name>`

you might need to login login on the root device

## Docker Compose

Used to create, setup and destroy multiply containers

create file **docker-compose.yml** in the project folder

> [!warning]
> Must name the file docker-compose.yml

add configurations for all the files in the file

```yml
version: "3.8"

services:
  postgres:
    image: postgres # hub.docker.com
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_DB: review
      POSTGRES_PASSWORD: password

  redis:
    image: redis
    ports:
      - "6379:6379"
```

`docker compose up` : start all containers

ctrl + c : stop all containers

`docker compose down` : remove all containers
`docker compose up -d` : run the containers in background


# PART 2
---

# Docker Networking 
There are various drivers using which our container can talk to the internet.

`ping www.google.com`

`docker network inspect bridge`
list of containers connected to the network bridge
your docker containers get an ip address using which they can communicate with the internet

`docker network ls`
all the networks and drivers available locally 

` docker run -it --network=host busybox`
 the container is not provided any bridge, it is directly connected to the internet using the host machine network

Note : While using bridge you need to expose ports but while using host network we don't need to expose ports manually because they use the host machine ports automatically

`--network=none`
This container will not have access to internet


# Create your own network
`docker network create -d bridge youtube`
-d : driver
bridge : network mode
youtube : name of the network

docker run -it --network=youtube --name tony-start ubuntu

two containers running on the same network can communicate with each other without needing ip address
lets say you have two containers with youtube network named tony_stark and dr_strange 

`ping tony_stark` on dr_strange 

# Docker Volumes
When a docker container is destroyed, data stored in it (its memory) is also lost.
To prevent this we use Docker Volumes.
Docker Volumes work as permanent storage for containers

Volume mapping
Mount a folder in your host machine to a container
The container will have access to only that folder of the host machine

`docker run -it -v <folder path on host>:<folder path on container> <container-image>`
-v : Volume mapping 
example : 
```
docker run -it -v /users/ayush/desktop/test-folder : /home/abc ubuntu
```

 any changes made in the container will also be reflected in the root device folder and vice versa
Also if the container is deleted, the data will not be deleted.

# Docker Multi stage builds
Up until now we were doing single stage builds i.e. take a base image and build around it. 

Let for example 

Typescript .ts -> in simple js 
we need to use typescript just for the build but not after that 
which results in increase in the whole size of the image
To resolve this we use multistage builds

```Dockerfile
FROM ubuntu as build

RUN apt-get update
RUN apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get upgrade -y
RUN apt-get install -y nodejs
RUN apt-get install typescript // example only not real

WORKDIR /app

COPY package.json /app/package.json
COPY package-lock.json /app/package-lock.json

RUN npm install
RUN tsc -p . # build

FROM ubuntu as runner

WORKDIR app/
copy --from=build app/ /
```

two stages are build and runner 
packages like typescript and others will not be forwarded to the runner stage
You can have multiple such stages 



