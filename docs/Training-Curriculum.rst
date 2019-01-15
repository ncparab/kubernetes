############
Curriculum
############

Week - I
--------

+---------+-------------------------------------------------------------+-------------+
|**Days** |                    **Topics**                               |**Durartion**|
+---------+-------------------------------------------------------------+-------------+
| Day 1   | Introduction to of Virtualization/Paravirtulization         | 1.5 Hrs     |
+         +-------------------------------------------------------------+             +
|         | Introduction to Containers                                  |             |
+         +-------------------------------------------------------------+             +
|         | Virtualizion vs Containerized Architecture                  |             |
+         +-------------------------------------------------------------+             +
|         | Container Concepts                                          |             |
+         +-------------------------------------------------------------+             |
|         | Centralized containerization vs Distributed containerization|             |
+---------+-------------------------------------------------------------+-------------+  
| Day 2   | Docker Introduction                                         | 1 Hrs       |
+         +-------------------------------------------------------------+             +
|         | Docker Architecture                                         |             |
+         +-------------------------------------------------------------+             +
|         | Docker Components                                           |             |
+         +-------------------------------------------------------------+             +
|         | Docker Images                                               |             |
+         +-------------------------------------------------------------+             +
|         | Docker Containers                                           |             |
+         +-------------------------------------------------------------+             +
|         | Docker registry                                             |             | 
+         +-------------------------------------------------------------+             +
|         | Docker installation                                         |             |
+---------+-------------------------------------------------------------+-------------+ 
| Day 3   | Docker Commands - I                                         | 1.5 Hrs     |
+         +                                                             +             +
|         | 1) docker Search                                            |             |
+         +                                                             +             +
|         | 2) docker Pull                                              |             |
+         +                                                             +             +
|         | 3) docker images                                            |             |
+         +                                                             +             +
|         | 4) docker tag                                               |             |
+         +                                                             +             +
|         | 5) docker run                                               |             |
+         +                                                             +             +
|         | 6) docker exec                                              |             |
+         +                                                             +             +
|         | 7) docker attach                                            |             |
+         +                                                             +             +
|         | 8) docker detach                                            |             |
+         +                                                             +             +
|         | 9) docker commit                                            |             | 
+---------+-------------------------------------------------------------+-------------+
| Day 4   | Docker Commands - II                                        | 1.5 Hrs     |
+         +                                                             +             +
|         | 1) docker volume                                            |             |
+         +                                                             +             +
|         | 2) docker inspect                                           |             |
+         +                                                             +             +
|         | 3) docker history                                           |             |
+         +                                                             +             +
|         | 4) docker save                                              |             |
+         +                                                             +             +
|         | 5) docker load                                              |             |
+         +                                                             +             +
|         | 6) docker import                                            |             |
+         +                                                             +             +
|         | 7) docker export                                            |             |
+         +                                                             +             +
|         | 8) docker ps                                                |             |
+         +                                                             +             +
|         | 9) docker push                                              |             |
+         +                                                             +             +
|         | 10) docker pull                                             |             |
+         +                                                             +             +
|         | 11) docker rm                                               |             |
+         +                                                             +             +
|         | 12) docker rmi                                              |             |
+         +                                                             +             +
|         | 13) docker prune                                            |             |
+         +                                                             +             +
|         | 14) docker start/stop                                       |             |
+         +                                                             +             +
|         | 15) docker stats                                            |             |
+         +                                                             +             +
|         | 16) docker log                                              |             |
+         +                                                             +             +
|         | 17) docker diff                                             |             |
+         +                                                             +             +
|         | 18) docker network                                          |             |
+---------+-------------------------------------------------------------+-------------+
| Day 5   | Docker networking                                           | 1 Hrs       |
+         +-------------------------------------------------------------+             +
|         | Docker parameter                                            |             |
+         +-------------------------------------------------------------+             +
|         | Automating docker deployment                                |             |
+         +-------------------------------------------------------------+             +
|         | Docker security setup                                       |             |
+         +-------------------------------------------------------------+             +
|         | Docker registry setup                                       |             |
+---------+-------------------------------------------------------------+-------------+
| Day 6   | Demo - Trusted relationship setup                           | 1 Hrs       |
+         +-------------------------------------------------------------+             +
|         | Dockerfile                                                  |             |
+         +-------------------------------------------------------------+             +
|         | Demo - Microservice flask                                   |             |
+         +-------------------------------------------------------------+             +
|         | Airflow Setup demo with Rabbit MQ and postgresql            |             |
+---------+-------------------------------------------------------------+-------------+
| Day 7   | Docker lab exercise                                         | 2 Hrs       |
+         +                                                             +             +
|         | 1) Setup docker automic component environment               |             |
+         +                                                             +             +
|         | 2) Setup docker combined component environement             |             |
+         +                                                             +             +
|         | 3) Setup distributed docker environment                     |             |
+         +                                                             +             +
|         | 4) Setup haterogenous microservices (flask vs node)         |             |
+         +    communication.                                           +             +
|         |                                                             |             |
+---------+-------------------------------------------------------------+-------------+

Week - II
----------

1.History of Orch tools  
========================

2.Kubernetes architecture
===========================

- Minukube arch
- Self hosted arch
- Kops
- Kubadm
- Aks
- Eks

3.Kube componenets
===================

- Control plance
- Kube api server
- Kuber controller manager
- Kube scheduler
- Etcd
- Load balancer
- Worker plane 
- Kubelet
- Kube proxy

4.Kubernetes concepts:
=======================

- Pods 
- Namepspaces
- Rs, rc
- Deployments
- Services
- Labels
- Volumes
- Hpa
- Daemonsets

5.Kube cli – 
=============

- Kubectl
- kops


6.Kube setup for developers
===========================

- Dev Mode

   i) Minikube installation
   
      a) Establishing architecture
     
      b) Developing a microservice
     
      c) Deploying a microservice
     
      d) Exposing a microservice
     
      e) Accessing microservice internally and externally
     
      f) Establishing High availability 
     
   ii) Performance testing on kube cluster
   
       a) Smoke testing &Performance testing High availability
     
- Prod mode

   i) Self hosted – single master multi node
   
      a) Establishing architecture
       
      b) Developing a microservice
       
      c) Deploying a microservice
       
      d) Exposing a microservice

      e) Accessing microservice internally and externally

      f) Establishing High availability 
      
      g) Smoke testing & Performance testing High availability
       
   ii) Prod mode - multi master, multi node(on premise)

       a) Establishing architecture

       b) Developing a microservice

       c) Deploying a microservice
    
       d) Exposing a microservice
       
       e) Accessing microservice internally and externally

       f) Establishing High availability 
       
       g) Smoke testing & Performance testing High availability 
       
   iii) Installer mode - multi master  multi node(Kops & kubadm) AKS ,EKS

        a) Establishing architecture

        b) Developing a microservice

        c) Deploying a microservice

        d) Exposing a microservice
       
        e) Accessing microservice internally and externally

        f) Establishing High availability 
       
        g) Smoke testing &Performance testing High availability
       
       
7.Kube network policy
=====================

- Kube dns

8-a.Advance concepts of K8s
============================

- Job & scheduling pods(Custom scheduling)
    
    i. Job object
    
    ii. Job patterns

- Config map
- Ingress controller, reverse proxy
- Persistent volume
- Storage class
- Stateless and stateful applications
- Helm chart  
- Logging and monitoring in kubernetes
- Taints and tolerations

8-b.Kubernetes Administration   
=================================

- RBAC
- Users and management
- Networking
- Node Maintenance

9.Securing Kubernetes cluster  (Advanced)
==========================================

- Understanding authentication 
- What ServiceAccounts are and why they're used 
- Understanding the role-based access control (RBAC) plugin 
- Using Roles and RoleBindings 
- Using ClusterRoles and ClusterRoleBindings 
- Understanding the default roles and bindings 

10.Kubernetes with devops
==========================

11.Openshift – on-premise 
==========================

12.Pivotal CloudFoundry
========================
