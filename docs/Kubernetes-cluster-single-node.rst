###############################################
Kubernetes Cluster (Single Master multi-node)
###############################################

Kubernetes requires a set of machines to host the Kubernetes control plane and the worker nodes where containers are ultimately run.
In this section we demonstrate setting up single master multi worker node configuration for the Kubernetes Cluster.

Prerequisites:
---------------

Ensure the following ports are open on the instances:

- Control node:

+-----+------------+--------------------------+
| TCP |  6443*     | Kubernetes API Server    |
+-----+------------+--------------------------+
| TCP | 2379-2380  | etcd server client API   |
+-----+------------+--------------------------+
| TCP | 10250      | Kubelet API              |
+-----+------------+--------------------------+
| TCP | 10251      | kube-scheduler           |
+-----+------------+--------------------------+
| TCP | 10252      | kube-controller-manager  |
+-----+------------+--------------------------+
| TCP | 10255      |  Read-Only Kubelet API   |
+-----+------------+--------------------------+

The following image shows ports needed for master – worker node communication.


The compute resources can be Bare metal servers or virtual machines. Series of steps we follow for our cluster to be set up:

- Installing the Client Tools:
- Provision the CA and Generating TLS Certificates
- Generate Kubernetes Configuration Files for Authentication
- Bootstrap the etcd Cluster
- Bootstrap the Kubernetes Control Plane
- Bootstrap the Kubernetes Worker Node
- Smoke Test

Client Tools:
-------------

- Install cfssl and kubectl tools on your machine.

Linux:
'''''''

.. code-block:: bash

   $ wget -q --show-progress --https-only --timestamping \
      https://pkg.cfssl.org/R1.2/cfssl_linux-amd64 \
      https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64

   $ chmod +x cfssl_linux-amd64 cfssljson_linux-amd64

   $ sudo mv cfssl_linux-amd64 /usr/local/bin/cfssl

   $ sudo mv cfssljson_linux-amd64 /usr/local/bin/cfssljson

Verify the installation by:
'''''''''''''''''''''''''''

.. code-block:: bash

   $ cfssl version

- Kubectl – Using kubectl, you can inspect cluster resources; create, delete, and update components; look at your new cluster; 
and bring up example apps.

Linux:
'''''''

.. code-block:: bash

   $ wget https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kubectl

   $ chmod +x kubectl

   $ sudo mv kubectl /usr/local/bin/

Verify installation using the below command. The output should be like as shown:

.. code-block:: bash

   $ Kubectl version

CA Certificate:
''''''''''''''''

Generate the CA configuration file, certificate, and private key:

.. code-block:: bash

   {

   cat > ca-config.json <<EOF
   {
     "signing": {
       "default": {
           "expiry": "8760h"
          },
        "profiles": {
           "kubernetes": {
                  "usages": ["signing", "key encipherment", "server auth", "client auth"],
                  "expiry": "8760h"
               }
              }
            }
           }
   EOF

   cat > ca-csr.json <<EOF
   {
      "CN": "Kubernetes",
      "key": {
         "algo": "rsa",
          "size": 2048
             },
      "names": [
          {
           "C": "US",
           "L": "Portland",
             "O": "Kubernetes",
             "OU": "CA",
             "ST": "Oregon"
           }
         ]
       }
   EOF

   cfssl gencert -initca ca-csr.json | cfssljson -bare ca
   }
   
Results:
'''''''''

You’ll see the following files in the current directory:

- ca-key.pem                   
- ca.pem

Admin Client Certificate;
''''''''''''''''''''''''''

Generate the admin client certificate and private key:

.. code-block:: bash

   {

    cat > admin-csr.json <<EOF
    {
      "CN": "admin",
      "key": {
        "algo": "rsa",
        "size": 2048
         },
      "names": [
        {
          "C": "US",
          "L": "Portland",
          "O": "system:masters",
          "OU": "Kubernetes The Hard Way",
          "ST": "Oregon"
          }
        ]
       }
   EOF

   cfssl gencert \
     -ca=ca.pem \
     -ca-key=ca-key.pem \
     -config=ca-config.json \
     -profile=kubernetes \
      admin-csr.json | cfssljson -bare admin

     }
     
Results:
'''''''''

- admin-key.pem
- admin.pem

The Kubelet Client Certificates:
'''''''''''''''''''''''''''''''''

Kubernetes uses a special-purpose authorization mode called Node Authorizer, that specifically authorizes API requests made by Kubelets.
In order to be authorized by the Node Authorizer, Kubelets must use a credential that identifies them as being in the system:nodes 
group, with a username of system:node:<nodeName>.

creating a certificate for each Kubernetes worker node that meets the Node Authorizer requirements.

Generate a certificate and private key for each Kubernetes worker node:

Replace {WORKER1_HOST} with the hostnames of the each of the worker nodes in the following snippet and generate key and cert file 
pertaining to that worker node.

.. code-block:: bash

   {  
    cat > ${WORKER1_HOST}-csr.json << EOF  
   {  
      "CN": "system:node:${WORKER1_HOST}",  
      "key": {  
        "algo": "rsa",  
        "size": 2048  
         },  
      "names": [  
       {  
         "C": "US",  
         "L": "Portland",  
         "O": "system:nodes",  
         "OU": "Kubernetes The Hard Way",  
         "ST": "Oregon"  
        }  
       ]  
      }  
   EOF  

   cfssl gencert \  
     -ca=ca.pem \  
     -ca-key=ca-key.pem \  
     -config=ca-config.json \  
     -hostname=${WORKER1_IP},${WORKER1_HOST} \  
     -profile=kubernetes \  
     ${WORKER1_HOST}-csr.json | cfssljson -bare ${WORKER1_HOST}  
    }
    
You will see the following files generated for each and every worker.

.. code-block:: bash

   ec2-13-56-183-161.us-west-1.compute.amazonaws.com.pem         --- worker 1 cert file
   ec2-54-193-116-183.us-west-1.compute.amazonaws.com-csr.json 
   ec2-13-56-183-161.us-west-1.compute.amazonaws.com-key.pem     --- worker 1 key file
   ec2-13-56-183-161.us-west-1.compute.amazonaws.com-csr.json  
   ec2-54-193-116-183.us-west-1.compute.amazonaws.com.csr       
   ec2-54-193-116-183.us-west-1.compute.amazonaws.com-key.pem    --- worker 2 key file
   ec2-54-193-116-183.us-west-1.compute.amazonaws.com.pem        --- worker 2 cert file
   ec2-13-56-183-161.us-west-1.compute.amazonaws.com.csr       
   
The Controller Manager Client Certificate
'''''''''''''''''''''''''''''''''''''''''''

Generate the kube-controller-manager client certificate and private key:

.. code-block:: bash

   {
    cat > kube-controller-manager-csr.json <<EOF
   {
       "CN": "system:kube-controller-manager",
       "key": {
         "algo": "rsa",
         "size": 2048
          },
       "names": [
       {
          "C": "US",
          "L": "Portland",
          "O": "system:kube-controller-manager",
          "OU": "Kubernetes The Hard Way",
          "ST": "Oregon"
          }
         ]
        }
   EOF

   cfssl gencert \
     -ca=ca.pem \
     -ca-key=ca-key.pem \
     -config=ca-config.json \
     -profile=kubernetes \
      kube-controller-manager-csr.json | cfssljson -bare kube-controller-manager

    }
    
As a result, You will see the following files generated:

- kube-controller-manager-key.pem
- kube-controller-manager.pem

The Kube Proxy Client Certificate
''''''''''''''''''''''''''''''''''

Generate the kube-proxy client certificate and private key:

.. code-block:: bash

   {

    cat > kube-proxy-csr.json <<EOF
   {
      "CN": "system:kube-proxy",
      "key": {
      "algo": "rsa",
      "size": 2048
      },
      "names": [
           {
             "C": "US",
             "L": "Portland",
             "O": "system:node-proxier",
             "OU": "Kubernetes The Hard Way",
             "ST": "Oregon"
             }
           ]
         }
   EOF

   cfssl gencert \
     -ca=ca.pem \
     -ca-key=ca-key.pem \
     -config=ca-config.json \
     -profile=kubernetes \
      kube-proxy-csr.json | cfssljson -bare kube-proxy

      }
      
You will see the following files generated executing the above code:

- kube-proxy-key.pem
- kube-proxy.pem

The Scheduler Client Certificate
---------------------------------

Generate the kube-scheduler client certificate and private key:

.. code-block:: bash

   {

    cat > kube-scheduler-csr.json <<EOF
   {
      "CN": "system:kube-scheduler",
      "key": {
      "algo": "rsa",
      "size": 2048
      },
     "names": [
       {
         "C": "US",
         "L": "Portland",
         "O": "system:kube-scheduler",
         "OU": "Kubernetes The Hard Way",
         "ST": "Oregon"
         }
       ]
      }
    EOF

    cfssl gencert \
     -ca=ca.pem \
     -ca-key=ca-key.pem \
     -config=ca-config.json \
     -profile=kubernetes \
      kube-scheduler-csr.json | cfssljson -bare kube-scheduler

     }
     
You should see following files

- kube-scheduler-key.pem 
- kube-scheduler.pem 

The Kubernetes API Server Certificate

static IP address will be included in the list of subject alternative names for the Kubernetes API Server certificate. This will 
ensure the certificate can be validated by remote clients.

The Control node hostname(ec2-13-57-5-169.us-west-1.compute.amazonaws.com) is provided to generate the cert.

Generate the Kubernetes API Server certificate and private key:

.. code-block:: bash

   {

   cat > kubernetes-csr.json << EOF
   {
     "CN": "kubernetes",
     "key": {
       "algo": "rsa",
       "size": 2048
       },
     "names": [
       {
         "C": "US",
         "L": "Portland",
         "O": "Kubernetes",
         "OU": "Kubernetes The Hard Way",
         "ST": "Oregon"
         }
       ]
     }
   EOF

   cfssl gencert \
     -ca=ca.pem \
     -ca-key=ca-key.pem \
     -config=ca-config.json \
     -hostname=${CERT_HOSTNAME} \
     -profile=kubernetes \
      kubernetes-csr.json | cfssljson -bare kubernetes

     }

Results:
'''''''''

- kubernetes-key.pem
- kubernetes.pem

The Service Account Key Pair

The Kubernetes Controller Manager leverages a key pair to generate and sign service account tokens as describe in the managing service 
accounts documentation.

Generate the service-account certificate and private key:

.. code-block:: bash

   {

   cat > service-account-csr.json <<EOF
   {
     "CN": "service-accounts",
     "key": {
        "algo": "rsa",
        "size": 2048
       },
     "names": [
       {
         "C": "US",
         "L": "Portland",
         "O": "Kubernetes",
         "OU": "Kubernetes The Hard Way",
         "ST": "Oregon"
        }
      ]
     }
   EOF

   cfssl gencert \
    -ca=ca.pem \
    -ca-key=ca-key.pem \
    -config=ca-config.json \
    -profile=kubernetes \
     service-account-csr.json | cfssljson -bare service-account

   }
   
Results:
'''''''''

- service-account-key.pem
- service-account.pem

At this point you should be having all the TLS cert files in your machine.

The kube-proxy, kube-controller-manager, kube-scheduler, and kubelet client certificates will be used to generate client authentication configuration files in the next section.
Distribute the client and server certificates  to worker nodes and control node:

.. code-block:: bash

   $ scp ca.pem ca-key.pem kubernetes-key.pem kubernetes.pem \
     service-account-key.pem service-account.pem ubuntu@ec2-13-57-5-169.us-west-1.compute.amazonaws.com:~/




for instance in W1 W2; do

.. code-block:: bash

   $ scp ca.pem ${hostname}-key.pem ${hostname}.pem ${hostname}:~/
   
done


Generating Kubernetes Configuration Files for Authentication

- Client Authentication Configs:

Each kubeconfig requires a Kubernetes API Server to connect to . Generate kubeconfig files for the controller manager, kubelet, kube-proxy, and scheduler clients and the admin user by providing the   Kubernetes Public IP Address. The “Kubernetes Public IP Address” will be the Control nodes public IP. Assign the public IP of the control node to a environment variable.

- kubelet Configuration File:

When generating kubeconfig files for Kubelets the client certificate matching the Kubelet's node name must be used. This will ensure 
Kubelets are properly authorized by the Kubernetes Node Authorizer.
Generate a kubeconfig file for each worker node:
for instance in W1 W2; do

.. code-block:: bash

   kubectl config set-cluster kubernetes-the-hard-way \
      --certificate-authority=ca.pem \
      --embed-certs=true \
      --server=https://${ Cnode_Public_IP}:6443 \
      --kubeconfig=${hostname}.kubeconfig

   kubectl config set-credentials system:node:${instance} \
      --client-certificate=${instance}.pem \
      --client-key=${instance}-key.pem \
      --embed-certs=true \
      --kubeconfig=${instance}.kubeconfig

   kubectl config set-context default \
      --cluster=kubernetes-the-hard-way \
      --user=system:node:${instance} \
      --kubeconfig=${instance}.kubeconfig

    kubectl config use-context default --kubeconfig=${instance}.kubeconfig

done

At this point, kubeconfig file will be generated for  each and every  worker  node
Generate a kube-proxy kubeconfig

- kube-proxy Kubernetes Configuration File:

Generate a kubeconfig file for the kube-proxy service:

.. code-block:: bash

   {
     kubectl config set-cluster kubernetes-the-hard-way \
      --certificate-authority=ca.pem \
      --embed-certs=true \
      --server=https://${KUBERNETES_PUBLIC_ADDRESS}:6443 \
      --kubeconfig=kube-proxy.kubeconfig

     kubectl config set-credentials system:kube-proxy \
      --client-certificate=kube-proxy.pem \
      --client-key=kube-proxy-key.pem \
      --embed-certs=true \
      --kubeconfig=kube-proxy.kubeconfig

     kubectl config set-context default \
      --cluster=kubernetes-the-hard-way \
      --user=system:kube-proxy \
      --kubeconfig=kube-proxy.kubeconfig

     kubectl config use-context default --kubeconfig=kube-proxy.kubeconfig
    }
    
Results:
'''''''''

- kube-proxy.kubeconfig

The kube-controller-manager Kubernetes Configuration File

Generate a kubeconfig file for the kube-controller-manager service:

.. code-block:: bash

   {
    kubectl config set-cluster kubernetes-the-hard-way \
      --certificate-authority=ca.pem \
      --embed-certs=true \
      --server=https://127.0.0,1:6443 \
      --kubeconfig=kube-controller-manager.kubeconfig

    kubectl config set-credentials system:kube-controller-manager \
      --client-certificate=kube-controller-manager.pem \
      --client-key=kube-controller-manager-key.pem \
      --embed-certs=true \
      --kubeconfig=kube-controller-manager.kubeconfig

    kubectl config set-context default \
      --cluster=kubernetes-the-hard-way \
      --user=system:kube-controller-manager \
      --kubeconfig=kube-controller-manager.kubeconfig

    kubectl config use-context default --kubeconfig=kube-controller-manager.kubeconfig
   }

You’ll see **kube-controller-manager.kubeconfig** file generated.

The kube-scheduler Kubernetes Configuration File

Generate a kubeconfig file for the kube-scheduler service:

.. code-block:: bash

   {
    kubectl config set-cluster kubernetes-the-hard-way \
      --certificate-authority=ca.pem \
      --embed-certs=true \
      --server=https:// 127.0.0,1:6443 \
      --kubeconfig=kube-scheduler.kubeconfig

    kubectl config set-credentials system:kube-scheduler \
      --client-certificate=kube-scheduler.pem \
      --client-key=kube-scheduler-key.pem \
      --embed-certs=true \
      --kubeconfig=kube-scheduler.kubeconfig

    kubectl config set-context default \
      --cluster=kubernetes-the-hard-way \
      --user=system:kube-scheduler \
      --kubeconfig=kube-scheduler.kubeconfig

    kubectl config use-context default --kubeconfig=kube-scheduler.kubeconfig
    }
Results:
''''''''''

- kube-scheduler.kubeconfig

The admin Kubernetes Configuration File

Generate a kubeconfig file for the admin user:

.. code-block:: bash

   {
     kubectl config set-cluster kubernetes-the-hard-way \
      --certificate-authority=ca.pem \
      --embed-certs=true \
      --server=https://127.0.0,1:6443 \
      --kubeconfig=admin.kubeconfig

     kubectl config set-credentials admin \
      --client-certificate=admin.pem \
      --client-key=admin-key.pem \
      --embed-certs=true \
      --kubeconfig=admin.kubeconfig

     kubectl config set-context default \
      --cluster=kubernetes-the-hard-way \
      --user=admin \
      --kubeconfig=admin.kubeconfig

     kubectl config use-context default --kubeconfig=admin.kubeconfig
    }

**admin.kubeconfig** will be gnereated at this point.

Distribute the Kubernetes Configuration Files
----------------------------------------------

Distribute Kubeconfig pertaining to that worker node and kube-proxy.kubeconfig to all of the worker nodes.
admin.kubeconfig kube-controller-manager.kubeconfig kube-scheduler.kubeconfig should be on the control node.

Bootstrap the etcd cluster(on control node)
--------------------------------------------

On the Control node, Download the official etcd release binaries :

.. code-block:: bash

   $ wget -q --show-progress --https-only --timestamping \
     "https://github.com/coreos/etcd/releases/download/v3.3.9/etcd-v3.3.9-linux-amd64.tar.gz"

Extract and install the etcd server and the etcdctl command line utility:

.. code-block:: bash

   {
     tar -xvf etcd-v3.3.9-linux-amd64.tar.gz
     sudo mv etcd-v3.3.9-linux-amd64/etcd* /usr/local/bin/
   }
   Configure the etcd Server
   {
     sudo mkdir -p /etc/etcd /var/lib/etcd
     sudo cp ca.pem kubernetes-key.pem kubernetes.pem /etc/etcd/
   }

Assign the internal IP to the Env variable:

.. code-block:: bash

   INTERNAL_IP = 172.31.17.241

Each etcd member must have a unique name within an etcd cluster. Set the etcd name to match the hostname of the current compute
instance:

.. code-block:: bash

   ETCD_NAME=${Cnode_hostname}
   Ex: ETCD_NAME= ec2-13-57-5-169.us-west-1.compute.amazonaws.com

Create the etcd.service systemd unit file:

.. code-block:: bash

   cat <<EOF | sudo tee /etc/systemd/system/etcd.service
   [Unit]
   Description=etcd
   Documentation=https://github.com/coreos

   [Service]
   ExecStart=/usr/local/bin/etcd \\
     --name ${ETCD_NAME} \\
     --cert-file=/etc/etcd/kubernetes.pem \\
     --key-file=/etc/etcd/kubernetes-key.pem \\
     --peer-cert-file=/etc/etcd/kubernetes.pem \\
     --peer-key-file=/etc/etcd/kubernetes-key.pem \\
     --trusted-ca-file=/etc/etcd/ca.pem \\
     --peer-trusted-ca-file=/etc/etcd/ca.pem \\
     --peer-client-cert-auth \\
     --client-cert-auth \\
     --initial-advertise-peer-urls https://${INTERNAL_IP}:2380 \\
     --listen-peer-urls https://${INTERNAL_IP}:2380 \\
     --listen-client-urls https://${INTERNAL_IP}:2379,https://127.0.0.1:2379 \\
     --advertise-client-urls https://${INTERNAL_IP}:2379 \\
     --initial-cluster-token etcd-cluster-0 \\
     --initial-cluster-state new \\
     --data-dir=/var/lib/etcd
   Restart=on-failure
   RestartSec=5

   [Install]
   WantedBy=multi-user.target
   EOF
   
- Start the etcd Server:

.. code-block:: bash

   {
     sudo systemctl daemon-reload
     sudo systemctl enable etcd
     sudo systemctl start etcd
   }

- Verify the Etcd Cluster by executing the following on the control node:

.. code-block:: bash

   $ sudo ETCDCTL_API=3 etcdctl member list \
     --endpoints=https://127.0.0.1:2379 \
     --cacert=/etc/etcd/ca.pem \
     --cert=/etc/etcd/kubernetes.pem \
     --key=/etc/etcd/kubernetes-key.pem


KUBERNETES COMPONENTS 
----------------------

The Kubernetes components that make up the control plane include the following components:

- Kubernetes API Server
- Kubernetes Scheduler
- Kubernetes Controller Manager

Each component is being run on the same machine in this case etcd too.

.. code-block:: bash

   $ sudo mkdir -p /etc/kubernetes/config

- Download the official Kubernetes release binaries:

.. code-block:: bash

   $ wget -q --show-progress --https-only --timestamping \
     "https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-apiserver" \
     "https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-controller-manager" \
     "https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-scheduler" \
     "https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kubectl"


- Install the Kubernetes binaries:

.. code-block:: bash

   {
    chmod +x kube-apiserver kube-controller-manager kube-scheduler kubectl
    sudo mv kube-apiserver kube-controller-manager kube-scheduler kubectl /usr/local/bin/
   }


- Configure the Kubernetes API Server


.. code-block:: bash

   {
      sudo mkdir -p /var/lib/kubernetes/

      sudo mv ca.pem ca-key.pem kubernetes-key.pem kubernetes.pem \
      service-account-key.pem service-account.pem \
      encryption-config.yaml /var/lib/kubernetes/
   }

- Create the kube-apiserver.service systemd unit file:

.. code-block:: bash

   cat <<EOF | sudo tee /etc/systemd/system/kube-apiserver.service
   [Unit]
   Description=Kubernetes API Server
   Documentation=https://github.com/kubernetes/kubernetes

   [Service]
   ExecStart=/usr/local/bin/kube-apiserver \\
     --advertise-address=${INTERNAL_IP} \\
     --allow-privileged=true \\
     --apiserver-count=3 \\
     --audit-log-maxage=30 \\
     --audit-log-maxbackup=3 \\
     --audit-log-maxsize=100 \\
     --audit-log-path=/var/log/audit.log \\
     --authorization-mode=Node,RBAC \\
     --bind-address=0.0.0.0 \\
     --client-ca-file=/var/lib/kubernetes/ca.pem \\
     --enable-admission-plugins=Initializers,NamespaceLifecycle,NodeRestriction,LimitRanger,ServiceAccount,DefaultStorageClass,ResourceQuota \\
     --enable-swagger-ui=true \\
     --etcd-cafile=/var/lib/kubernetes/ca.pem \\
     --etcd-certfile=/var/lib/kubernetes/kubernetes.pem \\
     --etcd-keyfile=/var/lib/kubernetes/kubernetes-key.pem \\
     --etcd-servers=https:// ec2-13-57-5-169.us-west-1.compute.amazonaws.com:2379 \\
     --event-ttl=1h \\
     --experimental-encryption-provider-config=/var/lib/kubernetes/encryption-config.yaml \\
     --kubelet-certificate-authority=/var/lib/kubernetes/ca.pem \\
     --kubelet-client-certificate=/var/lib/kubernetes/kubernetes.pem \\
     --kubelet-client-key=/var/lib/kubernetes/kubernetes-key.pem \\
     --kubelet-https=true \\
     --runtime-config=api/all \\
     --service-account-key-file=/var/lib/kubernetes/service-account.pem \\
     --service-cluster-ip-range=10.32.0.0/24 \\
     --service-node-port-range=30000-32767 \\
     --tls-cert-file=/var/lib/kubernetes/kubernetes.pem \\
     --tls-private-key-file=/var/lib/kubernetes/kubernetes-key.pem \\
     --v=2
   Restart=on-failure
   RestartSec=5

   [Install]
   WantedBy=multi-user.target
   EOF
   
- Configure the Kubernetes Controller Manager

Move the kube-controller-manager kubeconfig into place:

.. code-block:: bash

   $ sudo mv kube-controller-manager.kubeconfig /var/lib/kubernetes/
   
- Create the kube-controller-manager.service systemd unit file:

.. code-block:: bash

   cat <<EOF | sudo tee /etc/systemd/system/kube-controller-manager.service
   [Unit]
   Description=Kubernetes Controller Manager
   Documentation=https://github.com/kubernetes/kubernetes

   [Service]
   ExecStart=/usr/local/bin/kube-controller-manager \\
      --address=0.0.0.0 \\
      --cluster-cidr=10.200.0.0/16 \\
      --cluster-name=kubernetes \\
      --cluster-signing-cert-file=/var/lib/kubernetes/ca.pem \\
      --cluster-signing-key-file=/var/lib/kubernetes/ca-key.pem \\
      --kubeconfig=/var/lib/kubernetes/kube-controller-manager.kubeconfig \\
      --leader-elect=true \\
      --root-ca-file=/var/lib/kubernetes/ca.pem \\
      --service-account-private-key-file=/var/lib/kubernetes/service-account-key.pem \\
      --service-cluster-ip-range=10.32.0.0/24 \\
      --use-service-account-credentials=true \\
      --v=2
   Restart=on-failure
   RestartSec=5

   [Install]
   WantedBy=multi-user.target
   EOF
   
- Configure the Kubernetes Scheduler

Move the kube-scheduler kubeconfig into place:

.. code-block:: bash

   $ sudo mv kube-scheduler.kubeconfig /var/lib/kubernetes/
   
- Create the kube-scheduler.yaml configuration file:

.. code-block:: bash

   cat <<EOF | sudo tee /etc/kubernetes/config/kube-scheduler.yaml
      apiVersion: componentconfig/v1alpha1
   kind: KubeSchedulerConfiguration
   clientConnection:
      kubeconfig: "/var/lib/kubernetes/kube-scheduler.kubeconfig"
   leaderElection:
      leaderElect: true
   EOF
   
- Create the kube-scheduler.service systemd unit file:

.. code-block:: bash

   cat <<EOF | sudo tee /etc/systemd/system/kube-scheduler.service
   [Unit]
   Description=Kubernetes Scheduler
   Documentation=https://github.com/kubernetes/kubernetes

   [Service]
   ExecStart=/usr/local/bin/kube-scheduler \\
      --config=/etc/kubernetes/config/kube-scheduler.yaml \\
      --v=2
   Restart=on-failure
   RestartSec=5

   [Install]
   WantedBy=multi-user.target
   EOF
      
- Start the Controller Services

.. code-block:: bash

   $ sudo systemctl daemon-reload
   $ sudo systemctl enable kube-apiserver kube-controller-manager kube-scheduler
   $ sudo systemctl start kube-apiserver kube-controller-manager kube-scheduler

Check the status of the services.

Enable HTTP HEALTH Checks:

.. code-block:: bash

   $ sudo apt-get install -y nginx

.. code-block:: bash
   
   cat > kubernetes.default.svc.cluster.local <<EOF
   server {
      listen      80;
      server_name kubernetes.default.svc.cluster.local;

     location /healthz {
        proxy_pass                    https://127.0.0.1:6443/healthz;
        proxy_ssl_trusted_certificate /var/lib/kubernetes/ca.pem;
      }  
     }
   EOF
   {
   sudo mv kubernetes.default.svc.cluster.local \
    /etc/nginx/sites-available/kubernetes.default.svc.cluster.local

   sudo ln -s /etc/nginx/sites-available/kubernetes.default.svc.cluster.local /etc/nginx/sites-enabled/
   }

 
    sudo systemctl restart nginx && sudo systemctl enable nginx


- verify the comonents statuses by:

.. code-block:: bash

   $ kubectl get cs
   
The output should be as shown:

Bootstrap the Kubernetes Worker Nodes:

ON EACH WORKER:
'''''''''''''''

- INSTALL Docker, Kubelet and Kube-proxy as below:

Docker : Docker is a container runtime engine that Kubernetes should be compatible with the Docker 1.9.x - 1.11.x:. We can alternatively 
use containerd or other container runtimes. We showcased using Docker.



- Install Binaries:

.. code-block:: bash

   $ sudo apt-get -y install socat conntrack ipset
   $ wget -q --show-progress --https-only --timestamping \
      https://github.com/kubernetes-incubator/cri-tools/releases/download/v1.0.0-beta.0/crictl-v1.0.0-beta.0-linux-amd64.tar.gz \
      https://storage.googleapis.com/kubernetes-the-hard-way/runsc \
      https://github.com/opencontainers/runc/releases/download/v1.0.0-rc5/runc.amd64 \
      https://github.com/containernetworking/plugins/releases/download/v0.6.0/cni-plugins-amd64-v0.6.0.tgz \
      https://github.com/containerd/containerd/releases/download/v1.1.0/containerd-1.1.0.linux-amd64.tar.gz \
      https://storage.googleapis.com/kubernetes-release/release/v1.10.2/bin/linux/amd64/kubectl \
      https://storage.googleapis.com/kubernetes-release/release/v1.10.2/bin/linux/amd64/kube-proxy \
      https://storage.googleapis.com/kubernetes-release/release/v1.10.2/bin/linux/amd64/kubelet

   $ sudo mkdir -p etc/cni/net.d    /opt/cni/bin   /var/lib/kubelet  /var/lib/kube-proxy  /var/lib/kubernetes \ /var/run/kubernetes
   $ chmod +x kubectl kube-proxy kubelet runc.amd64 runsc
   $ sudo mv runc.amd64 runc
   $ sudo mv kubectl kube-proxy kubelet runc runsc /usr/local/bin/
   $ sudo tar -xvf crictl-v1.0.0-beta.0-linux-amd64.tar.gz -C /usr/local/bin/
   $ sudo tar -xvf cni-plugins-amd64-v0.6.0.tgz -C /opt/cni/bin/
   $ sudo tar -xvf containerd-1.1.0.linux-amd64.tar.gz -C /

Set the HOSNAME on each and every worker node and configure the kubelet config file as shown below:

For example on W1:

.. code-block:: bash

   HOSTNAME=ec2-13-56-183-161.us-west-1.compute.amazonaws.com
   $ Sudo mv ec2-13-56-183-161.us-west-1.compute.amazonaws.com-key.pem ec2-13-56-183-161.us-west-1.compute.amazonaws.com.pem /var/lib/kubelet/
   $ sudo mv ec2-13-56-183-161.us-west-1.compute.amazonaws.com.kubeconfig /var/lib/kubelet/kubeconfig
   $ sudo mv ca.pem /var/lib/kubernetes/

- create kubelet-config.yaml file by providing Kube-API server endpoint(herem CNODE_public_IP)

.. code-block:: bash

   cat <<EOF | sudo tee /var/lib/kubelet/kubelet-config.yaml
   kind: KubeletConfiguration
   apiVersion: kubelet.config.k8s.io/v1beta1
   authentication:
      anonymous:
         enabled: false
      webhook:
         enabled: true
      x509:
         clientCAFile: "/var/lib/kubernetes/ca.pem"
         authorization:
         mode: Webhook
   clusterDomain: "cluster.local"
   clusterDNS:
       - "10.32.0.10"
   podCIDR: "${POD_CIDR}"
   resolvConf: "/run/systemd/resolve/resolv.conf"
   runtimeRequestTimeout: "15m"
   tlsCertFile: "/var/lib/kubelet/ ec2-13-56-183-161.us-west-1.compute.amazonaws.com.pem"
   tlsPrivateKeyFile: "/var/lib/kubelet/ ec2-13-56-183-161.us-west-1.compute.amazonaws.com-key.pem"
   EOF


- Create Kubelet unit file 

.. code-block:: bash

   sudo sh -c 'echo "[Unit]
   Description=Kubernetes Kubelet
   Documentation=https://github.com/GoogleCloudPlatform/kubernetes
   After=docker.service
   Requires=docker.service

   [Service]
   ExecStart=/usr/bin/kubelet \
       --allow-privileged=true \
       --api-servers=https:// ec2-13-57-5-169.us-west-1.compute.amazonaws.com:6443 \
       --cloud-provider= aws \
       --cluster-dns=10.32.0.10 \
       --cluster-domain=cluster.local \
       --configure-cbr0=true \
       --container-runtime=docker \
       --docker=unix:///var/run/docker.sock \
       --network-plugin=cni \
       --config=/var/lib/kubelet/kubelet-config.yaml \
       --kubeconfig=/var/lib/kubelet/kubeconfig \
       --reconcile-cidr=true \
       --hostname-override=${HOSTNAME}
       --serialize-image-pulls=false \
       --tls-cert-file=/var/lib/kubernetes/kubernetes.pem \
       --tls-private-key-file=/var/lib/kubernetes/kubernetes-key.pem \
       --v=2

    Restart=on-failure
    RestartSec=5

    [Install]
    WantedBy=multi-user.target" > /etc/systemd/system/kubelet.service'


- Configure the Kubernetes Proxy :

.. code-block:: bash

   $ sudo mv kube-proxy.kubeconfig /var/lib/kube-proxy/kubeconfig



- Create the kube-proxy-config.yaml configuration file:

.. code-block:: bash

   cat <<EOF | sudo tee /var/lib/kube-proxy/kube-proxy-config.yaml
   kind: KubeProxyConfiguration
   apiVersion: kubeproxy.config.k8s.io/v1alpha1
   clientConnection:
      kubeconfig: "/var/lib/kube-proxy/kubeconfig"
   mode: "iptables"
   clusterCIDR: "10.200.0.0/16"
   EOF

- Create the kube-proxy.service systemd unit file:

.. code-block:: bash

   cat <<EOF | sudo tee /etc/systemd/system/kube-proxy.service
   [Unit]
   Description=Kubernetes Kube Proxy
   Documentation=https://github.com/kubernetes/kubernetes

   [Service]
   ExecStart=/usr/local/bin/kube-proxy \\
     --config=/var/lib/kube-proxy/kube-proxy-config.yaml
   Restart=on-failure
   RestartSec=5

   [Install]
   WantedBy=multi-user.target
   EOF

- Start the Worker Services

.. code-block:: bash

   {
     sudo systemctl daemon-reload
     sudo systemctl enable docker kubelet kube-proxy
     sudo systemctl start docker kubelet kube-proxy
   }


Remember to run the above commands on each worker node and verify the services are running.

At this point , triggering $kubectl get nodes command should list the registered nodes.


Provisioning Pod Network Routes
--------------------------------

Pods scheduled to a node receive an IP address from the node's Pod CIDR range. At this point pods can not communicate with other pods running on different nodes due to missing network routes.
Coordinating ports across multiple developers is very difficult to do at scale and exposes users to cluster-level issues outside of their control. Dynamic port allocation brings a lot of complications to the system - every application has to take ports as flags, the API servers have to know how to insert dynamic port numbers into configuration blocks, services have to know how to find each other, etc. Rather than deal with this, Kubernetes takes a different approach.
Kubernetes imposes the following fundamental requirements on any networking implementation:

- all containers can communicate with all other containers without NAT
- all nodes can communicate with all containers (and vice-versa) without NAT
- the IP that a container sees itself as is the same IP that others see it as

There are a number of ways that this network model can be implemented. We demonstrate using calico in this single master architecture.
Calico: Calico provides a highly scalable networking and network policy solution for connecting Kubernetes pods based on the same IP networking principles as the internet. Calico can be deployed without encapsulation or overlays to provide high-performance, high-scale data center networking. Calico also provides fine-grained, intent based network security policy for Kubernetes pods via its distributed firewall.
Download calico manifest:

.. code-block:: bash

   $ Curl  https://docs.projectcalico.org/v2.0/getting-started/kubernetes/installation/hosted/calico.yaml

priovide the Certs by setting the cert paths in the calico manifest bby uncommenting the following lines:

.. code-block:: bash

   etcd_ca: ""   # "/calico-secrets/etcd-ca"
   etcd_cert: "" # "/calico-secrets/etcd-cert"
   etcd_key: ""  # "/calico-secrets/etcd-key"

Apply the Calico Pod manifest by:

.. code-block:: bash

   $ Kubectl apply -f calico.yaml.
   
Executing the above command creates calico Pods that should be in running state.

Now, Create secrets if planning to use DockerHub as image repository so that Kubernetes will pull docker images from this repository.

.. code-block:: bash

   $ kubectl create secret docker-registry myregistrykey --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL

Smoke Test the cluster:
------------------------

- Testing the Deployment:

The below command will pull the image from the image repository(dockerhub) and creates a deployment for that application:

.. code-block:: bash

   $ kubectl run flaskapp –image=exeliq/flaskapp –port=5000

- KUBERNETES OBJECTS:

Alternatively, you can create an application deployment using a Kubernetes Object “deployment”

Creating  a application manifest:

When you create an object in Kubernetes, you must provide the object spec that describes its desired state, as well as some basic information about the object (such as a name). When you use the Kubernetes API to create the object that API request must include that information as JSON in the request body.
Alternatively, you can create application manifest/YAML that is similar to the following:

Creating a Deployment
The following is flask application Deployment manifest:


.. code-block:: bash

   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: flask-deployment
     labels:
        app: flask
   spec:
     replicas: 1
     selector:
      matchLabels:
         app: flask
     template:
      metadata:
         labels:
            app: nginx
      spec:
         containers:
             - name: fask
               image: exeliq/flaskapp
         ports:
             - containerPort: 5000


With the above Yaml, a deployment named flask-deployment is created, indicated by the .metadata.name field.

To create the flask Deployment run the following command:
   
.. code-block:: bash

   $ kubectl apply -f flask-deployment.yaml
   
 Next, run 
 
.. code-block:: bash

   $ kubectl get deployments

To view the running pods that backs the flask deployment, run:

.. code-block:: bash

   $ kubectl get pods -l run=flask-deployment

At this point you should be able to expose the flask deployment as a service that can be accessible by the clients.

- SERVICES:

Kubernetes Pods are ephemeral. While each Pod gets its own IP address, even those IP addresses cannot be relied upon to be stable over time. This leads to a problem: if some set of Pods (let’s call them backends) provides functionality to other Pods (let’s call them frontends) inside the Kubernetes cluster, how do those frontends find out and keep track of which backends are in that set? Hence. Services.
Deployments can be exposed as services as of type NodePort, LoadBalancer.

- Type: NodePort:

If you set the type field to NodePort while exposing the service, the Kubernetes master will allocate a port from a range specified by --service-node-port-range flag (default: 30000-32767), and each Node will proxy that port (the same port number on every Node) into your Service.

.. code-block:: bash

   $ kubectl expose deployment flask –type=NodePort –port=5000
   
Example: Nginx service

Flask app accessed by assigned nodeport port: 

- Type LoadBalancer:

On cloud providers which support external load balancers, setting the type field to LoadBalancer will provision a load balancer for your Service. The actual creation of the load balancer happens asynchronously, and information about the provisioned balancer will be published in the Service’s .status.loadBalancer field.

.. code-block:: bash

   $ kubectl describe svc flask
   
The above command lists Endpoints of the services where the service can be accessed internal to the cluster. Also shows the LoadBalancer attached to the service.
To view the available services run, 

.. code-block:: bash

   $ kubect get services

A list of all kubernetes services running in the cluster will be shown.

- Ingress:

Ingress, added in Kubernetes v1.1, exposes HTTP and HTTPS routes from outside the cluster to services within the cluster.In Kubernetes, Ingress allows external users and client applications access to HTTP services. Ingress consists of two components: an Ingress Resource and an Ingress Controller:

- Ingress Resource is a collection of rules for the inbound traffic to reach Services. These are Layer 7 (L7) rules that allow hostnames (and optionally paths) to be directed to specific Services in Kubernetes.
- Ingress Controller acts upon the rules set by the Ingress Resource, typically via an HTTP or L7 load balancer. It is vital that both pieces are properly configured so that traffic can be routed from an outside client to a Kubernetes Service.

- Installing Nginx ingress controller:

.. code-block:: bash

   $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/mandatory.yaml

- Ingress for Flask Service:

.. code-block:: bash

   apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
     name: flask-ingress
   spec:
    rules:
     - http:
        paths:
         - path: /flask
           backend:
              serviceName: flask
              servicePort: 80

creating this resource allows us to access the flask-svc service via the /flask path. When describing the service, it lists the nodes and you can access the service with node-ip.

.. code-block:: bash

   $ kubectl create -f flask-ingress.yaml 
   $ kubectl apply -f flask-ingress.yaml
   
Verify that the ingress resource is created by:

.. code-block:: bash

   $ kubectl get ingress 
   
Test Ingress and default backend
You should now be able to access the web application by going to the EXTERNAL-IP/flask address with the path mentioned in the  flask ingress.
The ingress controller will provision an implementation specific loadbalancer that satisfies the ingress, as long as the services (s1, s2) exist. When it has done so, you will see the address of the loadbalancer at the Address field.

