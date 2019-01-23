###########
Dockerfile
###########

What is Dockerfile?
--------------------

A Dockerfile is a text file that defines a Docker image. You’ll use a Dockerfile to create your own custom Docker image, in other words to
define your custom environment to be used in a Docker container.

Why Dockerfile?
----------------

You’ll want to create your own Dockerfile when existing images don’t satisfy your project needs. This will actually happen most of the 
time, which means that learning about the Dockerfile is a pretty essential part of working with Docker.

The Dockerfile contains a list of instructions that Docker will execute when you issue the docker build command. Your workflow is like this:

- you create the Dockerfile and define the steps that build up your images
- you issue the docker build command which will build a Docker image from your Dockerfile
- now you can use this image to start containers with the docker run command

Create image with **Dockerfile**
---------------------------------

1) Create Dockerfile
^^^^^^^^^^^^^^^^^^^^

Create an empty directory for this task and create an empty file in that directory with the name Dockerfile. You can do this easily by 
issuing the command touch Dockerfile in your empty directory.

2) Define the base image with FROM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Every Dockerfile must start with the FROM instruction. The idea behind is that you need a starting point to build your image. You can 
start FROM scratch,scratch is an explicitly empty image on the Docker store that is used to build base images like Alpine, Debian and so on.
The image you start from is called the base image. In our case let’s add FROM centos to the Dockerfile.

Right now your Dockerfile should look like this:

.. code-block:: bash

    FROM centos
    
3) Add the lines to install packages
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Please add the lines to install vim and curl like this:

.. code-block:: bash

   FROM centos
   
   RUN yum update
   
   RUN yum install vim
   
   RUN yum install curl
   
4) Build your image
^^^^^^^^^^^^^^^^^^^^

Go the location of file and run following command

.. code-block:: bash

   $ docker build .
   
.. image:: dfile1.png
   :width: 800px
   :length: 400px
   :alt: alternate text
   
.. image:: dfile2.png
   :width: 800px
   :length: 400px
   :alt: alternate text
   
.. image:: dfile3.png
   :width: 800px
   :length: 400px
   :alt: alternate text
   
5) List your images
^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   $ docker images

.. image:: dfile4.png
   :width: 800px
   :length: 200px
   :alt: alternate text   
 
