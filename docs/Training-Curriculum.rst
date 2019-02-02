######################
Training
######################

Curriculum
-----------

Week - I
'''''''''

+---------+-------------------------------------------------------------+-------------+
|**Days** |                    **Topics**                               |**Durartion**|
+---------+-------------------------------------------------------------+-------------+
| Day 1   | Introduction to of Virtualization/Paravirtulization         | 1.5 Hrs     |
+         +-------------------------------------------------------------+             +
|         | Introduction to Containers                                  |             |
+         +-------------------------------------------------------------+             +
|         | Virtualizion vs Containerized Architecture                  |             |
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
+         +-------------------------------------------------------------+             +
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
| Day 4   | Docker Commands - II                                        | 3 Hrs       |
+         +-------------------------------------------------------------+             +
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
|         | 11) docker rmi                                              |             |
+         +                                                             +             +
|         | 12) docker start/stop                                       |             |
+         +                                                             +             +
|         | 13) docker stats                                            |             |
+         +                                                             +             +
|         | 14) docker log                                              |             |
+         +                                                             +             +
|         | 15) docker diff                                             |             |
+         +                                                             +             +
|         | 16) docker network                                          |             |
+---------+-------------------------------------------------------------+-------------+
| Day 5   | Docker networking                                           | 3 Hrs       |
+         +-------------------------------------------------------------+             +
|         | Docker parameter                                            |             |
+         +-------------------------------------------------------------+             +
|         | Automating docker deployment                                |             |
+         +-------------------------------------------------------------+             +
|         | Docker security setup                                       |             |
+         +-------------------------------------------------------------+             +
|         | Docker registry setup                                       |             |
+---------+-------------------------------------------------------------+-------------+
| Day 6   | Demo - Trusted relationship setup                           | 3 Hrs       |
+         +-------------------------------------------------------------+             +
|         | Dockerfile                                                  |             |
+         +-------------------------------------------------------------+             +
|         | Demo - Microservice flask                                   |             |
+         +-------------------------------------------------------------+             +
|         | Airflow Setup demo with Rabbit MQ and postgresql            |             |
+---------+-------------------------------------------------------------+-------------+
| Day 7   | Docker lab exercise                                         | 3 Hrs       |
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
''''''''''

+---------+-------------------------------------------------------------+-------------+
|**Days** |                    **Topics**                               |**Durartion**|
+---------+-------------------------------------------------------------+-------------+
| Day 1   | Container Orchestration                                     | 4 Hrs       |
+         +-------------------------------------------------------------+             +
|         | History of Orchestration tools                              |             |
+         +-------------------------------------------------------------+             +
|         | Kubernetes architecture                                     |             |
+         +                                                             +             +
|         |  1) Minukube architecture                                   |             |
+         +                                                             +             +
|         |  2) Self hosted Kubernetes Cluster Architecture             |             |
+         +                                                             +             +
|         |  3) Kops                                                    |             |
+         +                                                             +             +
|         |  4) Kubadm                                                  |             |
+         +                                                             +             +
|         |  5) Eks                                                     |             |
+         +                                                             +             +
|         |  6) Aks                                                     |             |
+         +-------------------------------------------------------------+             +
|         | Kubernetes Components                                       |             |
+         +                                                             +             +
|         |  1) Control plance                                          |             |
+         +                                                             +             +
|         |      a) Kube api server                                     |             |
+         +                                                             +             +
|         |      b) Kuber controller manager                            |             |
+         +                                                             +             +
|         |      c) Kube scheduler                                      |             |
+         +                                                             +             +
|         |      d) Etcd                                                |             |
+         +                                                             +             +
|         |      e) Load Balancer                                       |             |
+         +                                                             +             +
|         |  2) Worker plane                                            |             |
+         +                                                             +             +
|         |      a) Kubelet                                             |             |
+         +                                                             +             +
|         |      b) Kubeproxy                                           |             |
+         +-------------------------------------------------------------+             +
|         | Kubernetes concepts                                         |             |
+         +                                                             +             +
|         |  1) Pods                                                    |             |
+         +                                                             +             +
|         |  2) Namespaces                                              |             |
+         +                                                             +             +
|         |  3) Rs, rc                                                  |             |
+         +                                                             +             +
|         |  4) Deployments                                             |             |
+         +                                                             +             +
|         |  5) Services                                                |             |
+         +                                                             +             +
|         |  6) Labels                                                  |             |
+         +                                                             +             +
|         |  7) Volumes                                                 |             |
+         +                                                             +             +
|         |  8) Hpa                                                     |             |
+         +                                                             +             +
|         |  9) Daemonsets                                              |             |
+         +-------------------------------------------------------------+             +
|         | Kubernetes Cli                                              |             |
+         +                                                             +             +
|         |  1) Kubectl                                                 |             |
+         +                                                             +             +
|         |  2) Kops                                                    |             |
+         +-------------------------------------------------------------+             +
|         | ASSIGNMENT(day -1)                                          |             |
+---------+-------------------------------------------------------------+-------------+
| Day 2   | Kubernetes Cluster Setup - I                                | 4 Hrs       |
+         +                                                             +             +
|         |  1) Dev Mode                                                |             |
+         +                                                             +             +
|         |      a) Minikube Installation                               |             |
+         +                                                             +             +
|         |          i) Establishing architecture                       |             |
+         +                                                             +             +
|         |          ii) Developing a microservice                      |             |
+         +                                                             +             +
|         |          iii) Deploying a microservice                      |             |
+         +                                                             +             +
|         |          iv) Exposing a microservice                        |             |
+         +                                                             +             +
|         |          v) Accessing microservice internally and externally|             |
+         +                                                             +             +
|         |          vi) Establishing High availability                 |             |
+         +                                                             +             +
|         |      b) Performance testing on kube cluster                 |             |
+         +                                                             +             +
|         |          i) Smoke testing &Performance testing High         |             |
+         +             availability                                    +             +
|         |  2) Installer Mode  (multi master multi node)               |             |
+         +                                                             +             +                                              
|         |      a) Establishing architecture                           |             | 
+         +                                                             +             +                                              
|         |      b) Developing a microservice                           |             |
+         +                                                             +             +                                              
|         |      c) Deploying a microservice                            |             |
+         +                                                             +             +                                              
|         |      d) Exposing a microservice                             |             |
+         +                                                             +             +                                              
|         |      e) Accessing microservice internally and externally    |             |
+         +                                                             +             +                                              
|         |      f) Establishing High availability                      |             |
+         +                                                             +             +                                              
|         |      g) Smoke testing &Performance testing High availability|             |
+         +-------------------------------------------------------------+             +                                              
|         | ASSIGNMENT                                                  |             |
+---------+-------------------------------------------------------------+-------------+
| Day 3   | Kubernetes Cluster Setup - II                               | 4 hrs       |
+         +                                                             +             +
|         |  1) Self Hosted Mode                                        |             |
+         +                                                             +             +
|         |      a) Establishing architecture                           |             |
+         +                                                             +             +
|         |      b) Developing a microservice                           |             |
+         +                                                             +             +
|         |      c) Deploying a microservice                            |             |
+         +                                                             +             +
|         |      d) Exposing a microservice                             |             |
+         +                                                             +             +
|         |      e) Accessing microservice internally and externally    |             |
+         +                                                             +             +
|         |      f) Establishing High availability                      |             |
+         +                                                             +             +
|         |      g) Smoke testing &Performance testing High availability|             |
+         +                                                             +             +
|         |  2) Prod Mode                                               |             |
+         +                                                             +             +
|         |      a) Establishing architecture                           |             |
+         +                                                             +             +
|         |      b) Developing a microservice                           |             |
+         +                                                             +             +
|         |      c) Deploying a microservice                            |             |
+         +                                                             +             +
|         |      d) Exposing a microservice                             |             |
+         +                                                             +             +
|         |      e) Accessing microservice internally and externally    |             |
+         +                                                             +             +
|         |      f) Establishing High availability                      |             |
+         +                                                             +             +
|         |      g) Smoke testing &Performance testing High availability|             |
+         +-------------------------------------------------------------+             +
|         | ASSINMENT                                                   |             |
+---------+-------------------------------------------------------------+-------------+
| Day 4   | Kubernetes Networking                                       | 4 hrs       |
+         +                                                             +             +
|         | Advance concepts of K8s                                     |             |
+         +                                                             +             +
|         |      a) Job & scheduling pods                               |             |
+         +                                                             +             +
|         |           i) Job object                                     |             |
+         +                                                             +             +
|         |           ii) Job patterns                                  |             |
+         +                                                             +             +
|         |           iii) Taints & Tolerations                         |             |
+         +                                                             +             +
|         |      b) Config map                                          |             |
+         +                                                             +             +
|         |      c) Ingress controller, reverse proxy                   |             |
+         +                                                             +             +
|         |      d) Persistent volume                                   |             |
+         +                                                             +             +
|         |      e) Storage class                                       |             |
+         +                                                             +             +
|         |      f) Stateless and stateful applications                 |             |
+         +                                                             +             +
|         |      g) Helm chart                                          |             |
+         +                                                             +             +
|         |      h) Logging and monitoring in kubernetes                |             |
+         +                                                             +             +
|         |      i) Taints and tolerations                              |             |
+         +-------------------------------------------------------------+             +
|         | Kubernetes Administration                                   |             |
+         +                                                             +             +
|         |      a) RBAC                                                |             |
+         +                                                             +             +
|         |      b) Users and management                                |             |
+         +                                                             +             +
|         |      c) Node Maintenance                                    |             |
+         +-------------------------------------------------------------+             +
|         | Securing Kubernetes cluster  (Advanced)                     |             |
+         +                                                             +             +
|         |      a) Understanding authentication                        |             |
+         +                                                             +             +
|         |      b) What ServiceAccounts are and why they're used       |             |
+         +                                                             +             +
|         |      c) Understanding the role-based access control(RBAC)   |             |
+         +         plugin                                              +             +
|         |      d) Using Roles and RoleBindings                        |             |
+         +                                                             +             +
|         |      e) Using ClusterRoles and ClusterRoleBindings          |             |
+         +                                                             +             +
|         |      f) Understanding the default roles and bindings        |             |
+         +-------------------------------------------------------------+             +
|         | ASSIGNMENT                                                  |             |
+---------+-------------------------------------------------------------+-------------+
| Day 5   | Kubernetes with devops                                      | 4 Hrs       |
+         +-------------------------------------------------------------+             +
|         | Openshift – on-premise                                      |             | 
+         +-------------------------------------------------------------+             +
|         | Pivotal CloudFoundry                                        |             |
+         +-------------------------------------------------------------+             +
|         | ASSIGNMENT                                                  |             |
+---------+-------------------------------------------------------------+-------------+


