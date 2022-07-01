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

```dockerfile
FROM node:alpine   
COPY . /app  
WORKDIR /app  
CMD node app.js 
```

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
```
Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
```

The image is not stored in this working directory. Docker manages its images itself
without reveling them to the outside.  

We can use `docker images` command on terminal to see all the images on the machine.  
```
C:\Projects\Docker\docker training\docker-training\hello-docker>docker images
REPOSITORY     TAG       IMAGE ID       CREATED         SIZE
hello-docker   latest    99546e5fe110   6 minutes ago   172MB
```

Also, `docker image ls` command can be used for the same.
```
C:\Projects\Docker\docker training\docker-training\hello-docker>docker image ls
REPOSITORY     TAG       IMAGE ID       CREATED         SIZE
hello-docker   latest    99546e5fe110   9 minutes ago   172MB
```

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
```
C:\Projects\Docker\docker training\docker-training\hello-docker>docker run hello-docker
Hello Docker!
```

Now, if we want, we can publish this image on DockerHub - so, anyone can use this
image. For testing - or production all we have to do is to pull and run this image.  

# DockerHub and PlayWithDocker

After we publish our image to DockerHub, we can pull it and run it anywhere.  
[Play-with-Docker](http://labs.play-with-docker.com) is a platform we can use to run 
or test our Docker containers online.  
* Sign in with our **Docker ID**.  
* Click on **Start** button to start a lab.  
* Click on **New Instance** button to start a new VM.  

We will get a blank machine, with a Linux OS and Docker. It doesn't even have 
Node installed. But because it has Docker, we can just pull an image from 
DockerHub and run it.  

![](https://github.com/dilshankarunarathne/docker-training/raw/main/ss.png)

We can use `docker pull <username>/<image name>` command to pull an image 
from DockerHub.  
After that, if we list all the available images, we will be able to see the 
image we just pulled.  
We can run it with `docker run` command as usual.  

# Linux 

Docker is built on basic Linux concepts. We must be familiar with Linux commands to easily use Docker.  

[Ubuntu - DockerHub](https://hub.docker.com/_/ubuntu): This image is built from official rootfs tarballs provided by Canonical.   
The ubuntu:latest tag points to the "latest LTS", since that's the version recommended for general use. The ubuntu:rolling tag points to the latest release (regardless of LTS status).  

We can pull this image into our machine by 
```docker
docker pull ubuntu
```
But we can also use the 
```docker
docker run ubuntu 
```
command to run the image. If we have this image locally, Docker will start a container with that image. If the image is not available, it will automatically pull it from DockerHub.  

When we use the `docker run ubuntu` command, it automatically creates a container with the pulled (or existed) image. But because we never interacted with the container, it will stop after the image is downloaded and installed.  
`docker ps` command will show all the running processes of docker. But if we use the `docker ps -a` command (**-a**for **all**), we will be able to see the newly added ubuntu image on the images list.  

To start a container and interact with it, we can use the command `docker run -it ubuntu` (**-it** for **interactive mode**).  
```cmd
C:\Users\Dilshan Karunarathne>docker run -it ubuntu
root@8c915e73ee0f:/#
```
We will get a **shell** from this command. The **shell** will execute the commands in the container.  
`root@8c915e73ee0f:/#` is the **shell prompt**.  
`root` is the user account we're logged as. So, by default - we log in as the *root* user, which has the highest privilages.  
`8c915e73ee0f` is the name of the machine. This container has an automatically generated id.  
And after the `:` we have a `/` character. This is the **working directory** of the container. The forward slash represents the root directory of the container.  
The `#` means that we have the highest privilages. If we logged in as a non-root user, we'd see a `$` sign.  

## Package Manager 

Most of the softwares and development tools provides a package manager. For example: yarn, npm, pip, maven, gradle, etc.  
Ubuntu has the **apt** package manager, which stands for *Advanced Package Tool*.  
**apt** is the newer version of the **apt-get** package manager. **apt** is easier to use than **apt-get**.  

We can install a package using *apt* by the command `apt install <package name>`.  
Linux has a package database that contains all the packages. But not all these packages are installed in our machine. We can use the command `apt list` to list all the packages installed in our machine. Some of those packages might have **installed** after the name. And others are not installed.  
If the package we want to install does not exist in the package database, we need to update the packages list with the `apt update` command. We should do this often when we install packages.  
We can remove a package with the `apt remove <package name>` command.  

## File System 

In Linux, the root directory `/` is the top of the file system. It contains the following directories:
* /bin  - binaries for programs 
* /boot - boot loader, files related to booting 
* /dev  - devices
* /etc  - configuration files (editable text configuration)
* /home - user home directories, each user has a home directory here 
* /root - root user's home directory
* /lib  - libraries, software dependencies 
* /var  - variables
* /proc - running process information 
* And also: /lib64, /media, /mnt, /opt, /sbin, /srv, /sys, /tmp, /usr 

In Linux, everything is a file, including devices, **directories**, network sockets, pipes, **running processes** and etc.  
Files that are used for accessing devices are stored in the `/dev` directory.  
The files that are updated frequently are stored in the `/var` directory, like logfiles and app data.  

## Commands 

* There is a command `pwd` that can be used to **print** the **working directory**. 
* The `ls` command can be used to **list** the files in the working directory.  
  If we want to list items line by line, we can use the `ls -1` command.  
  The `ls -l` command can be used to **list** the files in the working directory with more information.  
  The `ls -a` command can be used to **list** the files in the working directory with all the hidden files.  
  The `ls -lR` command can be used to **list** the files in the working directory with more information and subdirectories. 
* `cd` command can be used to **change** the **working directory**.  
  `cd ..` will change the working directory to the parent directory.  
  `cd /` will change the working directory to the root directory.  
  `cd /home` will change the working directory to the home directory of the user.  
* `mkdir` command can be used to **create** a **directory**. 
* To get to the current user's home directory we can use `cd ~` command. 
* To create a new file, we can use the `touch` command. Using this command, we can create multiple files at once. 
* We can use the `mv <source-file-path> <dest-file-path>` command to **move** a file. 
* `cp <source-file-path> <dest-file-path>` command can be used to **copy** a file. 
* `rm <file-path>` command can be used to **remove** a file.  
  `rm -r <directory-path>` command can be used to **remove** a directory. (**r** stands for **recursive**)  
  `rm -rf <directory-path>` command can be used to **remove** a directory and all its subdirectories. 
* `find <directory-path>` command can be used to **find** files in a directory.  
  To find only the directories, we can use the `find -type d` command.  
  To filter by file name, we can use the `find -name <file-name>` command. For the <file-name> argument, we can use the wildcard characters `*` and `?`.  
  This is case sensitive by default. To make it case insensitive, we can use the `find -iname <file-name>` command. 
* `grep <pattern> <file-path>` command can be used to **search** for a pattern in a file.  
  `grep -r <pattern> <directory-path>` command can be used to **search** for a pattern in all the files in a directory. 
* `head <file-path>` command can be used to **print** the first 10 lines of a file.  
  `tail <file-path>` command can be used to **print** the last 10 lines of a file. 
* `cat <file-path>` command can be used to **print** the contents of a file.  
  This command is useful if the content of our file is short (fits on a single page). 
* `more <file-path>` command can be used to **print** the contents of a file in more readable format. With this, we can only scroll down to see the content. To scroll up and down, we could use:   
  `less <file-path>` command can be used to **print** the contents of a file in more readable format. This is a newer command that is supposed to replace `more`. We might have to manually install it. 
* `wc <file-path>` command can be used to **count** the lines, words and characters in a file. 

## Standard I/O 

In Linux, standard input represents the keyboard, and standard output is the screen. But we can always change the source of thes inputs or outputs. This is called **redirection**.  

If we read the content of a file, let's say `file1.txt` - it will automatically be printed out to the console because that is the standard output.  
But we can use the redirection operator `>` to output it to something else. For example:  
`cat file1.txt > file2.txt` will output the content of `file1.txt` to `file2.txt`. 

The `cat` command can also be used to **concatenate** two files. For example:  
`cat file1.txt file2.txt > file3.txt` will concatenate the content of `file1.txt` and `file2.txt` and output it to `file3.txt`.  
If we hadn't used the redirection, content of both files will be printed out to the console. 

The redirection operator can be used pretty much anywhere. For example, if we have just a single line to write into a file, we don't need to use `nano`, we could just redirect the `echo` command.  
`echo "Hello World" > file1.txt` will output `Hello World` to `file1.txt`. 

We can list all the content of a directory and instead of printing it, we could redirect the output to a file.  
`ls -l /etc > etc_dir.txt` will output the content of the `/etc` directory to `etc_dir.txt`. 

In the same way, there is a redirection operator to redirect the standard input `<`. 

## Chaining Commands 

We can chain multiple commands, by seperating each with a semi-colon `;`.  
For example: `pwd ; ls -l ; cd /home ; ls -l`. This command will print the working directory, list the files in the working directory, change the working directory to the home directory and list the files in the home directory.  

By chaining commands with semi-colons, if a command fails, the next command will still be executed. This can have bad side effects.  
To make sure - if a command fails, the subsequent commands will not execute, we can chain commands with the and operator `&&`.  
For example: `pwd && ls -l && cd /home && ls -l`. This command will print the working directory, list the files in the working directory, change the working directory to the home directory and list the files in the home directory. 
If the first command fails, the second command will not execute.  
There is also an OR operator `||` that can be used to chain commands.  
For example: `pwd || ls -l || cd /home || ls -l`. This command will print the working directory, list the files in the working directory, change the working directory to the home directory and list the files in the home directory. If the first command fails, the second command will execute.  

There is another technique of **piping** where we can use the `|` operator to chain commands. It executes the first command and sends its output to the second command.  
For example: `pwd | ls -l | cd /home | ls -l`. This command will print the working directory, list the files in the working directory, change the working directory to the home directory and list the files in the home directory. 

When we have chained really long commands, we can make things a bit clear to read by splitting them into lines. When we press *enter* after a backslash `\`, the command will not be immediately executed. 
For example: 
```bash
root@2f5656e1s65:~#pwd ;\ 
> ls -l ;\ 
> cd /home ;\ 
> ls -l
```
This command will print the working directory, list the files in the working directory, change the working directory to the home directory and list the files in the home directory. 

## Environment Variables 

* `export <variable-name>=<value>` command can be used to **set** an environment variable.  
* `unset <variable-name>` command can be used to **unset** an environment variable.  
* `echo $<variable-name>` command can be used to **print** the value of an environment variable.  
* `echo $$` command can be used to **print** the process ID of the current process.  
* `echo $PATH` command can be used to **print** the path of the current process.  
* `echo $HOME` command can be used to **print** the home directory of the current process.  
* `echo $USER` command can be used to **print** the user name of the current process.  
* `echo $SHELL` command can be used to **print** the shell of the current process.  
* `echo $HOSTNAME` command can be used to **print** the host name of the current process.  
* `echo $HOSTTYPE` command can be used to **print** the host type of the current process.  
* `echo $LANG` command can be used to **print** the language of the current process.  
* `echo $LC_ALL` command can be used to **print** the language of the current process.  
* `echo $LC_CTYPE` command can be used to **print** the language of the current process.  
* `echo $LC_NUMERIC` command can be used to **print** the language of the current process.  
* `echo $LC_TIME` command can be used to **print** the language of the current process.  
* `echo $LC_COLLATE` command can be used to **print** the language of the current process.  
* `echo $LC_MONETARY` command can be used to **print** the language of the current process.  
* `echo $LC_MESSAGES` command can be used to **print** the language of the current process.  
* `echo $LC_PAPER` command can be used to **print** the language of the current process.



