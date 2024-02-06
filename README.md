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

## Docker

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
