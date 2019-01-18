#######################
All About Kubernetes
#######################

1. Container Orchestration
---------------------------

1.1 Quick Introduction
^^^^^^^^^^^^^^^^^^^^^^^^

Containers are revolutionizing the way we deliver IT applications. Containers have had enormous impact on the way the organizations
manage IT infrastructure. Containerized applications unlike virtual machines does not need an Operating System to be in place. 
Containerization allowed applications to be packaged with language runtime, libraries and source code and deployed as a whole. 
This isolates the resources that the applications consume.

1.2 What is Container Orchestration? 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

“Orchestration” refers to processes used to manage these containers and automate the management of these containers. 
 
If you imagine a traditional organization’s IT Data center, it consists of physical servers that ranges from 1 to few hundreds and 
thousands of them. Just like if a physical machine is failed or need to be configured in terms of memory , storage and networking  or 
software patching or least a restart is required in some cases ,necessary steps are taken for the server to be function as is. These steps
may be performed manually in the datacenter or remotely. Now you have thousands of our servers that you need to take care of. Though all 
these aspects comes under datacenter management, the same goes to container except the fact that containers are light-weight VMs without
operating system. Imagine if your server resources are virtual, though with respect to virtualization, with container technology You don’t 
have to spin up a new VM and configure for it to run the application. You don’t have to do OS related stuff – no need of CAPEX, OPEX, 
admin, patching, upgrading, driver support, security etc., Containers are essentially a process in operating system of the physical server.
But if there are multiple physical servers that host containers, and if there are tens of hundreds of containers in each and every node, 
you need a system that manages your containers time-to-time.

Simply said - Container Orchestration refers to the automated arrangement, coordination, and management of software containers.


