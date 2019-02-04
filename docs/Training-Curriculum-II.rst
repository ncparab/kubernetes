###############
Training
###############

Curriculum 
------------

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
|         |  1) Control plane                                           |             |
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
|         | ASSIGNMENT                                                  |             |
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
|         |          v) Accessing microservice                          |             |
+         +                                                             +             +
|         |          vi) Establishing High availability                 |             |
+         +                                                             +             +
|         |          vii) Performance testing on kube cluster           |             |
+         +                                                             +             +
|         |          viii) Smoke testing &Performance testing High      |             |
+         +             availability                                    +             +
|         |                                                             |             |
+---------+-------------------------------------------------------------+-------------+
| Day 3   |  Cloud Agnostic Installation (Installer Mode)               | 3 Hrs       |
+         +-------------------------------------------------------------+             +                                              
|         |   1) KUBEADM                                                |             | 
+         +                                                             +             +                                              
|         |      a) Overview                                            |             |
+         +                                                             +             +                                              
|         |      b) Installation                                        |             |
+         +                                                             +             +                                              
|         |      c) Validation                                          |             |
+         +                                                             +             +                                              
|         |      e) Deployments                                         |             |
+         +                                                             +             +                                              
|         |   2) KOPS                                                   |             |
+         +                                                             +             +                                              
|         |      a) Overview                                            |             |
+         +                                                             +             + 
|         |      b) Installtion                                         |             |
+         +                                                             +             + 
|         |      c) Validation                                          |             |
+         +                                                             +             + 
|         |      d) Deployments                                         |             |
+         +                                                             +             + 
|         |      e) Accessing Services                                  |             |
+         +-------------------------------------------------------------+             +                                              
|         |  ASSIGNMENT                                                 |             |
+---------+-------------------------------------------------------------+-------------+
| Day 4   | Self Hosted Kubernetes Cluster                              | 4 hrs       |
+         +                                                             +             +
|         |  1) Single Master Architecture                              |             |
+         +                                                             +             +
|         |     a) Installing the Client Tools                          |             |
+         +                                                             +             +
|         |     b) Provision the CA and Generating TLS Certificates     |             |
+         +                                                             +             +
|         |     c) Generate Kubernetes Configuration Files for          |             |
|         |        Authentication                                       |             |
+         +                                                             +             +
|         |     d) Bootstrap the etcd Cluster                           |             |
+         +                                                             +             +
|         |     e) Bootstrap the Kubernetes Control Plane               |             |
+         +                                                             +             +
|         |     f) Bootstrap the Kubernetes Worker Node                 |             |
+         +                                                             +             +
|         |     g) Smoke Test                                           |             |
+         +                                                             +             +
|         |  2) Multi-Master – Multi node Architecture                  |             |
+         +                                                             +             +
|         |     a) Establishing architecture                            |             |
+         +                                                             +             +
|         |     b) Kubernetes Control plane components                  |             |
+         +                                                             +             +
|         |     c) Kubernetes Worker  node                              |             |
+         +                                                             +             +
|         |     d) Exposing a microservice                              |             |
+         +                                                             +             +
|         |     e) Smoke testing                                        |             |
+         +-------------------------------------------------------------+             +
|         |   ASSINMENT                                                 |             |
+---------+-------------------------------------------------------------+-------------+
| Day 5   |  AKS –                                                      | 4 hrs       |
+         +                                                             +             +
|         |    1) Introduction                                          |             |
+         +                                                             +             +
|         |    2) Azure Kubernetes Service                              |             |
+         +                                                             +             +
|         |    3) Azure container Registry                              |             |
+         +                                                             +             +
|         |    4) Azure Kubernetes cluster setup                        |             |
+         +                                                             +             +
|         |    5) Deployments to AKS                                    |             |
+         +                                                             +             +
|         |    6) Accessing AKS applications                            |             |
+         +-------------------------------------------------------------+             +
|         | EKS –                                                       |             | 
+         +                                                             +             +
|         |    1) Introduction                                          |             |
+         +                                                             +             +
|         |    2) Amazon Web services                                   |             |
+         +                                                             +             +
|         |    3) Elastic Kubernetes Service                            |             |
+         +                                                             +             +
|         |    4) EKS Setup                                             |             |
+         +                                                             +             +
|         |    5) Deployments to EKS                                    |             |
+         +                                                             +             +
|         |    6) Accessing EKS applciations                            |             |
+         +-------------------------------------------------------------+             +
|         |   ASSIGNMENT                                                |             |
+---------+-------------------------------------------------------------+-------------+
