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
