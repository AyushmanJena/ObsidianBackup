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

COPY package.json package.json
COPY package-lock.json package-lock.json
COPY main.js main.js

RUN npm install

ENTRYPOINT [ "node", "main.js" ]
```
Note : the order in which you make config file matters. 
Add files first which do not change frequently and lastly the files updated more often, this helps in caching the files and takes less time to build the image after changes are made
This is called **layered caching** 
All lines after the updated file or command are built again

Now convert the whole thing into an image
`docker build -t youtube-nodejs .`
youtube-nodejs  : image name
. : path of the docker file config (. means in the same location)

now you can build your containers with the image as usual 

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