# Container

## Where to deploy an app nowadays?

1. Bare Metal: Like literally a server physically front of you.
   
2. Virtual Machine: Like EC2, you can configure the server setting it by yourself, and you are renting it from Amazon.
   
3. Container: This is a newer approach compared to the previous methods. Your app is running inside the lightweight, isolated environment. One OS for all containers and host. Fewer resources than VMs.

## VM vs Container?

You see the difference, 
  
  The VM needed the hypervisor, and it could have multiple OS and Apps could run on different OS. 
  
  The container has container engine, and operating system is shared by all 3 applications.

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/507d3a46-2372-4b7e-845d-a21174462972)

## Docker Concepts

### What is Docker?

Docker has many elements such as Docker Clients, Docker engine, Docker Desktop, Docker Registry, Docker Objects. You can not just say what is a Docker. 

Instead you should ask like waht is Docker Engine? 

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/d3e73615-10bb-4fe6-b254-62f461c98ac1)

Okay then, what are those?

#### Docker Engine

Docker Engine includes a server. (Docker Daemon)

Dockerd server manages docker objects via API requests. 

Docker API (REST) is similar to the express server you are so familar with

  Get /containers/json
  
  Post /containers/create

Docker cli

  docker run

  Make API calls via cli

#### Docker Objects:

##### Docker File

Don't over think it. This is just a text file. 

This is just the instructions to create a docker image.

##### Docker Image

Container Template.

Just like a class in OOP.

Immutable, read-only.

Images stacked in layers.

##### Docker Container

Runnable instance of the image. (think about the object created by a constructor of a class from Docker Image)

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/59b4f311-3346-42e6-bec5-aa28cfd3caa1)


#### Docker registeries

Collection of pre-built Images

Docker files goes to Git tracked by version control. 

And docker images go to the Docker Registries.

There are difference between Public or Private

Typical registries like:

  Docker Hub
  
  Github Package Reistry
  
  Amazon Elastic Container Registry
  
  Private Registries

### Docker provides a Linux Kernel

If the host OS is Linux, then docker would use the running OS. However, if it is on Windows or macOS, we will run a Linux VM (provided by the Docker Desktop)

Essentially, multiple processes are isolated and running on the same OS. 

Each process has its own separate concept of what the hardware, file system looks like. 

Limits imposed on what each process can use, access, and see within the system.

### Docker borrowed all these features from Linux:

1. Control a process view of the filesystem (limited area, overlay multiple filesystem)
   
2. Isolate processes from each other.
   
3. Control which hardware and system reosources a process can use.
   
4. COntrol how muhc CPU,RAM,Network a process can use.
   
5. Control which system calls a process can make.

### Docker Desktop

This is a GUI app to manage docker on Windows or Mac.

Docker Destop includes docker engine, docker cli, and docker-compose cli. 

Helps manage resources like containers, images, and volumes. 


## docker commands


### A basic run with options

docker run will go the docker hub

find the image you are trying to run

download it and build a container based on the image

then finally start running that image

for example:

```bash
# https://docs.docker.com/engine/reference/builder/
# --name If the name is not specified, then docker will generate a name for this with some random strings.
# -- for the long version and - for the short version
# -i is for interactive container, so you could run commands in the 
docker run -i -t --name test ubuntu

```

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/de6b63e2-725c-4e44-8d86-ed4d10e6c891)

You will also notice that from the docker desktop -> container 

There is a container running on the host machine. 

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/46387b22-795e-4fc1-acb6-efcc55af8c63)

When you type exit, you exited out from the ubuntu.

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/be5df19c-36ad-423c-bdac-d39e075a5ad1)

Accordingly, inside the docker desktop -> container 

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/e60270bc-bd18-4806-ab09-c60380ace18e)

### A docker run with options, commands and arguments

```bash

 docker run -it --name test ubuntu echo "I am now sending args and commands into the Ubuntu container"

```

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/c026a62f-9caf-4b1c-b2e1-7b8266a99a6d)

### A docker run with auto cleaning container after the command runs

```bash

docker run --rm -it ubuntu

```

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/817243b2-64c8-4978-bd9b-04f312dbaa6d)

Cleaned as you expected

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/c1cc6970-871a-4273-a229-a91f268917c5)

### A docker run with httpd server

```

docker run --rm httpd

```
notice this is not working as you expected because this is a daemon process.

You could not interact with it.

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/5bb8684e-8585-4ad2-865f-52ebf00e2697)

How to run this thing at the backgroud?

```bash

docker run -d --rm httpd

```

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/402c3694-ef2f-4eed-a919-b760fd9fcca5)

nice but how would I know if it running? Using some ubuntu ish docker commands like 

```bash

docker ps

```

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/4bacd61a-f341-4323-a06e-6a018f902ebe)

and you could use docker commands to check a container's log

```bash

docker logs 136ccccfa4e98654436f3f8237ea81cb2a9d658d4c53ccb36d9e96e474305b61

```

you could use docker id or docker auto-assigned name

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/e3077529-f766-41d1-8dd4-5e1add39c4ee)

Now it is the time to stop the container by using docker command. 

```bash

docker kill dockerId

```

But remember this is a server you could not interact with it by using http request. So how to make it work?

### Making a server container running on a port

-p meaning publish the port on the host 

the port in the host device : the port in the container

So you are binding the host's 8080 with the container's 80 as the below example shows

```bash

docker run --rm --name apache_server -p 8080:80 -d httpd

```

Now see if it is running as expected:

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/9c884fdf-ab09-4781-a518-bd9a7571d86e)

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/b7d163dc-eea3-4e3c-8f2e-a6db205147ec)

### sending a command to the running container 

This will need you to use docker exec -it id/name

```bash

docker exec -it fb3a675e727caff76691229ebbd8fbd0c5d7a4ae4e05d1c602b6751b661cbc12 bash


```

I told that server to open a new shell for me

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/ba178719-ad79-4dba-8740-245de6e43550)

So stuff you could do on this apache server

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/4f04a10c-a48b-4a72-bdc6-18e2618fe9f2)

And you got this

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/30a347a5-3ac6-4562-abf7-e4a566b44a5f)

### Or even sending a file to container

just like how you create a file in the other systems by using VS code

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/e6e70124-a5d8-4bb1-b520-991c9d54daa1)

[image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/111b9175-eed5-42c1-87c1-913aadd2d799)

now the trick comes

docker run --rm --name apache_server -p 80:80 -v path/in/host:/path/in/container -d httpd

for example:

```bash

docker run --rm --name apache_server -p 80:80 -v /home/zkrguan/sample/sample.html:/usr/local/apache2/htdocs/ -d httpd

```

run the command with lil trouble shooting

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/6f01dc94-3136-4a78-b170-88b4a793710e)

It is like binding the path with host and the container

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/5e8c137a-9508-46c7-98e3-35ff0c8ec9b8)


## Dockerfile and docker build

For Docker Engine to builds a Docker Image, it needs 

1. Docker FIle defines the set of instructions to be executed.

2. Build Context defines the set of files and directives. => Everything in the same folder as the docker file.

An example of the build context:

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/c723ffa0-be0d-476c-9d86-247a3a63d58f)

node_modules => don't need for docker image building

.git => don't need for deployment 

src => big but needed 

index js => of course needed

What to do with the uneeded files in the build context?

### .dockerignore

Typically exclude these files from building.

```md

./git

node_modules

env.*

# meaning override the rule and including env.test
!env.test

build/dist/

```

### dockefile

#### what is it?

It is the instructions for the Docker Engine

#### where to put?

Put it into the root of the project

#### Each line is running one by one in the sequence

#### New layers are created for each line

some are thrown away, but some get cached and reused if nothing changes

#### format?


```docker

# use hash tag as the comment symbol

# blank lines are ignored

# use vertical whitespace

# Don't be afraid to use these. 

```

#### syntax

##### FROM

1. Every Dockerfile must include a FROM instruction

2. Defines the base image to use: every image is based on another image. 

   Base images, be as general or specifc as you need:
   
   Linux distro - ubuntu, debian, centos, amazonLinux
   
   Language Runtime - Python, gcc, npm bash, java. 

3. Images are built for a particular chip architecture:

   arm64 vs amd32 vs s390x vs x86
   
5. Images are versioned with tags, pick the right version

   node, node:lts, node 16, node:latest
   
6. Images are based on other images, pick the right size

   node:16-alpine, node:16-bullseye

The goal is choosing the version it will work for you the best.

For Rust and Go, you can just run stuff in binary.

FROM scratch 


##### Label

This is optional 

Add arbitray metadata to an image

key=value pairs

```docker
# notice the syntax by using \ to separate a long line into two lines
LABEL maintainer = "Zhaokai@gmail.com" \
      description = "Database container for app"
```

People like to write docker files like this:

Just to save all the LABEL key word

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/a191b314-f4e0-40cf-b3cf-de6e4cc08e72)

##### ENV, ARG

ARG key = value

   Default values for build-time variables

   Can be overriden at build-tiem via docker build --build-arg

   Not avaliable in the final image

ENV key = value

DEfault values for environment variables

Can be overridden at runtime via 'docker run -e'

Great way to pass config options to a container when it starts. 

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/de788569-2857-40cd-be01-c0ed79bf5e3b)

ARG: I can use docker build --build-arg NODE_VERSION=16 to override

ENV: I have to change the env variable

docker run --env LOG_LEVEL=debug fragments

docker run -e LOG_LEVEL=info -e PORT=5000 fragments

docker run --env-file .env fragments

##### WORKDIR 

Define a working directory within the image

Create the directory if necessary

All paths can be relative to this directory  within the image thereafter

Include as manay WORKDIR instrucitons as you needed (mostly just one)

##### COPY

We almost always need to COPY files and folders into the image. 

We COPY:

   From a source in the Build Context 

   TO a destination in the image file system


![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/4c4c947f-112c-4cc7-a606-c15e19ef20ff)



##### RUN

RUN command arg1 arg2 

example like 

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/a11a2a8d-3c69-4812-b163-d395262015fe)

This is why you don't need to copy the node_modules over to here. 



##### CMD

When you run this, what would you like to happen in the image?

like docker run image-name

A program , command, or a script you want to call

CMD ["node", "server.js"]


##### EXPOSE

If you run this container, what port number will be showing. 

Binding 80 with local host 8000. 

![image](https://github.com/zkrguan/Container_and_Docker/assets/97544709/5653b1cd-cb40-4817-84bb-582fc5100ace)


### docker build 

docker build path

docker build . 

docker build -f Dockerfile.debug ../docker



### A full walk through example of the dockerize a project:

1. You have to think about what to write down on the .dockerignore

The general rule is 

whatever you need for developement 

and 

whatever generated contents 

should be in the .dockerignore file

2. Moving onto the Dockerfile

```
# 1. BASE IMAGE, STICK WITH A SPECIFIC VERSION 
FROM node:version code

# 2. Label for recording your author's information and more

LABEL maintainer="Zhaokai Guan <zkrguan@gmail.com>" \
      description="My notes about how to write dockerfile"

# 3. Define env vairables

ENV NPM_CONFIG_LOGLEVEL=warning \

# 4. Where to put the work in the image?

# I put everything into the app
# This is like create folder and then cd into
WORKDIR /app

# 5. copy the package information over to the work dir

COPY package*

# 6. install the packages from the list and move the source code files into the image

RUN npm ci

COPY . .

# 7. npm run build

RUN npm run build

# 8. run the application

CMD npm run serve

# 9. EXPOSE the port to the Public

EXPOSE 1234 : 1234

(left is the local right is the container's)

```

