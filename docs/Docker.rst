########
Docker
########

1. Introduction to Docker
---------------------------

Docker is an open source software platform to create, deploy and manage virtualized application containers on a common operating system
(OS), with an ecosystem of allied tools. Docker Inc., the company that originally developed Docker, supports a commercial edition and is 
the principal sponsor of the open source tool.

Docker is a tool that packages, provisions and runs containers independent of the OS. Container technology is available through the 
operating system: A container packages the application service or function with all of the libraries, configuration files, dependencies 
and other necessary parts to operate. Each container shares the services of one underlying operating system.

Docker was created to work on the Linux platform, but has extended to offer greater support for non-Linux operating systems, including 
Microsoft Windows and Apple OS X. Versions of Docker for Amazon Web Services (AWS) and Microsoft Azure are available.


2. Docker Architecture

Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running,
and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to 
a remote Docker daemon.

.. image:: architecture.PNG
   :width: 600px
   :height: 400px
   :alt: alternate text
