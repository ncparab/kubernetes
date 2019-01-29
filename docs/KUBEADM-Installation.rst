########################
KUBEADM Installation
########################

Overview of kubeadm
--------------------

Kubeadm is a tool built to provide kubeadm init and kubeadm join as best-practice “fast paths” for creating Kubernetes clusters. kubeadm performs the actions necessary to get a minimum viable cluster up and running. By design, it cares only about bootstrapping, not about provisioning machines. Likewise, installing various nice-to-have addons, like the Kubernetes Dashboard, monitoring solutions, and cloud-specific addons, is not in scope.

Pre-requistes:
--------------

- One or more machines running a deb/rpm-compatible OS, for example Ubuntu or CentOS
- 2 GB or more of RAM per machine. Any less leaves little room for your apps.
- 2 CPUs or more on the master
- Full network connectivity among all machines in the cluster. A public or private network is fine.

Installing kubeadm on your hosts by(ubuntu):
=============================================

.. code-block:: bash

   $ apt-get update && apt-get install -y apt-transport-https curl
   $ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key 
   $ add -
     cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
     deb https://apt.kubernetes.io/ kubernetes-xenial main
     EOF
   $ apt-get update
   $ apt-get install -y kubelet kubeadm kubectl
   $ apt-mark hold kubelet kubeadm kubectl

Installing kubeadm on your hosts by(Fedora):
=============================================


The command to initialize the control plane is $kubeadm init. You can pass parameters such as api server listening endpoint, pod network cidr and kubernetes version to be installed.
Kubeadm allows you to create a control-plane node in phases.The init command executes the series of phases:


Once installed you can check the version of the kubeadm installed by 

.. code-block:: bash

   $kubeadm version


Ensure all the Kubernetes control plane components are in running  status.



At this point you’ve kubernetes control plane components, but you can able to add worker nodes to this kubeadm cluster by,

kubeadm join:
^^^^^^^^^^^^^^

Joining your nodes:
^^^^^^^^^^^^^^^^^^^

The nodes are where your workloads (containers and pods, etc) run. To add new nodes to your cluster do the following for each machine:

SSH to the machine
Become root (e.g. sudo su -)
Run the command that was output by kubeadm init. Example as shown:
 
Run this on any machine you wish to join an existing cluster. kubeadm configures the local kubelet to connect to the API server with the definitive identity assigned to the node.

.. code-block:: bash

   $ kubeadm join --discovery-token abcdef.1234567890abcdef --discovery-token-ca-cert-hash sha256:<hash>

If you don’t have the value of --discovery-token-ca-cert-hash, you can get it by running the following command chain on the master node:

.. code-block:: bash

   $ openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | \
     openssl dgst -sha256 -hex | sed 's/^.* //'


This is token-based discovery of the kubeadm master with CA pinning.


The token outputted by kubeadm is valid for 23h. If expired you can generate another token by 


.. code-block:: bash

   $kubeadm token create

You can get the existing token list by 

.. code-block:: bash

   $kubectl token list

Once the worker node is registered to the master, application deployments or pods can be deployed the same way we did in minikube. But if we want to consider the master node alone for the deployments as well, then that can be done by removing taints on the master node as shown:



At this point, kubernetes would be abe to schedule the pods on the master node too. But if there is any error that is related to network plugin during the scheduling of pods for example as shown below, you would need to install CNI-plugin for pod/container communication.


You can install CNI-plugin such as Calico or weavenet here after to put forth the pod/container communication, which we’ll showcase in implementation of self-hosted kubernetes cluster.

Install calic CNI plugin by:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   $ kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/rbac-kdd.yaml
   $ kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml

Ensure the calico pods are running as expected:

Now you can create a deployment, with the deployment descriptor or application YAML file and run it with kubectl command. Again Ensure the pod is in running state that is created by the deployment. 



Now you can expose the deployment as of type NodePort so that the service available for the clients.
