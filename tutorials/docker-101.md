
---

  

title: Docker

  

description: Docker

  

#
### Exploring Docker


**What is Docker**
#
 Docker is a software development tool and a virtualization technology that makes it easy to develop, deploy, and manage applications by using containers.
 
Container refers to a lightweight, stand-alone, executable package of a piece of software that contains all the libraries, configuration files, dependencies, and other necessary parts to operate the application.

**Difference between Docker and Virtual Machine**

- One of the popular approaches for running an application is over a virtual machine (VM). VMs run applications inside a guest operating system, which runs on virtual hardware powered by the server’s host OS.

    ![Docker architecture](_images/docker_docker-architecture.png  "Docker architecture")

- Containers have a different approach to running applications. Containers do not include an operating system. Containers package the application binaries, utilize a host’s operating system, and share relevant libraries and resources as necessary to run applications.

-  Table: Docker Vs Virtual Machines*

    ![Docker vs VM](_images/Docker-vs-VM.png  "Difference between Docker and VM")

#

  

**Why Containers ?**

Containers provide the following benefits
- Reduced IT management resources

- Reduced size of snapshots

- Quicker spinning up apps

- Reduced and simplified security updates

- Less code to transfer, migrate, and upload workloads


Unlike virtual machines, containers do not have high overhead and hence enable more efficient usage of the underlying system and resources.

The isolation and security aspects of Docker allow many containers to be run simultaneously on a given host. Once packaged, containers can be shipped and run anywhere.


**Docker Architecture**
#

Docker consists of a Docker Engine, which is a client server application. It contains the following components:

![Docker architecture](_images/docker_architecture.png "Docker architecture")

- A server, which is a type of long-running program called a "daemon process". The daemon creates and manages Docker objects, such as images, containers, networks and volumes.

- A REST API, which specifies interfaces that programs can use to talk to the daemon and give it instructions.

- A command line interface (CLI) client (the Docker command line).

- The Docker client talks to the Docker daemon, which does the heavy lifting of building, running and distributing Docker containers.

- When a Docker command such as *docker run* is executed, the client passes on this command to the daemon process, which in turn carries them out.

Now let's understand container and Images briefly:

- Containers allow the packaging of your application (and everything that you need to run it) in a “container image”. Inside a container you can include a base operating system, libraries, files and folders, environment variables, volume mount-points, and your application binaries.

- A “container image” is a template for the execution of a container — It means that you can have multiple containers running from the same image, all sharing the same behavior, which promotes the scaling and distribution of the application. These images can be stored in a remote registry to ease the distribution.


 ***Docker repository***
#
A ***Docker repository*** is where you can store 1 or more versions of a specific Docker image. An image can have 1 or more versions (tags).

***Docker images*** are read-only templates that are used to create Docker containers. Docker enables you to create new images, update existing images, or download images that others created. Docker images are the build component of Docker.
****

- A ***Docker image*** can be compared to a git repo. A git repo can be hosted inside of a GitHub repository, but it could also be hosted on Gitlab, BitBucket or your own git repo hosting service. It could also sit on your development box and not be hosted anywhere.

- The same goes for a Docker image. You can choose to not push it anywhere, but you could also push it to the Docker Hub which is both a public and private service for hosting Docker repositories. There are other third party repository hosting services too.

- The thing to remember here is a Docker repository is a place for you to publish and access your Docker images. Just like GitHub is a place for you to publish and access your git repos.

- It’s also worth pointing out that the Docker Hub and other third party repository hosting services are called “registries”. A registry stores a collection of repositories.

- You could say a registry has many repositories and a repository has many different versions of the same image which are individually versioned with tags.
#

 **Types of Container Registries**

There are lots of container registries out there. They can be broken down into several distinct categories:


- **Cloud-based registries:** Registries that are hosted in the public cloud by a third-party provider, such as Docker Hub.
    
- **On-premises registries:** Registries that you can install on your own infrastructure. You can create an on-premises registry manually using Docker, via Docker Registry (which is distinct from Docker Hub) or using a third-party registry like Quay.

- **Public registries:** Registries that anyone can access. Docker Hub is an example. So is Quay.io, a hosted public registry that is built using Quay. (To be clear, you can install Quay itself on your infrastructure; Quay.io is a hosted implementation of Quay designed for public access.)

- **Private registries:** Registries that require permissions in order to upload or download container images. You can create private registries using a service like Docker Hub, even though many Docker Hub registries are publicly accessible.


**Commonly used registries:**

- **Quay.io:**
Red Hat® Quay container and application registry provides secure storage, distribution, and deployment of containers on any infrastructure. It is available as an add-on for OpenShift or as a standalone component.

- **Docker hub:**
Docker Hub is the world’s largest repository of container images with an array of content sources including container community developers, open source projects and independent software vendors (ISV) building and distributing their code in containers. Users get access to free public repositories for storing and sharing images or can choose subscription plan for private repos.

- **Redhat Catalog:**
Mission-critical applications require trusted containers. Container images available from the Red Hat Ecosystem Catalog are built from base images that have been vetted by Red Hat’s internal security team and hardened against security flaws. Use the Red Hat Container Catalog (RHCC) to find container images that have been tested, secured, and verified by Red Hat.

]
**Docker Base Images**
#

A ****base image**** is the image that is used to create all of your container images. 
Your base image can be an official Docker image, such as Centos, or you can modify an official Docker image to suit your needs, or you can create your own base image from scratch.
****

The Docker Official Images are a curated set of Docker repositories hosted on Docker Hub. They are designed to:

 - Provide essential base OS repositories (for example, ubuntu, centos) that serve as the starting point for the majority of users.

 - Provide drop-in solutions for popular programming language runtimes, data stores, and other services, similar to what a Platform as a Service (PAAS) would offer.

 - Exemplify Dockerfile best practices and provide clear documentation to serve as a reference for other Dockerfile authors.

 - Ensure that security updates are applied in a timely manner. This is particularly important as many Official Images are some of the most popular on Docker Hub.

Docker, Inc. sponsors a dedicated team that is responsible for reviewing and publishing all content in the Official Images. This team works in collaboration with upstream software maintainers, security experts, and the broader Docker community.

#
***In the upcoming section "Use Red Hat Universal Base Images" , you will learn about different Red Hat based rhel/ubi base images and advantages of using them to create your application containers.***



### DockerFile


**What is Dockerfile**
****

A ***Dockerfile*** is a text document that contains all the commands a user could call on the command line to assemble an image. 
Docker can build images automatically by reading the instructions from a Dockerfile. 
The Dockerfile provides the instructions to build a container image through the command :

**docker build -t [username/]<image-name>[:tag] <dockerfile-path>**

It starts from a previously existing Base image (through the FROM clause) followed by any other needed Dockerfile instructions.

#

**Example Dockerfile**

*This example creates a custom WildFly container with a custom administrative user. It also exposes the administrative port 9990 and binds the administrative interface publicly through the parameter ‘bmanagement’.*




    # Use the existing WildFly image
    FROM registry.connect.redhat.com/newrelic-openshift/java-agent

    # Add an administrative user
    RUN /opt/jboss/wildfly/bin/add-user.sh admin Admin#70365 --silent

    # Expose the administrative port
    EXPOSE 8080 9990

    # Bind the WildFly management to all IP addresses
    CMD [“/opt/jboss/wildfly/bin/standalong.sh”, “-b”, “0.0.0.0”,
    “-bmanagement”, “0.0.0.0”]

#
  
**List of instructions used in the Dockerfile**

<span style="color:black">

| **Command**   |	**Description** |
| ------------- |   :-------------|
| FROM	        | Sets the base image for subsequent |
| MAINTAINER	| Sets the author field of the generated images  |
| RUN	        | Execute commands in a new layer on top of the current image and commit the results |
| CMD	        | Allowed only once (if many then last one takes effect) |
| LABEL	        | Adds metadata to an image |
| EXPOSE	    | Informs container runtime that the container listens on the specified network ports at runtime |
| ENV	        | Sets an environment variable |
| ADD	        | Copy new files, directories, or remote file URLs from into the filesystem of the container |
| COPY          | Copy new files or directories into the filesystem of the container |
| ENTRYPOINT	| Allows you to configure a container that will run as an executable |
| VOLUME	    | Creates a mount point and marks it as holding externally mounted volumes from native host or other containers |
| USER	        | Sets the username or UID to use when running the image |
| WORKDIR	    | Sets the working directory for any RUN, CMD, ENTRYPOINT, COPY, and ADD commands |
| ARG	        | Defines a variable that users can pass at build-time to the builder using --build-arg |
| ONBUILD	    | Adds an instruction to be executed later, when the image is used as the base for another build |
| STOPSIGNAL    | 	Sets the system call signal that will be sent to the container to exit |

</span>

### How to create & manage a Container using Docker


**Lab- Create an application**
#
In this section, we will explore what is required to run an application on the Docker host. We will accomplish this by following the steps below:

- **Create an application**

- **Create a Dockerfile**

- **Build an image from a Dockerfile for an application**

- **Create a container**

- **Access the application**

- **Manage containers**

- **Update the application code**

- **Update the Dockerfile to use ENV variables**

- **Recreate the image from the Dockerfile**

- **Create a container**

- **Test the container**

- **Delete images**

We will run through one Node.js example for our Docker demonstration. This is a "Hello world" Node.js setup, which we will be running with our Docker setup. We will also cover various related concepts during the demonstration.

***
**Create an application**

#
The application you will be deploying is a sample "Hello world" Node.js application. Before we start working on deployment, let's first create the files required for the Node.js application.


- #Execute the following command to navigate to your home directory

    cd $WORKING_DIR

- #Execute the following command to create a "nodejsapp" directory. This directory will contain the application codebase

    mkdir -p nodejsapp

- #Execute the following command to navigate inside the "nodejsapp" directory

    cd nodejsapp

- #Execute the following command to download the demo code .zip file from Git using the "curl" command

    curl -OL https://raw.githubusercontent.com/snippet-java/ops/master/docker-demo/node-demo.zip

- #Execute the following command to unzip the node-demo.zip file

    unzip node-demo.zip

- #Execute the following command to navigate to the "node-demo" directory

    cd node-demo
In order to run a Node.js app, Node.js requires a package.json file that describes the app and its dependencies.

Here is our package.json file, which lists the package "express" as a dependency:

- #Execute the following command to see the content of the package.json file

    cat package.json

The package.json file mentions a server.js file, which is the script file we need to spin up the REST app. Requesting this app will respond with "Hello world":

- #Execute the following command to see the content of the server.js file

    cat server.js


****
**Lab- Build Container Image**
****
  

**Create a Dockerfile**
#
Before we actually create a Dockerfile, let’s take a closer look at some Docker terminology.

The steps required to pack the application artifacts are listed with simple syntax in a file known as a "Dockerfile". In addition to artifacts, this file can also contain information such as how to start an application, or specify on which port the application will be listening:


- #Execute the following command to ensure that you are in correct directory

    cd $WORKING_DIR/nodejsapp/node-demo

- #Execute the following command to see the contents of the Dockerfile

    cat Dockerfile

Let’s discuss the contents of the Dockerfile.

**FROM registry.access.redhat.com/ubi7/ubi:latest** - Our application is Node.js, and a Node environment is required to run it. The "FROM registry.access.redhat.com/ubi7/ubi:latest" syntax will pull that base Docker image from a repository. We are installing Nodejs on top of the base image.

This also suggests that our application image will be based on another image, which in turn provides the Node environment for us.

**WORKDIR** - Creates an app directory in which to place all of the application artifacts inside a container. We will discuss containers shortly. This is the working directory.

**COPY** - Copies the files from the local directory we are using into the directory specified by our WORKDIR command.

**RUN npm install** - Installs all the libraries needed to run our Node.js application.

**EXPOSE 8080** - Exposes the port to communicate with the container.

**CMD [“npm”,”start”]** - Starts the application.



#
**Build an image from a Dockerfile for an application**
#

Let’s create an image from our Dockerfile, which will make an image of the Node.js application.

The `docker build -t [username/]<image-name>[:tag] <dockerfile-path>` command is used to build a Docker image. It begins with a previously existing base image (referenced with the FROM clause) and is followed by any other necessary Dockerfile instructions.

This process is very similar to the compilation of source code into binary output; however, in this case, the output of the Dockerfile will be a container image.

- #Execute the following command (from the directory containing the Dockerfile) to build an image from the Dockerfile created in the previous section

    docker build -t myrepo/node-web-app:v1.0 .

The -t option is used in the command above to tag the image. Tags are helpful for finding images via tag name. It is customary to tag images as `[username/]<image-name>[:tag]`.

If a tag is not mentioned with the -t option, then it is assigned the default "latest".

As the build progresses, Docker will print:

>Sending build context to Docker daemon  18.94kB
Step 1/10 : FROM registry.access.redhat.com/ubi7/ubi:latest
latest: Pulling from ubi7/ubi
a3ac36470b00: Pull complete 
82a8f4ea76cb: Pull complete 
Digest: sha256:a8884b9b7039bae8a11e13b80c905bc7448aa654b9cbf7c7e1ea7763bf730769
Status: Downloaded newer image for registry.access.redhat.com/ubi7/ubi:latest
 ---> d36cb7ab6004
Step 2/10 : LABEL name="APPLICATION NAME"       maintainer="EMAIL@ADDRESS.COM"       vendor="COMPANY NAME"       version="VERSION NUMBER"       release="RELEASE NUMBER"       summary="APPLICATION SUMMARY"       description="APPLICATION DESCRIPTION"
 ---> Running in 53f0202cba38
Removing intermediate container 53f0202cba38
 ---> 717ecd094055
Step 3/10 : COPY licenses /licenses
 ---> 994e5a216a4f
Step 4/10 : WORKDIR /usr/src/app
 ---> Running in d54bfe95bbb3
Removing intermediate container d54bfe95bbb3
 ---> dbff382e18cc
Step 5/10 : COPY package*.json ./
Step 6/10 : RUN curl -sL https://rpm.nodesource.com/setup_14.x | bash - &&     INSTALL_PKGS="nodejs gcc-c++ make" &&     yum -y update-minimal --enablerepo ubi-7 --setopt=tsflags=nodocs     --security --sec-severity=Important --sec-severity=Critical &&         yum -y install --setopt=tsflags=nodocs ${INSTALL_PKGS}
 ---> Running in c8320daacb29
>
>##Installing the NodeSource Node.js 14.x repo...
Complete!
>
>Removing intermediate container c8320daacb29
 ---> 0b8843b27e14
Step 7/10 : RUN npm install
 ---> Running in 49bc7b5883c0
>
>added 50 packages from 37 contributors and audited 50 packages in 1.943s
found 0 vulnerabilities
>
>Removing intermediate container 49bc7b5883c0
 ---> daf8fc6802aa
Step 8/10 : COPY . .
 ---> f1ab603926d2
Step 9/10 : EXPOSE 8080
 ---> Running in 6b606e4470d3
Removing intermediate container 6b606e4470d3
 ---> 7eb4c4ff0fe6
Step 10/10 : CMD [ "npm", "start" ]
 ---> Running in da99aaa322a1
Removing intermediate container da99aaa322a1
 ---> f2aef4b568ec
Successfully built f2aef4b568ec
Successfully tagged myrepo/node-web-app:v1.0

- #Execute the following command to list the images and verify that the "myrepo/node-web-app:v1.0" image is listed

    docker images | grep myrepo/node-web-app

> REPOSITORY              TAG            IMAGE ID            CREATED             SIZE
>
> myrepo/node-web-app     v1.0           d4c361929838        14 minutes ago      909 MB


<br/>

****
**Lab- Application Container Lifecycle**
****

**Create a container**
#

As we've already discussed, a container is a runnable instance of an image. Let’s create a container out of the image "myrepo/node-web-app:1.0".


The docker run command can be used to create and run a container.

- #Execute the following command to run the container from the image "myrepo/node-web-app" and name the container "node_web_app"

    docker run --name node_web_app -p 49160:8080 -d myrepo/node-web-app:v1.0

Our Dockerfile exposes port 8080, the port on which the application listens. But this is a private port and can’t be accessed externally.

The -p flag redirects a public port to a private port inside the container. The docker run command maps 49160 to port 8080 of the container. A request to 49160 will be routed to port 8080 on the container.

The -d flag runs the container in detached mode, leaving the container running in the background.

After executing the docker run command, a unique container ID will be displayed as follows:

> 543c207113d6ba6b207bc5e817c7b91d55a1f0098fbf3f2e6a8e3cce90727e73

<br/>

**Listing containers**
#

We can generate a list of containers, just as we could with images:

- #Execute the following command to list your running containers

    docker ps | grep node_web_app

- #The following command will list all containers, irrespective of status (e.g., running, stopped, or exited)

    docker ps -a

<br/>

**Accessing the application**
#

The container that hosts our Node.js application is now running.

Let’s try to access it:

- #Execute the following command to access the container app

    curl -i localhost:49160
It prints the response of the Node.js application, as below:

> HTTP/1.1 200 OK
> X-Powered-By: Express
> Content-Type: text/html; charset=utf-8
> Content-Length: 12
> ETag: W/"c-M6tWOb/Y57lesdjQuHeB1P/qTV0"
> Date: Wed, 19 Jun 2019 08:42:04 GMT
> Connection: keep-alive
>
> Hello world

<br/>

**Manage containers**
#

Let’s check out the various operations we can perform with containers.

**View inside container**

Just as we can navigate within a virtual machine's directory, we can also navigate within a container. There is a Docker command that allows us to navigate inside a container and view its contents, as if we were using a physical machine:

- #Execute the following command to store the container ID in a variable

    container_id=$(docker ps | grep "myrepo/node-web-app:v1.0" | awk '{print $1}')

- #Execute the following command to connect to the container

    docker exec -it $container_id /bin/bash
Once you connect to the container, execute the following command to see the working directory:

    pwd
You will see the following response:

> /usr/src/app

- #Execute the following command to list the files inside the working directory

    ls

You will see the following response:

> Dockerfile     licenses       package.json       server_v2.js
> Dockerfile_v2  node_modules   package-lock.json  server.js

The output above shows that all the application files are located in the working directory (/usr/src/app) that was set as the value for WORKDIR in the Dockerfile.

We can run commands inside the container to verify that the Node server process is running.

- #Execute the following command to exit the container shell

    exit

**Stop or start the container**

Containers can be stopped and started, just like applications.

The docker stop command stops the container:

- #Execute the following command to stop the "node_web_app" container

    docker stop node_web_app

The docker start command starts the container.

- #Execute the following command to start the "node_web_app container" after it has been stopped

    docker start node_web_app

The docker stop and docker start commands operate with both the container name and the container ID. The container ID can be obtained by listing the containers.

**Remove container**

The docker rm command removes the container (the container must be stopped before it can be removed):

- #Execute the following command to stop the "node_web_app" container

    docker stop node_web_app
- #Execute the following command to remove the stopped container

    docker rm node_web_app   

<br/>

****
**Lab- Build Container Image**
****

**Create a container**
#

As we've already discussed, a container is a runnable instance of an image. Let’s create a container out of the image "myrepo/node-web-app:1.0".

The docker run command can be used to create and run a container.

- #Execute the following command to run the container from the image "myrepo/node-web-app" and name the container "node_web_app"

    docker run --name node_web_app -p 49160:8080 -d myrepo/node-web-app:v1.0

Our Dockerfile exposes port 8080, the port on which the application listens. But this is a private port and can’t be accessed externally.

The -p flag redirects a public port to a private port inside the container. The docker run command maps 49160 to port 8080 of the container. A request to 49160 will be routed to port 8080 on the container.

The -d flag runs the container in detached mode, leaving the container running in the background.

After executing the docker run command, a unique container ID will be displayed as follows:

> 543c207113d6ba6b207bc5e817c7b91d55a1f0098fbf3f2e6a8e3cce90727e73

<br/>

**Listing containers**
#

We can generate a list of containers, just as we could with images:

- #Execute the following command to list your running containers

    docker ps | grep node_web_app
#The following command will list all containers, irrespective of status (e.g., running, stopped, or exited)

    docker ps -a


<br/>

**Accessing the application**
#

The container that hosts our Node.js application is now running.

Let’s try to access it:

- #Execute the following command to access the container app

    curl -i localhost:49160

It prints the response of the Node.js application, as below:

> HTTP/1.1 200 OK
> X-Powered-By: Express
> Content-Type: text/html; charset=utf-8
> Content-Length: 12
> ETag: W/"c-M6tWOb/Y57lesdjQuHeB1P/qTV0"
> Date: Wed, 19 Jun 2019 08:42:04 GMT
> Connection: keep-alive
>
> Hello world

<br/>

**Manage containers**
#

Let’s check out the various operations we can perform with containers.

View inside container
Just as we can navigate within a virtual machine's directory, we can also navigate within a container. There is a Docker command that allows us to navigate inside a container and view its contents, as if we were using a physical machine:

- #Execute the following command to store the container ID in a variable

    container_id=$(docker ps | grep "myrepo/node-web-app:v1.0" | awk '{print $1}')
- #Execute the following command to connect to the container

    docker exec -it $container_id /bin/bash

Once you connect to the container, execute the following command to see the working directory:

    pwd

You will see the following response:

> /usr/src/app

- #Execute the following command to list the files inside the working directory

ls
You will see the following response:

> Dockerfile     licenses       package.json       server_v2.js
> Dockerfile_v2  node_modules   package-lock.json  server.js

The output above shows that all the application files are located in the working directory (/usr/src/app) that was set as the value for WORKDIR in the Dockerfile.


We can run commands inside the container to verify that the Node server process is running.

- #Execute the following command to exit the container shell

    exit

**Stop or start the container**

Containers can be stopped and started, just like applications.

The docker stop command stops the container:

- #Execute the following command to stop the "node_web_app" container

    docker stop node_web_app

The docker start command starts the container.

- #Execute the following command to start the "node_web_app container" after it has been stopped

    docker start node_web_app

The docker stop and docker start commands operate with both the container name and the container ID. The container ID can be obtained by listing the containers.

**Remove container**

The docker rm command removes the container (the container must be stopped before it can be removed):

- #Execute the following command to stop the "node_web_app" container

    docker stop node_web_app
- #Execute the following command to remove the stopped container

    docker rm node_web_app


<br/>

****
**Lab- Updating an Existing Application Container**
****

**Update the Dockerfile to use ENV variables**
#

We have hardcoded port 8080 in the Dockerfile and the server.js file. Instead of hardcoding the port, we can use an ENV variable to hold its value and then access its value via variable.

- #Execute the following command to ensure that you are in correct directory

    cd $WORKING_DIR/nodejsapp/node-demo


Let's use the Dockerfile with ENV variable. Execute below command to use updated Dockerfile:

    mv Dockerfile_v2 Dockerfile

- #Execute the following command to see the contents of the updated Dockerfile

    cat Dockerfile

We added the ENV variable to hold the value "8080" and updated the EXPOSE command to use this ENV variable.

Logging is available at the container level. Let’s update our application to include some logging statements.

Please ensure you remove the container and image we created in the previous exercise. Use the instructions detailed above to remove our application container and application image.


<br/>

**Update the application code to use ENV variables and loggers**
#

Let’s alter the server.js file to use the ENV variable value and a few logger statements. Execute below command to use updated server.js file:

    mv server_v2.js server.js

#Execute the following command to see the contents of the updated server.js file

    cat server.js

<br/>

**Recreate the image from the Dockerfile**
#

- #Execute the following command (from the directory containing the Dockerfile) to build the image again

    docker build -t myrepo/node-web-app:v1.1 .

In the command above, we set the version of the image to "1.1".

- #Execute the following command to list the images and verify that the "myrepo/node-web-app:v1.1" image is listed

    docker images | grep myrepo/node-web-app

<br/>

**Create a container**
#

- #Execute the following command to run the container from the image "myrepo/node-web-app:v1.1" with a user-provided name

docker run --name node_web_app -p 49160:8080 -d myrepo/node-web-app:v1.1

- #Execute the following command to list the containers

    docker ps | grep node_web_app


- #Execute the following command to view the logs

    docker logs node_web_app

The docker logs command shows the output of the npm start command, as below, since this is the command we execute when starting a container:

> docker_web_app@1.0.0 start /usr/src/app
> node server.js
>
> Starting app
> Constructing response
> Running on http://0.0.0.0:8080


<br/>

****
**Lab- Application Container Lifecycle**
****

**Test the container**

- #Execute the following command to access the container app

    curl -i localhost:49160
You will see the output of the curl command, as below (note the difference in the "Hello world" statement):

> HTTP/1.1 200 OK
> X-Powered-By: Express
> Content-Type: text/html; charset=utf-8
> Content-Length: 23
> ETag: W/"17-HtEjRtPNxxTwRiO506FUqUtwFSI"
> Date: Wed, 19 Jun 2019 09:30:39 GMT
> Connection: keep-alive
>
> Hello world once again


**Remove the image
**
#

The docker rmi command is used to remove an image.

Please ensure you have stopped and removed the container before you remove the image.

- #Execute the following command to stop the "node_web_app" container

    docker stop node_web_app
- #Execute the following command to remove the "node_web_app" container

    docker rm node_web_app
- #Execute the following command to remove the image "myrepo/node-web-app"

    docker rmi myrepo/node-web-app:v1.1
As shown in the output below, the docker rmi command will untag the image, then delete the image from the server:

> Untagged: myrepo/node-web-app:v1.1
>
> Deleted: sha256:f95433cfe06dfbda9bf6491ccda4eafb32795a561512bebcef026a294ed5504f
>
> Deleted: sha256:049817d2ca5f62976a8d5df074c1339e29b5abe6a927fabc5eea0aff78feeee9
>
> Deleted: sha256:69159c6db26cdedafb656c722878f102b3d03dc64d212e6a8e84cd55f4ff20da
>
> Deleted: sha256:7cb87f91a35bfc59662f07afb334f6fb4ad7754fb8cada4d09f47cc64f97ef8b
>
> Deleted: sha256:bce2333ed3125665ef5a562bffcb47e6391af21dca14b642cf583ee2b96a746f
>
> Deleted: sha256:cdad01f16f90f864cd8e7c339742b016631df1371d89aa85b0596539d094e0c3
>
> Deleted: sha256:4e12909bcb47659749d8b35e6dfe612537e93c216f858db390194647b9f91b88

Images can also be removed using the image ID.

### Containerization Best Practices

**Use Red Hat Universal Base Images**

**What are Red Hat base images?**

Red Hat provides multiple base images that you can use as a starting point for your own images. These images are available through the Red Hat Registry (registry.access.redhat.com and registry.redhat.io) and described in the <a href="https://access.redhat.com/containers/?count=10#/category/Base%20Image"> Red Hat Container Catalog. </a>

***Red Hat Enterprise Linux (RHEL)*** base images are meant to form the foundation for the container images you build.
**Characteristics of RHEL base images include:**

+ **Supported:** Supported by Red Hat for use with your containerized applications. Contains the same secured tested, and certified software packages you have in Red Hat Enterprise Linux. 
+ **Cataloged:** Listed in the <a href="https://access.redhat.com/containers/"> Red Hat Container Catalog. </a>, where you can find descriptions, technical details, and a health index for each image.
+ **Updated:** Offered with a well-defined update schedule, so you know you are getting the latest software (see <a href="https://access.redhat.com/articles/2208321">Red Hat Container Image Updates</a>).
+ **Tracked:** Tracked by errata, to help you understand the changes that go into each update.
+ **Reusable:** Only need to be downloaded and cached in your production environment once, where each base image can be reused by all containers that include it as their foundation.


**RHEL 8:**

For RHEL 8, all Red Hat base images are available as new ***Universal Base Images (UBI)***. These include versions of RHEL standard, minimal, init, and Red Hat Software Collections that are all now freely available and redistributable.

Red Hat also provides a set of language runtime images, based on <a href="https://developers.redhat.com/blog/2018/11/15/rhel8-introducing-appstreams/">Application Streams</a>, that you can build on when you are creating containers for applications that require specific runtimes. Runtime images include python, php, ruby, nodejs, and others. All of the RHEL 8 images are UBI images, which means that you can freely obtain and redistribute them.

***Red Hat Universal Base Images (UBI)*** for RHEL 8 provide the same quality RHEL software for building container images as their non-UBI predecessors (<code>rhel6, rhel7, rhel-init,</code> and <code>rhel-minimal</code> base images), but offer more freedom in how they are used and distributed.


**Note:** For a list of available Red Hat UBI images, and associated information about UBI repositories and source code, see <a href="https://access.redhat.com/articles/4238681">Universal Base Images (UBI): Images, repositories, and packages.</a>

#

**RHEL 7:**

There is a set of RHEL 7 images as well that you can run on RHEL 8 systems. For RHEL 7, there are both UBI (redistributable) and non-UBI (require subscription access and are non-redistributable) base images. Those images include three regular base images (<code>rhel7, rhel-init</code>, and <code>rhel-minimal</code>) and three UBI images (ubi7, ubi7-init, and ubi7-minimal).

#

**Using standard Red Hat base images**

Standard RHEL 8 base images (<code>ubi8</code>) have a robust set of software features that include the following:

+ **init system:** All the features of the systemd initialization system you need to manage systemd services are available in the standard base images. These init systems let you install RPM packages that are pre-configured to start up services automatically, such as a Web server (httpd) or FTP server (vsftpd).
+ **yum:** Software needed to install software packages is included via the standard set of <code>yum</code> commands (<code>yum, yum-config-manager, yumdownloader</code>, and so on). For the UBI base images, you have access to free yum repositories for adding and updating software.
+ **utilities:** The standard base image includes some useful utilities for working inside the container. Utilities that are in this base image that are not in the minimal images include <code>tar, dmidecode, gzip, getfacl</code> (and other acl commands), <code>dmsetup</code> (and other device mapper commands), and others.   



**Using minimal Red Hat base images**

The <code>ubi8-minimal</code> images are stripped-down RHEL images to use when a bare-bones base image in desired. If you are looking for the smallest possible base image to use as part of the larger Red Hat ecosystem, you can start with these minimal images.

RHEL minimal images provide a base for your own container images that is less than half the size of the standard image, while still being able to draw on RHEL software repositories and maintain any compliance requirements your software has.

Here are some features of the minimal base images:

+ **Small size:** Minimal images are about 92M on disk and 32M compressed. This makes it less than half the size of the standard images.
    Software installation (microdnf): Instead of including the full-blown yum facility for working with software repositories and RPM software packages, the minimal images includes the microdnf utility. Microdnf is a scaled-down version of dnf. It includes only what is needed to enable and disable repositories, as well as install, remove, and update packages. It also has a clean option, to clean out cache after packages have been installed.
+ **Based on RHEL packaging:** Because minimal images incorporate regular RHEL software RPM packages, with a few features removed such as extra language files or documentation, you can continue to rely on RHEL repositories for building your images. This allows you to still maintain compliance requirements you have that are based on RHEL software. Features of minimal images make them perfect for trying out applications you want to run with RHEL, while carrying the smallest possible amount of overhead. What you don’t get with minimal images is an initialization and service management system (systemd or System V init), a Python run-time environment, and a bunch of common shell utilities.
+ **Modules for <code>microdnf</code> are not supported:** Modules used with the <code>dnf</code> command let you install multiple versions of the same software, when available. The <code>microdnf</code> utility included with minimal images does not support modules. So if modules are required, you should use a non-minimal base images, which include yum.

If your goal, however, is just to try to run some simple binaries or pre-packaged software that doesn’t have a lot of requirements from the operating system, the minimal images might suit your needs. If your application does have dependencies on other software from RHEL, you can simply use microdnf to install the needed packages at build time.

Red Hat intends for you to always use the latest version of the minimal images, which is implied by simply requesting <code>ubi8/ubi-minimal</code> or <code>ubi8-minimal</code>. Red Hat does not expect to support older versions of minimal images going forward.

#

**Using Init Red Hat base images**

The UBI ubi8-init images contains the systemd initialization system, making them useful for building images in which you want to run systemd services, such as a web server or file server. The Init image contents are less than what you get with the standard images, but more than what is in the minimal images.

**Note:** Because the <code>ubi8-init</code> image builds on top of the ubi8 image, their contents are mostly the same. There are a few critical differences, however. In <code>ubi8-init</code>, the Cmd is set to <code>/sbin/init</code>, instead of <code>bash</code>, to start the systemd Init service by default. It includes <code>ps</code> and process related commands (procps-ng package), which <code>ubi8</code> does not. Also, <code>ubi8-init</code> sets SIGRTMIN+3 as the StopSignal, as systemd in ubi8-init ignores normal signals to exit (SIGTERM and SIGKILL), but will terminate if it receives SIGRTMIN+3.

Historically, Red Hat Enterprise Linux base container images were designed for Red Hat customers to run enterprise applications, but were not free to redistribute. This can create challenges for some organizations that need to redistribute their applications. That’s where the Red Hat Universal Base Images come in.


### What You Learned


**Summary**

#

**What you learned in this course**

This course provided a refresher on Containers and how to create them using Docker, which includes:
- What is Docker

- What are Docker Images

- Docker Repositories

- What is a Dockerfile

- How to create a Docker Container

- Best practices while working with Containers

![Docker summary](_images/docker-summary.png  "Docker summary")