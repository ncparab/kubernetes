###############
Virtualization
###############

Introduction to Virtualization
-------------------------------

Virtualization – “In computing, Virtualization refers to the act of creating a virtual(rather than actual) version of something, including
virtual computer platforms, storage devices and computer network resources.” – Wikipedia

.. image:: virtualization.png
   :width: 600px
   :height: 400px
   :alt: alternate text
   
Virtualization for example allows you to create environments  that let’s you deploy your application in an encapsulated environment  so called virtual machine, that consists of all the resources that required by application to run along with Operating system that is completely abstracted from that of the physical server’s and further you can create many of these Virtual machines and deploy your applications.


Need for Virtualization 
""""""""""""""""""""""""

- Improves availability - makes the application available within minutes to hours with existing Data Center resources
- Automatic load balancing
- Virtualization lowers costs and improves efficiency of the physical hardware.
- Data protection
- Disaster recovery
- Reduces energy consumption and maintenance
- Increase Business Agility

Forms of Virtualization
""""""""""""""""""""""""

- Server Virtualization leverages storage virtualization and Network virtualization
- Storage virtualization
- Network virtualization(NW function virtualization)
- Desktop virtualization
- I/O virtualization – multiple fibre channel connections with single cable 
- Application virtualization -  multiple version of application

Hypervisor 
"""""""""""

“A hypervisor or Virtual Machine monitor is computer software, firmware or hardware that creates and runs virtual machines” – Wikipedia 
Simply said, hypervisor is a piece of software that makes server virtualization possible, that is responsible for creating and running virtual machines on top of the virtualization layer. It is responsible for providing the resources required by the virtual machine . These resources are such as Virtual CPU, virtual memory, virtual Disk and virtual Network etc.

Hypervisors have optimization techniques for if you overcommit the resources(of course you can) such as cpu and memory to virtual machines than you have on physical resources.

Virtual machine
"""""""""""""""""

A virtual machine is an encapsulated environment with operating systems that are running on top of  virtualization layer and inside VM we have a guest operating system isolated form underlying physical server. In other words, the implementation of a VM is virtualizing an hardware layer with a hypervisor where the resources are isolated from physical hardware.The VMs treat themselves as a whole server without knowing that its sharing resources such as virtual cpu, memory, networking and storage with another VM. These are resources are provisioned through virtualization layer. These VMs are portable and are hardware independent.

What exactly is server virtualization?
"""""""""""""""""""""""""""""""""""""""

Running an operating system and application in an encapsulated virtual machine on top of virtualization layer. This operating system is abstracted from the physical servers. These encapsulated VM’s are virtualized servers.

Complexities of Virtualization
"""""""""""""""""""""""""""""""

- Greater demands on all resources(performance bottlenecks)
- Blending of storage IO – Blender effect
- Complexity in troubleshooting – performance issues – you have to install full operating system to launch an application.



