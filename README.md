# Docker 

A platform for building, running and shipping applications on a consistent manner.  

Have you ever been in a situation where, your application works on your local machine, but fails on another...?
Reasons:
+ One or more files are missing in deployment
+ Software version mismatch on the target machine
+ Different configuration settings & environment variables 

With Docker, you can easily package your applications with anything they need - and run it anywhere.  
Say your application needs Node 14, MongoDB 4. You can package your application along with these. And that application package will run anywhere. If it works fine on your local machine, it will also work fine in deployment.  

Another advantage of using Docker:  
Say, you have another team member that works on the same project. It can take hours just to get his machine to the same configuration and install all dependencies, so he can work alongside you.   
We can simply tell Docker to bring up your application and Docker will automatically download all the dependencies inside an isolated environment called a **container**.  
These containers allow multiple applications to use different versions of software dependencies - side by side.  

Also, when we are done with an application, we can remove it - along with all of its dependencies using Docker easily. We don't have to worry about removing dependencies, because we were using a container.  

# Container 

A container is an isolated environment for running an application.  

What is the difference between a Virtual Machine and a container ?  
A VM - is an abstraction of a machine (physical hardware).  

Hypervisors are software that are used to simulate computer environments.  
(Eg: VirtualBox, VM-Ware, Hyper-V)  

The benefit of VMs for a developer is that we can run our applications in isolation, inside these virtual machines.  

Problems with VMs:
- Each VM needs a full-blown OS 
- Slow to start
- Resource intensive 

Benefits of Containers over VMs:
* Allow running multiple apps in isolation  
  Just like VMs
* Much more lightweight 
* All containers on a single machine shares the same OS of the host
* Starts quickly
* Needless hardware resources 

# Architecture of Docker

Docker uses a Client-Server architecture. It has client components that talks to server components using a Restful API.   
The server is also called the **Docker Engine**. It works on the background and takes care of building and running Docker containers.  
Technically, it's a process that runs on the OS. 

Kernel of an operating system is the part that manage all applications as well 
as the hardware resources like memory and CPU.  

Every operating system has its own kernel.  
On a Linux machine we can only run Linux containers, because these containers need Linux.    
On a Windows machine however, we can run both Windows and Linux containers. Because 
since Windows 10, a custom-built Linux kernel ships with Windows. So, with the 
Windows kernel that's always been on Windows, and the new Linux kernel, we can run
both Windows and Linux applications - natively on Windows.  
macOS has its own kernel. But it doesn't support the use of containers. So Docker 
on macOS, uses a light-weight Linux kernel.  

## Installing Docker

We can visit [docs.docker.com/get-docker](https://docs.docker.com/get-docker/) to download and install docker on any platform.  
At the time I'm typing this note, there is a Docker Desktop application for 
Windows and macOS. 

## Development Workflow

We need to Dockerize an application, so it can be run by Docker. This is done 
by adding a Dockerfile to it.  
A Dockerfile is a plain text file that includes instructions that Docker uses 
to package up this application into an image.  
This image contains everything that the application needs to run. Such as
* A cut-down operating system 
* A runtime environment 
* Application files 
* Third-party libraries 
* Environment variables

Once we have an image, we can tell Docker to start a container using that image.  
A container is a process, which has its own file system, which is provided by 
the image. So, the application loads inside the container, and that is how we 
run applications in Docker, instead of running them directly from our machine.

Once we have an image, we can push it to a **Docker registry** on **Docker Hub**.  
_**DockerHub to Docker is like GitHub to Git.**_  
It is a storage for Docker images that anyone can use.  
Once we push our image to DockerHub, then we can use it on any machine running Docker.  

All the instructions for building an image of an application are written in a
Dockerfile. With that, we can package our application into an image.  

## Writing the Dockerfile

Let's Dockerize a 'hello world' JavaScript code.  
First, we can create an `app.js` in the hello-docker directory as below.  
`console.log("Hello Docker!");`  
Then we need to create the Dockerfile. In the same director 
<pre><code> 
FROM node:alpine   
COPY . /app  
WORKDIR /app  
CMD node app.js  
</code></pre>

`FROM` keyword specifies the base image. We can either start from a linux image 
and install node on top of it. `FROM linux/node` or we can start with a node image.  
This node image is already built on a linux image.  
And these images are officially published on [DockerHub](http://hub.docker.com/search?q=node&type=image).  
We also need to specify the version of the image we're using. With a colon (**:**) we 
can specify any version we'd like. For this example, we'll use **alpine**. It is
a lightweight linux distribution.  

Then we need to specify our program files (application files). We can use the `COPY`
instruction to do that.  
With `COPY . /app`, we are specifying that, we need to  
`COPY` &nbsp; copy  
`.` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; all the files in the current directory,  
`/app` &nbsp; to the _/app_ directory **in the image's** file system.  

Then we can use the command `CMD` instruction to execute a command 
(to run the application).  
We need to invoke Node.js for this. So, it should be `node /app/app.js`.  

With the `WORKDIR` command, we can specify a working directory. So, after that, 
any command will execute in the working directory.  
So, now we can just use  
`CMD node app.js`

## Dockerizing and running Docker images

To build the image, we can use the `docker build` command.  
`-t` can be used to add a tag to identify the image.  
We also need to give a name for the image.  
Also, we need to specify - where to find the Dockerfile.  

`docker build -t hello-docker .`  

`-t` adds a default tag by Docker.  
`hello-docker` is the image name.  
`.` indicates the current directory to find the Dockerfile.  

After the image is built this message will be displayed.  
<pre><code>
Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
</code></pre>

The image is not stored in this working directory. Docker manages its images itself
without reveling them to the outside.  

We can use `docker images` command on terminal to see all the images on the machine.  
<pre><code>
C:\Projects\Docker\docker training\docker-training\hello-docker>docker images
REPOSITORY     TAG       IMAGE ID       CREATED         SIZE
hello-docker   latest    99546e5fe110   6 minutes ago   172MB
</code></pre>

Also, `docker image ls` command can be used for the same.
<pre><code>
C:\Projects\Docker\docker training\docker-training\hello-docker>docker image ls
REPOSITORY     TAG       IMAGE ID       CREATED         SIZE
hello-docker   latest    99546e5fe110   9 minutes ago   172MB
</code></pre>

So, we have a repository called `hello-docker`, and it has a tag called latest. 
This is a tag automatically added by Docker. These tags are used for versioning 
the images.  
Each image also has a unique identifier.  
The creation time and the size of the image is also displayed. Because we used 
Node on linux-alpine, we've ended up having 112 megabytes of data in our image.  
So, this image contains 
* Alpine Linux
* Node
* Application files  

If we used a different Node image, that's based on a different version of Linux 
we would get a different size for our image. 

To run this image, we can just use the command `docker run <image-name>`.  
<pre><code>
C:\Projects\Docker\docker training\docker-training\hello-docker>docker run hello-docker
Hello Docker!
</code></pre>

Now, if we want, we can publish this image on DockerHub - so, anyone can use this
image. For testing - or production all we have to do is to pull and run this image.  

# DockerHub and PlayWithDocker

After we publish our image to DockerHub, we can pull it and run it anywhere.  
[Play-with-Docker](labs.play-with-docker.com) is a platform we can use to run 
or test our Docker containers online.  
* Sign in with our **Docker ID**.  
* Click on **Start** button to start a lab.  
* Click on **New Instance** button to start a new VM.  

We will get a blank machine, with a Linux OS and Docker. It doesn't even have 
Node installed. But because it has Docker, we can just pull an image from 
DockerHub and run it.  

<pre><code>
outputs of
docker version
on play with docker
</code></pre>

We can use `docker pull <username>/<image name>` command to pull an image 
from DockerHub. 