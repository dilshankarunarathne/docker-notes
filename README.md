# Docker 

A platform for building, running and shipping apllications on a consistant manner.

Have you ever been in a situation where, your application works on your local machine, but fails on another...?
Reasons:
	* One or more files are missing in deployment
	* Software version mismatch on the target machine
	* Different configuration settings & environment variables 

With Docker, you can easily package your applications with anything they need - and run it anywhere.  
Say your application needs Node 14, MongoDB 4. You can package your application along with these. And that application package will run anywhere. If it works fine on your local machine, it will also work fine in deployment. 

Another advantage of using Docker:
Say, you have another team member that works on the same project. It can take hours just to get his machine to the same configuration and install all dependancies so he can work alongside you.   
We can simply tell Docker to bring up your application and Docker will automatically download all the dependancies inside an isolated environment called a **container**.  
These containers allow multiple applications to use different versions of software dependancies - side by side. 

Also, when we are done with an application, we can remove it - along with all of its dependancies using Docker 