Tutorial by Piyush Garg 
part 1 : https://www.youtube.com/watch?v=31k6AtW-b3Y
part 2 : https://www.youtube.com/watch?v=xPT8mXa-sJg

Docker is a platform that allows developers to build, package and run applications inside containers.

**Docker allows you to package an application with all its dependencies and run it consistently across different environments using containers.**

Docker solves the problem of replicating same project on multiple environments.

Docker solves problems like: 
1. Inconsistent environment due to which code works on one machine but fails on another
2. Dependency/ version conflicts 
3. Time consuming project setup
4. Configuring same environment for cloud deployment

#### Using Docker
Install Docker CLI & Docker Desktop

`docker -v`
Checks docker version on your system

`docker run -it ubuntu`
checks if you have a ubuntu image locally
if not, then it downloads the ubuntu image from hub.docker.com
it creates a container and runs it.

You can check the ubuntu image in docker as well.

inside the terminal it shows like : 
```
root@234gjhg2j3h4324kh:/#
```
- Now you are inside the container 
- Any commands you will run will be run inside ubuntu terminal
- The random string is container id

By default it gives a random name to the container 
To give your custom name to the container:  
`docker run -it --name <container-name> <image>`

Custom docker images : 
`docker run -it mailhog/mailhog`
the first mailhog signifies the repository / namespace e.g. organization on Docker hub
the second mailhog signifies the image name
helps with locating and fetching the correct image

#### Containers
container behaves as a virtual machine but lighter and more portable
Containers are lightweight isolated processes that share the host OS kernel, unlike virtual machines which include a full OS.

now you should be in the container
ctrl + d to exits the container shell. Use `docker stop <container>` to stop the container explicitly

Docker Containers can have different versions of tools installed than your local machine.
For example you machine could have node version 19 while a container can have node version 21.
Multiple containers can have multiple versions running.

#### Images
images behave as an os
Docker images are read-only templates containing application code, runtime, libraries, and dependencies.
- the containers run the images
- the containers are isolated from each other (until we want it to communicate)

`docker images `
or `docker image ls`
lists all the images on the machine

`docker container ls`
lists all the docker running containers
`docker container ls -a` : list all containers (running and not running)

`docker start <container name>` : to start the container

`docker stop <container name>` to stop the container

`docker run` : spin up a new container

`docker run -it <image_name>`
new container with the given image 

`docker run -it --name <container_name> <image_name>`
give a name to your container

`docker exec <container name> <command to execute>`
execute a command in a container
but this command runs the command and return back to the host machine 

`docker exec -it <container name> bash`
it : interactive
it joins the host terminal with the container terminal


Note : 
Instead of installing a vanilla image like ubuntu and installing node on it 
you can also install a node image and continue further from there
Find the list of available docker images on hub.docker.com
ex : `docker run -it node`
Check for docker verified and docker official images 

## Port Mapping
If you are running an application on a port in a container, you cannot access the application from the host machine at that port.
In order to do that we need to expose the ports
Containers have isolated networking. To access services from the host, ports must be mapped using `-p`.

Command :
`docker run -it -p 1025:1025 <image>` 
-p : port mapping
this command exposes the port(1025) in the container to the port on the host machine

Now we can access the port from our host machine

Note : we can mount the container port on some other port on host machine as well
ex : `docker run -it -p 9000:1025 <image>`
9000 -> host machine port
1025 -> container port

## Environment variables
Passing  environment variables to your docker container : 
`docker run -it -p 1025:1025 -e key=value -e key=value mailhog/mailhog`
mailhog/mailhog : image example


## Dockerization of a Node.js Application 
Create a file `Dockerfile` in your project
> [!warning]
> File name must be : **Dockerfile**

DUMMY NODE JS APPLICATION : 
`main.js`
```js
const express = require("express");
const app = express();

const PORT = process.env.PORT || 8000;

app.get("/", (req, res) => {
	return res.json({message: "Hey, I am node js in container"});
});

app.listen(PORT, () => console.log(`Server startyed on port : ${PORT}`));
```

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
`COPY <source> <destination>`
```Dockerfile
COPY package.json package.json
COPY package-lock.json package-lock.json
COPY main.js main.js
```
good practice to have the same names across both locations

 to copy all the files instead of specifying all files manually
```Dockerfile
COPY . . 
```

if you copy all files and want to ignore some specific files from being copied use `.dockerignore` : 
create the file in the root directory  
ignores files you do not want to copy like node_modules
.dockerignore
```DockerFile
node_modules/
```

to generate the node_modules within the container
```Dockerfile
RUN npm install
```

```Dockerfile
ENTRYPOINT [ "node", "main.js" ]
```
This command states that whenever the container is started, we need to run the following command
so running `docker run my-image` internally becomes `node main.js`


Copy files into an isolated folder inside the container instead of just pasting it there : 
/app 
```DockerFile
COPY main.js /app/main.js
```
- helps to get a better control over our application

Instead of mentioning /app/ everytime to copy or perform directory operations manually 
set the working directory :
```DockerFile
WORKDIR /app 
``` 
- tells docker that all the code below this layer will be executed in the /app directory 
- now the container command line also starts from the /app as well

##### Final Dockerfile config file : 
```java
FROM ubuntu

RUN apt-get update
RUN apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get upgrade -y
RUN apt-get install -y nodejs

COPY package.json /app/package.json
COPY package-lock.json /app/package-lock.json

WORKDIR /app  
RUN npm install

COPY main.js /app/main.js

ENTRYPOINT [ "node", "main.js" ]
```

##### Layered Caching
Note : the order in which you make config file matters. 
Add files first which do not change frequently and lastly the files updated more often, this helps in caching the files and takes less time to build the image after changes are made
This is called **layered caching** 
All layers after the updated file or command are built again


Now convert the whole thing into a custom image
`docker build -t youtube-nodejs .`
`youtube-nodejs`  : image name
`.` : path of the docker file config (. means in the root)
-t : tagging an image, to assign name and optional version to the image

now you can build your containers with the image as usual 
`docker run -it -p 8000:8000 youtube-nodejs`

running the container with env value for PORT : 
`docker run -it -e PORT=4000 -p 4000:4000 youtube-nodejs`

## Publishing to Hub
hub.docker.com

create account
create repository
public
create 
build image with the name given in the docker hub (along with username)
`docker build -t <new-image-name>`
e.g. `docker build -t piyushgargdev/youtube-nodejs`

`docker push <new-image-name>`
e.g. `docker push piyushgargdev/youtube-nodejs

you might need to login on the root device to perform push
`docker login` 
then enter username and password

## Docker Compose

Real world projects might need to run multiple containers at the same time.

Docker Compose is Used to create, setup and destroy multiply containers

create file **docker-compose.yml** in the project folder

> [!warning]
> Must name the file docker-compose.yml

add configurations for all the files in the file

```yml
version: "3.8"

services:
  postgres:
    image: postgres  # hub.docker.com
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

`docker compose down` : stop all containers
`docker compose up -d` : run the containers in background



# PART 2
---

## Docker Networking 
Docker Networking enables communication between containers, the host system and external networks. 
It abstracts complex networking configurations and provides isolated flexible environments for containerized applications.

in a container terminal
`ping www.google.com`

There are various drivers/modes using which our container can talk to the internet.
a. bridge Mode  (default)
b. host Mode
c. ipvlan mode
d. macvlan mode
e. none

Explicitly Mentioning the network type : 
`docker run -it --network=<type> image`

while creating a container, if we do not specify the networking driver, it joins the container to a **bridge** network.

**Bridge** -> There is a link created between your container and your host machine, through which the container can access the internet or external network.

**Host** -> The container is directly connected to the network of the host machine. There is not separate bridge provided.

`docker network inspect bridge`
list of containers connected to the network bridge
your docker containers get an ip address using which they can communicate with the internet

`docker network ls`
all the networks and drivers available locally 

Note : While using bridge you need to expose ports but while using host network we don't need to expose ports manually because they use the host machine ports automatically

Host network mode removes isolation and directly uses host ports — **not recommended for production.**

`--network=none`
This container will not have access to internet/network

### Create your own network
`docker network create -d <driver-mode> <network-name>`
e.g. : `docker network create -d bridge youtube`
-d : driver

container run with youtube network :
`docker run -it --network=youtube --name tony-stark ubuntu`

two containers running on the same network can communicate with each other without needing ip address
lets say you have two containers with `youtube` network named `tony_stark` and `dr_strange` 

run `ping tony_stark` on dr_strange 

Using custom networks have an advantage : 
with custom networks with multiple containers, we do not need to manage ip addresses manually. We can use the container names instead.
And since ip addresses change in real world production, using names of containers is easier.


### Docker Volumes
When a docker container is destroyed, data stored in it (its memory) is also lost.
To prevent this we use Docker Volumes.
Docker Volumes work as permanent storage for containers

**Volume mapping**
Mount a folder in your host machine to a container
The container will have access to only that folder of the host machine

`docker run -it -v <folder path on host>:<folder path on container> <container-image>`
-v : Volume mapping 
example : 
```
docker run -it -v /users/ayush/desktop/test-folder:/home/abc ubuntu
```

/users/ayush/desktop/test-folder -> host machine folder
/home/abc -> container folder

- Any changes made in the container will also be reflected in the root device folder and vice versa.
- Also if the container is deleted, the data will not be deleted.
- We can also map multiple containers to the same host machine. This is a standard method for data sharing, configuration files or logs across different services.

You can also create your custom volumes : 
`docker volume create my-vol` 
and mount the volumes next time only instead of specifying it manually every time

### Docker Multi stage builds
Up until now we were doing single stage builds i.e. take a base image and build around it. 

Let for example 

Typescript .ts -> compiled to simple js to run 
for example: 
you will need to run typescript and ts -p to install typescript

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

FROM node as runner

WORKDIR app/
copy --from=build /app /app
```

ubuntu, node -> docker images
- Docker builds multiple intermediate images 
- Only the last stage (runner) becomes the final image
- The container runs only that final image

two stages are 
1. build 
2. runner

packages like typescript and others will not be forwarded to the runner stage
You can have multiple such stages

Note : Changes in one stage/image do not automatically propagate to another (like /app)
So we explicitly move data between stages using 
```
COPY --from=build /app /app
```
This takes /app from the build stage and copies it into /app in the runner stage
