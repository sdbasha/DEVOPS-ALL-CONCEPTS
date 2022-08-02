CKA 2021 Exam Objectives
=================================

These are the exam objectives you review and understand in order to pass the test.

CNCF Exam Curriculum repository
===========================================
Cluster Architecture, Installation, and Configuration 25%
============================================================
1.Manage role based access control

2.Lab: RBAC with Kubernetes in Minikube
3.Use kubeadm to install a basic cluster

4.Lab: Install Kubernetes On Ubuntu
5.Manage a highly available Kubernetes cluster

6.Options for Highly Available topology
7.Weaveworks Kubeadm HA cluster
8.Provision underlying infrastructure to deploy Kubernetes cluster

9.Peform a version upgrade on Kubernetes cluster using kubeadm

10.implment etcd backup and restore

11.Kubecon Europe 2020: Kubeadm deep dive

12.sample commands used during backup/restore/update of nodes

====================================================================================
13.Workloads & Scheduling – 15%
14.Understand deployments and how to perform rolling update and rollbacks
15.Use ConfigMaps and Secrets to configure applications
16.configure a pod with a configmap
17.configure a pod with a secret
18.Know how to scale applications
19.scaling a statefulset
20.scaling a replicaset
21.Understand the primitives used to create robust, self-healing, application deployments
22.Replicaset
23.Deployments
24.Statefulsets
25.Daemonset
26.Understand how resource limits can affect Pod scheduling
27.Awareness of manifest management and common templating tools
28.Kustomize
29.Kustomize Blog
30.manage kubernetes objects
31.Install service catalog using helm
32.Non-k8s.io resource: CNCF Kubecon video: An introduction to Helm - Bridget Kromhout, Microsoft & Marc Khouzam, City of Montreal
33.Non-k8s.io resource: External resource: templating-yaml-with-code
===============================================================================================
34.Services & Networking – 20%
35.Understand host networking configuration on the cluster nodes

36.Understand connectivity between Pods

37.The concept of Pods networking
38.Understand ClusterIP, NodePort, LoadBalancer service types and endpoints

39.service
40.Know how to use Ingress controllers and Ingress resources

41.Ingress concepts
42.Know how to configure and use CoreDNS

43.Choose an appropriate container network interface plugin

44.Kubernetes Networking Intro and Deep-Dive - Bowei Du & Tim Hockin, Google
45.Kubernetes and Networks: why is this so dang hard?
46.Kubecon Eu 2020 Tutorial: Communication Is Key - Understanding Kubernetes Networking - Jeff Poole, Vivint Smart Home
===================================================================================================================
47.Storage – 10%
48.Understand storage classes, persistent volumes
49.Understand volume mode, access modes and reclaim policies for volumes
50.Understand persistent volume claims primitive
51.Know how to configure applications with persistent storage
52.StorageClass, PersistentVolume, and PersitentVolumeClaim examples
==========================================================================================
53.Troubleshooting – 30%
54.Evaluate cluster and node logging
55.Understand how to monitor applications
56.Manage container stdout & stderr logs
57.Troubleshoot application failure
58.Pending or termintated pods
59.Troubleshoot cluster component failure
60.Troubleshoot networking
61.DNS troubleshooting





==========================================================================================================================================================================



RBAC-TUTORIAL::
===============

Role-based access control (RBAC) is a method of regulating access to computer or network resources based on the roles of individual users within your organization.
It is an approach that is used for restricting access to users and applications on the system/network. 
RBAC is used by Kubernetes for authorization, for example giving access to a user, adding/removing permissions and setting up rules.


***to provide acess to your kubernetes cluster and or to the namespace.......we may have admin or devoloper they may need different access.
to provide access to kubernetes namespaces(namespaes are the logical partitions)
**ROLE**
=======
role allows users to give a set of permissions using 
it give like resource rules and verbs::list , get , update

**CLUSTER-ROLE**
===============================
TO provide the access to the compleate cluster-level we uses  the cluster-role
it allows the access to resource, rules and verbs:: list , get , update

the dofference between role and cluster-role is cluster-role has access to cluster level whearas role has the access-leve to namespace


***to attach the role to users we need to give role-binding and cluster-binding
RBAC authorization uses the rbac.authorization.k8s.io API group to drive authorization decisions, 
allowing you to dynamically configure policies through the Kubernetes API.

Role example::

yaml << EOF

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
  
EOF

ClusterRole example::

yaml << EOF

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  # "namespace" omitted since ClusterRoles are not namespaced
  name: secret-reader
rules:
- apiGroups: [""]
  #
  # at the HTTP level, the name of the resource for accessing Secret
  # objects is "secrets"
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
  
 EOF
 
RoleBinding example:::


ymal << EOF

apiVersion: rbac.authorization.k8s.io/v1
# This role binding allows "jane" to read pods in the "default" namespace.
# You need to already have a Role named "pod-reader" in that namespace.
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
# You can specify more than one "subject"
- kind: User
  name: jane # "name" is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  # "roleRef" specifies the binding to a Role / ClusterRole
  kind: Role #this must be Role or ClusterRole
  name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io
  
EOF

ClusterRoleBinding example::


yaml <<

apiVersion: rbac.authorization.k8s.io/v1
# This cluster role binding allows anyone in the "manager" group to read secrets in any namespace.
kind: ClusterRoleBinding
metadata:
  name: read-secrets-global
subjects:
- kind: Group
  name: manager # Name is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
  
EOF


Creating a cluster with kubeadm::
=======================================

The kubeadm tool is good if you need:

A simple way for you to try out Kubernetes, possibly for the first time.
A way for existing users to automate setting up a cluster and test their application.
A building block in other ecosystem and/or installer tools with a larger scope.


Install Kubernetes on Ubuntu 18.04 LTS
==============================================
Step1: On All Machines ( Master & All nodes ):
===========================================
### INSTALL DOCKER & Configure 

sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update ; clear
sudo apt-get install -y docker-ce

sudo vi /etc/docker/daemon.json

{
	"exec-opts": ["native.cgroupdriver=systemd"]
}

sudo service docker restart


### INSTALL KUBEADM,KUBELET,KUBECTL

echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update ; clear
sudo apt-get install -y kubelet kubeadm kubectl	


Step2: On Master only:
===========================
sudo kubeadm init --ignore-preflight-errors=all

sudo mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

## install networking driver -- Weave/flannel/canal/calico etc... 

## below installs weave networking driver 

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

kubectl get nodes
kubectl get all --all-namespaces



Step3: On Nodes only:
=============================
copy the kubeadm join token from master & run it on all nodes
      
Ex: kubeadm join 10.128.15.231:6443 --token mks3y2.v03tyyru0gy12mbt \
       --discovery-token-ca-cert-hash sha256:3de23d42c7002be0893339fbe558ee75e14399e11f22e3f0b34351077b7c4b56
	   
	   
=======================================================================================================================================	  



Multi Master cluster setup using Kubeadm
=====================================================================================================================================
What is Kubeadm ?
Kubeadm is a tool built to provide kubeadm init and kubeadm join as best-practice “fast paths” for creating Kubernetes clusters. 
kubeadm performs the actions necessary to get a minimum viable cluster up and running. By design, it cares only about bootstrapping, not about provisioning machines.
With kubeadm, your cluster should pass Kubernetes Conformance tests.
You can find more details about the conformance tests at https://github.com/cncf/k8s-conformance

Pre-requisite
==============
For this demo, we will use 2 master and 2 worker node to create a multi master kubernetes cluster using kubeadm installation tool. 
Below are the pre-requisite requirements for the installation:

2 machines for master, ubuntu 16.04+, 2 CPU, 2 GB RAM, 10 GB storage
2 machines for worker, ubuntu 16.04+, 1 CPU, 2 GB RAM, 10 GB storage
1 machine for loadbalancer, ubuntu 16.04+, 1 CPU, 2 GB RAM, 10 GB storage
All machines must be accessible on the network. For cloud users - single VPC for all machines
sudo privilege
ssh access from loadbalancer node to all machines (master & worker).
ssh access can be given to any account. ssh through root is not mandatory
Note that we will not cover ssh setup between loadbalancer.

Below are my virtual machines on GCP - region - us-west1

image

Setting up loadbalancer
At this point we will work only on the loadbalancer node. In order to setup loadbalancer, you can leverage any loadbalancer utility of your choice. If you feel like using cloud based TCP loadbalancer, feel free to do so. There is no restriction regarding the tool you want to use for this purpose. For our demo, we will use HAPROXY as the primary loadbalancer.

What are we loadbalancing ?
We have 2 master nodes. Which means the user can connect to either of the 2 api-servers. The loadbalancer will be used to loadbalance between the 2 api-servers.

Now that we have some information about what we are trying to achieve, we can now start configuring our loadbalancer node.

Login to the loadbalancer node

Switch as root - sudo -i

Update your repository and your system

sudo apt-get update && sudo apt-get upgrade -y

Install haproxy
sudo apt-get install haproxy -y
Edit haproxy configuration
vi /etc/haproxy/haproxy.cfg
Add the below lines to create a frontend configuration for loadbalancer -

frontend fe-apiserver
   bind 0.0.0.0:6443
   mode tcp
   option tcplog
   default_backend be-apiserver
Add the below lines to create a backend configuration for master1 and master2 nodes at port 6443. Note : 6443 is the default port of kube-apiserver

backend be-apiserver
   mode tcp
   option tcplog
   option tcp-check
   balance roundrobin
   default-server inter 10s downinter 5s rise 2 fall 2 slowstart 60s maxconn 250 maxqueue 256 weight 100

       server master1 10.138.0.15:6443 check
       server master2 10.138.0.16:6443 check
Here - master1 and master2 are the names of the master nodes and 10.138.0.15 and 10.138.0.16 are the corresponding internal IP addresses.

Restart and Verify haproxy
systemctl restart haproxy
systemctl status haproxy
Ensure haproxy is in running status.

Run nc command as below -

nc -v localhost 6443
Connection to localhost 6443 port [tcp/*] succeeded!
Note If you see failures for master1 and master2 connectivity, you can ignore them for time being as we have not yet installed anything on the servers.

Install kubeadm,kubelet and docker on master and worker nodes
In this step we will install kubelet and kubeadm on the below nodes

master1
master2
worker1
worker2
The below steps will be performed on all the below nodes.

Log in to all the 4 machines as described above

Switch as root - sudo -i

Update the repositories

apt-get update
Turn off swap
swapoff -a 
Install kubeadm and kubelet
apt-get update && apt-get install -y apt-transport-https curl

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

apt-get update

apt-get install -y kubelet kubeadm

apt-mark hold kubelet kubeadm 

Install container runtime - docker
sudo apt-get update

sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io -y
Configure kubeadm to bootstrap the cluster
We will start off by initializing only one master node. For this demo, we will use master1 to initialize our first control plane.

Log in to master1
Switch to root account - sudo -i
Execute the below command to initialize the cluster -
kubeadm init --control-plane-endpoint "LOAD_BALANCER_DNS:LOAD_BALANCER_PORT" --upload-certs --pod-network-cidr=192.168.0.0/16 
Here, LOAD_BALANCER_DNS is the IP address or the dns name of the loadbalancer. I will use the dns name of the server, i.e. loadbalancer as the LOAD_BALANCER_DNS. In case your DNS name is not resolvable across your network, you can use the IP address for the same.

The LOAD_BALANCER_PORT is the front end configuration port defined in HAPROXY configuration. For this demo, we have kept the port as 6443.

The command effectively becomes -

kubeadm init --control-plane-endpoint "loadbalancer:6443" --upload-certs --pod-network-cidr=192.168.0.0/16 
image

Your output should look like below -

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join loadbalancer:6443 --token cnslau.kd5fjt96jeuzymzb \
    --discovery-token-ca-cert-hash sha256:871ab3f050bc9790c977daee9e44cf52e15ee37ab9834567333b939458a5bfb5 \
    --control-plane --certificate-key 824d9a0e173a810416b4bca7038fb33b616108c17abcbc5eaef8651f11e3d146

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use 
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join loadbalancer:6443 --token cnslau.kd5fjt96jeuzymzb \
    --discovery-token-ca-cert-hash sha256:871ab3f050bc9790c977daee9e44cf52e15ee37ab9834567333b939458a5bfb5 
The output consists of 3 major tasks -

Setup kubeconfig using -
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Setup new control plane (master) using
  kubeadm join loadbalancer:6443 --token cnslau.kd5fjt96jeuzymzb \
    --discovery-token-ca-cert-hash sha256:871ab3f050bc9790c977daee9e44cf52e15ee37ab9834567333b939458a5bfb5 \
    --control-plane --certificate-key 824d9a0e173a810416b4bca7038fb33b616108c17abcbc5eaef8651f11e3d146

Join worker node using
kubeadm join loadbalancer:6443 --token cnslau.kd5fjt96jeuzymzb \
    --discovery-token-ca-cert-hash sha256:871ab3f050bc9790c977daee9e44cf52e15ee37ab9834567333b939458a5bfb5 
NOTE

Your output will be different than what is provided here. While performing the rest of the demo, ensure that you are executing the command provided by your output and dont copy and paste from here.

Save the output in some secure file for future use.

Log in to master2
Switch to root - sudo -i
Check the command provided by the output of master1
You can now use the below command to add another node to the control plane -

kubeadm join loadbalancer:6443 --token cnslau.kd5fjt96jeuzymzb \
    --discovery-token-ca-cert-hash sha256:871ab3f050bc9790c977daee9e44cf52e15ee37ab9834567333b939458a5bfb5 \
    --control-plane --certificate-key 824d9a0e173a810416b4bca7038fb33b616108c17abcbc5eaef8651f11e3d146

Execute the kubeadm join command for control plane on master2
image

Your output should look like -

 This node has joined the cluster and a new control plane instance was created:

* Certificate signing request was sent to apiserver and approval was received.
* The Kubelet was informed of the new secure connection details.
* Control plane (master) label and taint were applied to the new node.
* The Kubernetes control plane instances scaled up.
* A new etcd member was added to the local/stacked etcd cluster.

Now that we have initialized both the masters - we can now work on bootstrapping the worker nodes.

Log in to worker1 and worker2
Switch to root on both the machines - sudo -i
Check the output given by the init command on master1 to join worker node -
kubeadm join loadbalancer:6443 --token cnslau.kd5fjt96jeuzymzb \
    --discovery-token-ca-cert-hash sha256:871ab3f050bc9790c977daee9e44cf52e15ee37ab9834567333b939458a5bfb5 
Execute the above command on both the nodes -

Your output should look like -

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Configure kubeconfig on loadbalancer node
Now that we have configured the master and the worker nodes, its now time to configure Kubeconfig (.kube) on the loadbalancer node. It is completely up to you if you want to use the loadbalancer node to setup kubeconfig. kubeconfig can also be setup externally on a separate machine which has access to loadbalancer node. For the purpose of this demo we will use loadbalancer node to host kubeconfig and kubectl.

Log in to loadbalancer node
Switch to root - sudo -i
Create a directory - .kube at $HOME of root
mkdir -p $HOME/.kube
SCP configuration file from any one master node to loadbalancer node
scp master1:/etc/kubernetes/admin.conf $HOME/.kube/config

NOTE

If you havent setup ssh connection, you can manually download the file /etc/kubernetes/admin.conf from any one of the master and upload it to $HOME/.kube location on the loadbalancer node. Ensure that you change the file name as just config on the loadbalancer node.

Provide appropriate ownership to the copied file
chown $(id -u):$(id -g) $HOME/.kube/config
image

Install kubectl binary
snap install kubectl --classic
Verify the cluster
kubectl get nodes 

NAME      STATUS     ROLES    AGE     VERSION
master1   NotReady   master   21m     v1.16.2
master2   NotReady   master   15m     v1.16.2
worker1   NotReady   <none>   9m17s   v1.16.2
worker2   NotReady   <none>   9m25s   v1.16.2

Install CNI and complete installation
From the loadbalancer node execute -

kubectl apply -f https://docs.projectcalico.org/v3.8/manifests/calico.yaml

This installs CNI component to your cluster.

You can now verify your HA cluster using -

kubectl get nodes 

NAME      STATUS   ROLES    AGE   VERSION
master1   Ready    master   22m   v1.16.2
master2   Ready    master   17m   v1.16.2
worker1   Ready    <none>   10m   v1.16.2
worker2   Ready    <none>   10m   v1.16.2



==================================================================================

Peform a version upgrade on Kubernetes cluster using kubeadm::
=================================================================
1.upgrade the metadata for apt; determine which versionn to upgrade
     a. sudo apt update
     b. sudo apt-cache madison kubeadm
2. update the kubeadm packege
	a. sudo apt-mark unhold kubeadm
	b. sudo apy-get install -y kubeadm=1.20.1.00
	c. sudo apt-mark hold kubeadm && kubeadm version
3.draining nodes to evict worklads
	a. kubectl drain k8smaster --ignore-daemonsets
4.verify the upgrade plan
	a. sudo kubeadm upgrade plan
5.upgrade cluster, also upgrade kubelete and kubectl
	a. sudo kubeadm upgrade apply v1.20.1.-00
	b. kubectl get install kubelete=1-20.1-00
	c. sudo apt-get install kubelete=1.20.1-00 kubelete=1.20.1-00
	d. sudo apt-mark hold kubelete and kubectl
6.restart kubelete, and uncordon the node
	a. sudo systemctl deamon-reload && sudo systemctl restart kubelete
	b. kubectl uncordon master-node


===================================================================================
ETCD-BACKUP
===========================================
sudo etcd version
sudo apt install etcd-client
sudo ETCDCTL_API=3 etcdctl version
sudo ETCDCTL_API=3 etcdctl snapshot

========================================

kubectl -n kube-system get pods -A  -o wide | grep etcd
kubectl -n kube-system describe pod etcd-masternode


sudo ETCDCTL_API=3 etcdctl --endpoints 10.2.0.9:2379 \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  member list



sudo ETCDCTL_API=3 etcdctl --endpoints 10.2.0.9:2379 \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  snapshot save etcd-save-file-$(date +%m-%d-%y)



ETCDCTL_API=3 etcdctl --write-out=table snapshot status etcd-save-file-$(date +%m-%d-%y)



===========================================================================================================================


WHAT IS DEPLOYMENT::

Deployments are upgraded and higher version of replication controller. 
They manage the deployment of replica sets which is also an upgraded version of the replication controller.
They have the capability to update the replica set and are also capable of rolling back to the previous version.

They provide many updated features of matchLabels and selectors. 
We have got a new controller in the Kubernetes master called the deployment controller which makes it happen. 
It has the capability to change the deployment midway.

rolling upadate::

kubectl apply -f deployment.yaml --record=true
kubectl rolout status deployment deployment-name

kubectl rollout history deployment deployment-name------this will shows the records of the rolling updates
***kubectl rollout undu deployment deployment-name
Updating the Deployment
$ kubectl set image deployment/Deployment tomcat=tomcat:6.0

==================================================================================

DEPLOYMENT STRATEGIES::

1. Recreate
2. Ramped---Rolling update
3. Blue/Green
4. Canary
5. A/B testing





Recreate::
This strategy of deployment in kubernetes is a dummy deployment which consists of shutting down version A 
then deploying version B after version A is turned off completely. 
This technique implies downtime of the service that depends on both shutdown and boot duration of the application. Manifest looks like:

yaml >>spec:
      replicas: 3
      strategy:
      type: Recreate
      template:


Pros::

When it comes to easy setup, Recreate is a good option.
Application state entirely renewed.
Cons::

High impact on the user, expect downtime that depends on both shutdown and boot duration of the application.


Ramped----Rolling update::

Ramped deployment strategy is the default strategy in Kubernetes. 
In this, pods of the previous version are slowly replaced one by one by pods of the new version without any cluster downtime. 
Also,this is one of the best Deployment strategies in Kubernetes 
that is only applicable when your service is horizontally scaled means running more than one instance. Once an instance is in ready state- traffic gets sent on it.
Manifest looks like:

yaml << EOF
      spec:
      replicas: 3
      strategy:
      type: RollingUpdate
      rollingUpdate:
      maxSurge: 2 # how many pods we can add at a time
      maxUnavailable: 0 # maxUnavailable define how many pods can be unavailable
     # during the rolling update
     template:

EOF

Pros::
Slow-release of version across instances.
Useful for stateful applications that can handle the data.
Cons::
In this strategy, rollout and rollback take time.
There is no control over the traffic.



Blue/Green::

This Kubernetes Deployment strategy varies from a ramped deployment strategy, 
green (version B) is deployed alongside blue (version A) with an equal number of instances. 
If the new version is working fine and meets all requirements after testing the traffic is shifted from version A to 
version B at the load balancer level. Mainfest looks like:


apiVersion: v1
kind: Service
metadata:
name: my-app
labels:
app: my-app
spec:
type: NodePort
ports:
- name: http
port: 8080
targetPort: 8080 # Note here that we match both the app and the version.
# When switching traffic, we update the label “version” with
selector: # the appropriate value, ie: v2.0.0
app: my-app

Pros::

In this, rollout and rollback are fast.
Versioning issues are fixed as a whole application state is changed completely.
Cons::

The cost factor, as this strategy requires double the resources.
Required proper testing of application before deploying it to the production.


Canary::

In this Kubernetes deployment strategy, traffic is split based on weight. 
A canary deployment consists of slowly moving production traffic from version A to version B.
Let’s understand this with the help of an example:
Suppose 90 percent of the traffic goes to version A, 10 percent goes to version B. 
When there is no proper stability of new application on the platform and application is lacking testing then this strategy is beneficial. 
Canary release is to test any potential problem before it impacts the whole production server. 
In the following manifest, we use 2 replicasets for version A and 1 replicaset for version B.

spec:
replicas: 3 # for version A
spec:
replicas: 1 # for version B
This is a native Kubernetes Deployment strategy, where we update the number of replicas to adjust the traffic over the versions. 
If the impact of release is unknown, then canay deployment strategy is suggested.



Pros::

In this strategy, rollback is fast.
Convenient for monitoring of application performance and error rates.

Cons::

Rollout is slow in this strategy.
Traffic distribution is costly sometimes.



A/B Testing::

This another one of the best Deployment strategies in Kubernetes works like another variant of canary deployment 
where A/B testing is only used for features like front-end, unlike canary which deals with the backend. 
Rather than launch the feature publicly, it only releases to a specific set of users. 
When it comes to a business-related decision in statistics rather than a deployment here A/B testing plays an important role.

Pros::

With this deployment strategy, you can run several versions in parallel.
Useful for performance monitoring and checking error rates within the application.
Rollbacks are faster with this strategy.

Cons::

Rollouts are slow with this strategy.
Traffic distribution can be expensive.

=================================
POD::

A pod is a collection of containers and its storage inside a node of a Kubernetes cluster. 
It is possible to create a pod with multiple containers inside it. For example, keeping a database container and data container in the same pod.

A pod is a abstraction layer of a container which manges the container and its network.


kubectl run busybox --image=busybox --dry-run=client -o yaml --restart=Never > yamlfile.yaml

kubectl create job my-job --dry-run=client -o yaml --image=busybox -- date  > yamlfile.yaml



COFIGMAP::

A Kubernetes ConfigMap is an API object that allows you to store data as key-value pairs. 
Kubernetes pods can use ConfigMaps as configuration files, environment variables or command-line arguments

It uses the external configurations for example it consist of database urls and volume urls and environment variables.



secret::

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. 
Secrets are similar to ConfigMaps but are specifically intended to hold confidential data.


STATEFULLSETS::

StatefulSet is the workload API object used to manage stateful applications(databases).

Manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

Like a Deployment, a StatefulSet manages Pods that are based on an identical container spec. Unlike a Deployment, 
a StatefulSet maintains a sticky identity for each of their Pods.



When should I use StatefulSet?
When to use StatefulSet::

A Redis pod that has access to a volume, but you want it to maintain access to the same volume even if it is redeployed or restarted.
A Cassandra cluster and have each node maintain access to its data.
A webapp that needs to communicate with its replicas using known predefined network identifiers.


kubectl get statefulsets <stateful-set-name>
kubectl scale statefulsets <stateful-set-name> --replicas=<new-replicas>
kubectl edit statefulsets <stateful-set-name>
kubectl patch statefulsets <stateful-set-name> -p '{"spec":{"replicas":<new-replicas>}}'
=======================================================================================================================

REPLICASETS::

A ReplicaSet is a process that runs multiple instances of a Pod and keeps the specified number of Pods constant. 
Its purpose is to maintain the specified number of Pod instances running in a cluster 
at any given time to prevent users from losing access to their application when a Pod fails or is inaccessible




**The key difference between the replica set and the replication controller is, 
the replication controller only supports equality-based selector whereas the replica set supports set-based selector.


kubectl autoscale rs frontend --max=10 --min=3 --cpu-percent=50



EAMONSETS::

A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. 
As nodes are added to the cluster, Pods are added to them. 
As nodes are removed from the cluster, those Pods are garbage collected. 
Deleting a DaemonSet will clean up the Pods it created.

uses::
To run a daemon for node monitoring on every note, such as Prometheus Node Exporter, collectd, or Datadog agent.

Monitoring Exporters: 
You would want to monitor all the nodes of your cluster so you will need to run a monitor on all the nodes of the cluster like NodeExporter.
Logs Collection Daemon: 
You would want to export logs from all nodes so you would need a DaemonSet of log collector like Fluentd to export logs from all your nodes.




=========================================================================


kustomize is a tool to customize the configuration files and thair labels and also we can build pods, deployments for specific environments.

kustomize build

tree daigram for to customize the configuration files

 someapp/
   ├── base/
   │   ├── kustomization.yaml
   │   ├── deployment.yaml
   │   ├── configMap.yaml
   │   └── service.yaml
   └── overlays/
      ├── production/
      │   └── kustomization.yaml
      │   ├── replica_count.yaml
      └── staging/
          ├── kustomization.yaml
          └── cpu_count.yaml


commonLabels:
    env: production
   bases:
   - ../../base
   patches:
   - replica_count.yaml



apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: the-deployment
   spec:
     replicas: 100


============================================================================


Managing Kubernetes Objects Using Imperative Commands::

run: Create a new Pod to run a Container.
expose: Create a new Service object to load balance traffic across Pods.
autoscale: Create a new Autoscaler object to automatically horizontally scale a controller, such as a Deployment.


kubectl create service nodeport <myservicename>


scale: Horizontally scale a controller to add or remove Pods by updating the replica count of the controller.
annotate: Add or remove an annotation from an object.
label: Add or remove a label from an object.
The kubectl command also supports update commands driven by an aspect of the object. Setting this aspect may set different fields for different object types:

set <field>: Set an aspect of an object.


get: Prints basic information about matching objects. Use get -h to see a list of options.
describe: Prints aggregated detailed information about matching objects.
logs: Prints the stdout and stderr for a container running in a Pod


kubectl create service clusterip my-svc --clusterip="None" -o yaml --dry-run=client | kubectl set selector --local -f - 'environment=qa' -o yaml | kubectl create -f -

============================================================================================================================


KUBERNETES NETWORKING::

===================================================================

1.cantainer to container comunication
2.pod to pod comunication 
3.pod to service comunication
4.external to service comunication

Networking mainly deals with communicationn between pods, services, nodes and externel-services

container to codntainer::

pods are basic units of kubernetes, so pods conatins one or more containers that allocated on same host and confgiure to share same network
containers in a pod can reach on local-host for example if a pod has 5 containers all the containers will communicate with local-host
docker can start a container rather than create new virtual interface for it shares rhe existing interface so both the containers have same virtual interface and
both are communicate with shared virtual interface like that the container to container communication happen.
and also pod consist of pause conatainer which purpose is to provide network interface to other conatainers and when u use the pause command it suspends the current process
untill signal is recieved.


pod to pod::

Two pods are communicate with other using intranode-network and internode-network, componets of pod network are 
root network namespace , pod network namespace, linux network bridge
each pod has it s own network with a virtual ethernet pair connecting it to the root network and to enable pods to with each other the linux ethernet bridge is used


intranode-network::
is the basically communication between two diffrent pods on same node
the pod1 packet leaves from pod network and enters to root network virtual ethernet and discovers the destination using arp request
and it has ip now bridges knows where to forward the packet to pod2 and crosses the pipe pair and reaches the pod2

internode-network::
is the basically communication between two different pods in two diffrent nodes
so this is enable by routing as we know every node is assigned a cidr block for the pod-ips and each pos has its own ip bcz doesn't conflict on other node pods
as cloud provider routes the traffic the same thing is also created by network-plugins.

Nodes located in different pods communicate through this method. Let us take this example to help us understand this communication method better.
 Assume there are four pods networks: 
Pod ‘A’, pod ‘B’, pod ‘C’, and pod ‘D’. 
Suppose we have two root networks: 
Pods ‘A’ and ‘B’ are located in the first network and pods ‘C’ and ‘D’ in the second.

If you want to transfer a packet from pod ‘A’ to pod ‘D’, in Linux networking, for instance, this is what happens.

The packet leaves pod ‘A’ network and goes into the root network through veth0. 
This packet has to pass through the Linux bridge to help it find its destination. 
The node has no goal within its pod, so it is sent back to interface eth0. It then leaves the first node for the routing table.

The objective of the routing table is to route the packet to the required node. 
This node is located in pod ‘D’. Before reaching the bridge, the packet first passes through node 2, and is directed to its intended destination.


Pod-to-service networking::
Like containers, pods are ephemeral. Containers can go down at any moment, 
which may lead to the loss of all data stored in them. Same way, 
pods have IP addresses that may disappear unexpectedly. 
When this happens, Kubernetes replaces the lost IP addresses with new ones. 
So, each pod gets allocated a single IP address.

An abstraction layer set on top of pods solves this problem. 
It helps communicate with the pods and eliminates the need to track pods as they start, die, 
and get rescheduled without affecting the application.

Kubernetes services handle the abstraction layer. 
These services assign a single IP address to each set of pods. 
This enables the routing of traffic addressed to a pod through the virtual IP address of the service. 
As a result, Kubernetes overcomes the dynamic nature of pods networking and blocks unhealthy and dying pods from receiving traffic.


================================================================================================================================================


SERVICE::

service is static ip and permenant ip for the pods
also provides load-balancing
loos coupling within the cluster and ouside of the cluster.


types::

1.clusterip
2.headless
3.loadbalancer
4.nodeport
5.externalName- ingress



1. ClusterIP::

ClusterIP is the default and most common service type.
Kubernetes will assign a cluster-internal IP address to ClusterIP service. This makes the service only reachable within the cluster.
You cannot make requests to service (pods) from outside the cluster.
You can optionally set cluster IP in the service definition file.
Use Cases
Inter service communication within the cluster. For example, communication between the front-end and back-end components of your app.


2. NodePort::

NodePort service is an extension of ClusterIP service. A ClusterIP Service, to which the NodePort Service routes, is automatically created.
It exposes the service outside of the cluster by adding a cluster-wide port on top of ClusterIP.
NodePort exposes the service on each Node’s IP at a static port (the NodePort). Each node proxies that port into your Service. So, external traffic has access to fixed port on each Node. It means any request to your cluster on that port gets forwarded to the service.
You can contact the NodePort Service, from outside the cluster, by requesting <NodeIP>:<NodePort>.
Node port must be in the range of 30000–32767. Manually allocating a port to the service is optional. If it is undefined, Kubernetes will automatically assign one.
If you are going to choose node port explicitly, ensure that the port was not already used by another service.
Use Cases
When you want to enable external connectivity to your service.
Using a NodePort gives you the freedom to set up your own load balancing solution, to configure environments that are not fully supported by Kubernetes, or even to expose one or more nodes’ IPs directly.
Prefer to place a load balancer above your nodes to avoid node failure.


3. LoadBalancer::

LoadBalancer service is an extension of NodePort service. NodePort and ClusterIP Services, to which the external load balancer routes, are automatically created.
It integrates NodePort with cloud-based load balancers.
It exposes the Service externally using a cloud provider’s load balancer.
Each cloud provider (AWS, Azure, GCP, etc) has its own native load balancer implementation. The cloud provider will create a load balancer, which then automatically routes requests to your Kubernetes Service.
Traffic from the external load balancer is directed at the backend Pods. The cloud provider decides how it is load balanced.
The actual creation of the load balancer happens asynchronously.
Every time you want to expose a service to the outside world, you have to create a new LoadBalancer and get an IP address.
Use Cases
When you are using a cloud provider to host your Kubernetes cluster.
This type of service is typically heavily dependent on the cloud provider.


4. ExternalName::

Services of type ExternalName map a Service to a DNS name, not to a typical selector such as my-service.
You specify these Services with the `spec.externalName` parameter.
It maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value.
No proxying of any kind is established.
Use Cases
This is commonly used to create a service within Kubernetes to represent an external datastore like a database that runs externally to Kubernetes.
You can use that ExternalName service (as a local service) when Pods from one namespace to talk to a service in another namespace.



5.Headless::

A headless service is a service with a service IP but instead of load-balancing it will return the IPs of our associated Pods. 
This allows us to interact directly with the Pods instead of a proxy

you need to define a headless service for stateful applications. The definition of headless service is similar to the standard service, but it doesn't have the clusterIP. 
Simply adding clusterIP: none in service definition creates a headless service.


Core-dns::

CoreDNS is a flexible, extensible DNS server that can serve as the Kubernetes cluster DNS. ... 
You can use CoreDNS instead of kube-dns in your cluster by replacing kube-dns in an existing deployment, 
or by using tools like kubeadm that will deploy and upgrade the cluster for you.

What is the use of CoreDNS?

DNS server stores record in its database and answers domain name query using the database. 
If the DNS server doesn't have this data, it tries to find for a solution from other DNS servers. 
CoreDNS became the default DNS service for Kubernetes 1.13+ onwards.

kubectl get pods -n kube-system --all

CoreDNS is a DNS server that is modular and pluggable, 
and each plugin adds new functionality to CoreDNS. 
This can be configured by maintaining a Corefile, which is the CoreDNS configuration file. As a cluster administrator, you 
can modify the ConfigMap for the CoreDNS Corefile to change how DNS service discovery behaves for that cluster.




INGRESS-AND-INGRESS-CONTROLLER
========================================================================

Kubernetes Ingress is an API object that provides routing rules to manage external users' access to the services in a Kubernetes cluster.


In production environments, you typically need content-based routing, support for multiple protocols, and authentication. 
Ingress allows you to configure and manage these capabilities inside the cluster. 

Ingress is made up of an Ingress API object and the Ingress Controller. 
As we have discussed, 
Kubernetes Ingress is an API object that describes the desired state for exposing services to the outside of the Kubernetes cluster. 
An Ingress Controller is essential because it is the actual implementation of the Ingress API. 
An Ingress Controller reads and processes the Ingress Resource information and usually runs as pods within the Kubernetes cluster.



An Ingress provides the following:

Externally reachable URLs for applications deployed in Kubernetes clusters
Name-based virtual host and URI-based routing support
Load balancing rules and traffic, as well as SSL termination




What is the Ingress Controller?
If Kubernetes Ingress is the API object that provides routing rules to manage external access to services, 
Ingress Controller is the actual implementation of the Ingress API. 
The Ingress Controller is usually a load balancer for routing external traffic to your Kubernetes cluster and is responsible for L4-L7 Network Services. 

Layer 4 (L4) refers to the connection level of the OSI network stack—external connections load-balanced in a round-robin manner across pods. 
Layer 7 (L7) refers to the application level of the OSI stack—external connections load-balanced across pods, based on requests. 
Layer 7 is often preferred, but you should select an Ingress Controller that meets your load balancing and routing requirements.

Ingress Controller is responsible for reading the Ingress Resource information and processing that data accordingly. The following is a sample Ingress Resource:


YAML >>
   apiVersion: networking.k8s.io/v1beta1
kind: Ingress
spec:
  backend:
    serviceName: ServiceName
    servicePort: <Port Number>




PERSISTANCE-VOLUME
======================================================

Kubernetes Persistent Storage offers Kubernetes applications a convenient way to request, 
and consume, storage resources. 
A Volume is a basic building block of the Kubernetes storage architecture. 
Kubernetes Persistent Volumes are a type of Volume that lives within the Kubernetes cluster, 
and can outlive other Kubernetes pods to retain data for long periods of time.



Containers are immutable, meaning that when a container shuts down, 
all data created during its lifetime is lost. 
This is suitable for some applications, but in many cases, 
applications need to preserve state or share information with other applications. 
A common example is applications that rely on databases (see our post on MySQL Kubernetes). For these and other use cases, 
there is a need for containers to have a place to store information persistently—so it can survive the shutdown of one or more containers.



What are Kubernetes Persistent Volumes?Kubernetes persistent volumes (PVs) are a unit of storage provided 

by an administrator as part of a Kubernetes cluster. Just as a node is a compute resource used by the cluster, 
a PV is a storage resource.Persistent volumes are independent of the lifecycle of the pod that uses it, meaning that even if the pod shuts down, 
the data in the volume is not erased. They are defined by an API object,
 which captures the implementation details of storage such as NFS file shares, or specific cloud storage systems. 



In order for a Pod to start using these volumes, it must request a volume by issuing a persistent volume claim (PVC). 
PVCs describe the storage capacity and characteristics a pod requires, 
and the cluster attempts to match the request and provision the desired persistent volume.


PersistentVolumeClaim (PVC)::

This is a request sent by a Kubernetes node for storage. 
The claim can include specific storage parameters required by the application—for example an amount of storage, 
or a specific type of access (read/write, read-only, etc.). 
Kubernetes looks for a PV that meets the criteria defined in the user’s PVC, 
and if there is one, it matches claim to PV. This is called binding. 
You can also configure the cluster to dynamically provision 
a PV for a claim.StorageClassThe StorageClass object allows cluster administrators to define PVs with different properties, 
like performance, size or access parameters. It lets you expose persistent storage to users while abstracting the details of storage implementation. 

There are many predefined StorageClasses in Kubernetes (see the following section), 
or you can create your own.Administrators can define several StorageClasses that give users multiple options for performance. 
For example, one can be on a fast SSD drive but with limited capacity, and one on a slower storage service which provides high capacity.

Persistent Volume Life Cycle::
A PV is a cluster resource and a PVC is a request for a PV resource. 
The interaction between PVs and PVCs follows a distinct lifecycle, 
starting with provisioning and including binding, using, and reclaiming.
ProvisioningHere are the two main types of provisioning:

Static - 
PVs created by a cluster administrator. These PVs are located in 
the Kubernetes API and contain information about storage resources available 
to cluster users.

Dynamic - 
to meet PVC demands, clusters can attempt to dynamically provision a volume.
 This typically occurs when available static PVs do not match the PVC. In this case, 
the cluster can optionally be configured to use the default StorageClasses.

BindingHere is a quick rundown of how the binding process works:
A user creates a PVC request, specifying a certain amount of storage and the 
required access modes.A control loop, located in the master, looks for new PVCs, 
and then:

Static - 
the control loop attempts to find a matching PV and then binds it to the PVC.

Dynamic - 
if a PV was already dynamically provisioned for the PVC, the control loop binds them together.
After a PVC and a PV are bound, they remain exclusive.A PVC to PV binding is a one-to-one mapping. 
The process uses a ClaimRef, which creates a bi-directional binding between the PV and the PVC.
UsingPods use claims as volumes. Here is how the using process works:The cluster inspects the claim to find the bound 

volume and then mounts that volume.When using volumes that support multiple access modes, 

users can specify the desired mode.Once a user is provisioned a volume,
 the bound PV belongs to the user.It is also possible to schedule Pods. 
Users can schedule Pods and access a claimed PV by including a PersistentVolumeClaim section 
in the volumes block of the Pod.ReclaimingA reclaim policy for PVs tells the cluster what to 
do with the volume after the claim is released, and volumes can be Retained, Recycled, or Deleted.
 Once users do not need their volume anymore, they can delete the PVC objects from the API
 that allows reclamation of the resource.Types of Persistent VolumesKubernetes comes with numerous 
plugins that let you make different types of storage resources available to nodes in the Kubernetes cluster. 


These are implemented using the StorageClass object.
Here are some of the main plugins currently supported:
Cloud Storage and Virtualization	Proprietary Storage Platforms	Physical Drives / Storage Protocols

GCEPersistentDisk	Flocker	NFS

AWSElasticBlockStore	RBD (Ceph Block Device)	iSCSI

AzureFile	Cinder (OpenStack block storage)	FC (Fibre Channel)

AzureDisk	Glusterfs	
 
VsphereVolume	Flexvolume	 

 	Quobyte Volumes	 
 	Portworx Volumes	 
 	ScaleIO Volumes	 
 	StorageOS	



 


  
Persistent Volumes FeaturesKubernetes Persistent Volumes offer powerful capabilities. 
The most important are detailed below.
Capacity	
The capacity attribute lets you set the maximum storage capacity of the PV. 

Storage is specified in bytes, to ensure quantities are standard across all storage services and devices.

Volume Mode	
By default, Kubernetes creates a file system on the PV, but if desired, 
you can use a raw block device directly without an additional layer.

Access Modes 	
A PV can have the following access modes:●      
ReadWriteOnce—enables:: read and write and can be mounted by only one node●      
ReadOnlyMany—enables:: read only and can be mounted by multiple nodes (but not at the same time)●      
ReadWriteMany—both ::read and write, can be mounted by several nodes (not at the same time) Note: Different storage plugins may only support some of these access modes.
Reclaim Policy	::The reclaim policy specifies what happens when the node no longer needs the persistent storage. 
It can be set to Retain, meaning the PV is kept alive until it is explicitly deleted; Recycle, 
meaning the data is scrubbed but can be restored later; and Delete, meaning it is irreversibly deleted. Note: 
Different storage plugins may only support some of these reclamation policies.

Phase	::
A PV goes through the following lifecycle phases, which are visible to other entities in the cluster:●     
Available—--free for use, binding has not occured yet●      
Bound—--the PV was matched to a PersistentVolumeClaim and binding has occurred●      
Released—--the user deleted their PVC, but the PV is not yet reclaimed by the cluster●      
Failed—---the PV could not be reclaimed by the cluster automatically




YAML >>> PV
 apiVersion: v1
kind: PersistentVolume
metadata:
name: redis-pv
spec:
storageClassName: ""
capacity:
storage: 1Gi
accessModes:
- ReadWriteOnce
hostPath:
path: "/mnt/data"


YAML >>> PV-CLAIM
 apiVersion: v1
kind: PersistentVolumeClaim
metadata:
name: redisdb-pvc
spec:
storageClassName: ""
accessModes:
- ReadWriteOnce
resources:
requests:
storage: 1Gi


YAML >>>POD-USING-PVS

  apiVersion: v1
kind: Pod
metadata:
name: redispod
spec:
containers:
- image: redis
name: redisdb
volumeMounts:
- name: redis-data
mountPath: /data
ports:
- containerPort: 6379
protocol: TCP
volumes:
- name: redis-data
persistentVolumeClaim:
claimName: redisdb-pvc




Troubleshooting – 30%
=========================================


Evaluate cluster and node logging::

Application logs can help you understand what is happening inside your application. 
The logs are particularly useful for debugging problems and monitoring cluster activity. 
Most modern applications have some kind of logging mechanism. Likewise, container engines are designed to support logging.


yaml >>>
 apiVersion: v1
kind: Pod
metadata:
  name: counter
spec:
  containers:
  - name: count
    image: busybox
    args: [/bin/sh, -c,
            'i=0; while true; do echo "$i: $(date)"; i=$((i+1)); sleep 1; done']


kubectl logs counter



yaml >>>
 apiVersion: v1
kind: Pod
metadata:
  name: counter
spec:
  containers:
  - name: count
    image: busybox
    args:
    - /bin/sh
    - -c
    - >
      i=0;
      while true;
      do
        echo "$i: $(date)" >> /var/log/1.log;
        echo "$(date) INFO $i" >> /var/log/2.log;
        i=$((i+1));
        sleep 1;
      done      
    volumeMounts:
    - name: varlog
      mountPath: /var/log
  - name: count-log-1
    image: busybox
    args: [/bin/sh, -c, 'tail -n+1 -f /var/log/1.log']
    volumeMounts:
    - name: varlog
      mountPath: /var/log
  - name: count-log-2
    image: busybox
    args: [/bin/sh, -c, 'tail -n+1 -f /var/log/2.log']
    volumeMounts:
    - name: varlog
      mountPath: /var/log
  volumes:
  - name: varlog
    emptyDir: {}



kubectl logs counter count-log-1







Troubleshoot Applications::


kubectl describe pods ${POD_NAME}

My pod stays pending::

If a Pod is stuck in Pending it means that it can not be scheduled onto a node. 
Generally this is because there are insufficient resources of one type or another that prevent scheduling. 
Look at the output of the kubectl describe ... command above. 
There should be messages from the scheduler about why it can not schedule your pod. Reasons include:

You don't have enough resources: You may have exhausted the supply of CPU or Memory in your cluster, 
in this case you need to delete Pods, adjust resource requests, or add new nodes to your cluster. See Compute Resources document for more information.

You are using hostPort: When you bind a Pod to a hostPort there are a limited number of places that pod can be scheduled. In most cases, 
hostPort is unnecessary, try using a Service object to expose your Pod. 
If you do require hostPort then you can only schedule as many Pods as there are nodes in your Kubernetes cluste



My pod stays waiting::

If a Pod is stuck in the Waiting state, then it has been scheduled to a worker node, 
but it can't run on that machine. Again, the information from kubectl describe ... 
should be informative. The most common cause of Waiting pods is a failure to pull the image. There are three things to check:

Make sure that you have the name of the image correct.
Have you pushed the image to the registry?
Run a manual docker pull <image> on your machine to see if the image can be pulled


My pod is crashing or otherwise unhealthy::

Once your pod has been scheduled, the methods described in Debug Running Pods are available for debugging.

My pod is running but not doing what I told it to do
If your pod is not behaving as you expected, it may be that there was an error in your pod description 
(e.g. mypod.yaml file on your local machine), and that the error was silently ignored when you created the pod. 
Often a section of the pod description is nested incorrectly, or a key name is typed incorrectly, and so the key is ignored. 
For example, if you misspelled command as commnd then the pod will be created but will not use the command line you intended it to use.

The first thing to do is to delete your pod and try creating it again with the --validate option. For example, 
run kubectl apply --validate -f mypod.yaml. If you misspelled command as commnd then will give an error like this:
============================================================================================================================================
I0805 10:43:25.129850   46757 schema.go:126] unknown field: commnd
I0805 10:43:25.129973   46757 schema.go:129] this may be a false alarm, see https://github.com/kubernetes/kubernetes/issues/6842
pods/mypod
=============================================================================================================================================


Debugging Replication Controllers::

Replication controllers are fairly straightforward. They can either create Pods or they can't. 
If they can't create pods, then please refer to the instructions above to debug your pods.

You can also use kubectl describe rc ${CONTROLLER_NAME} to introspect events related to the replication controller.

Debugging Services::

Services provide load balancing across a set of pods. 
There are several common problems that can make Services not work properly. 
The following instructions should help debug Service problems.

First, verify that there are endpoints for the service. For every Service object, the apiserver makes an endpoints resource available.

You can view this resource with:

kubectl get endpoints ${SERVICE_NAME}::

Make sure that the endpoints match up with the number of pods that you expect to be members of your service. 
For example, if your Service is for an nginx container with 3 replicas, you would expect to see three different IP addresses in the Service's endpoints



My service is missing endpoints::

If you are missing endpoints, try listing pods using the labels that Service uses. Imagine that you have a Service where the labels are:

...
spec:
  - selector:
     name: nginx
     type: frontend

You can use:

kubectl get pods --selector=name=nginx,type=frontend
to list pods that match this selector. 
Verify that the list matches the Pods that you expect to provide your Service. Verify that the pod's containerPort matches up with the Service's targetPort



Kubernetes Node logging
When a container running on Kubernetes writes its logs to stdout or stderr streams, 
they are picked up by the kubelet service running on that node, and are delegated to 
the container engine for handling based on the logging driver configured in Kubernetes.

In most cases, Docker container logs will end up in the ""/var/log/containers"" 
directory on your host. Docker supports multiple logging drivers but, unfortunately, Kubernetes API does not support driver configuration.




$ journalctl -u docker --Logs begin at Wed 2019-05-29 10:59:24 CEST, 
end at Mon 2019-07-15 10:55:17 CEST. --jul 29 10:59:35 thinkpad systemd[1]: 
Starting Docker Application Container Engine...jul 29 10:59:35 thinkpad dockerd[2172]: 
time="2019-05-29T10:59:35.285765854+02:00" level=info msg="libcontainerd: 
started new docker-containerd process" pjul 29 10:59:35 thinkpad dockerd[2172]: 
time="2019-05-29T10:59:35.286021587+02:00" level=info msg="parsed scheme: \"unix\"" module=grpc



Kubernetes system components logging::
In addition to kubelet and kube-proxy node services we covered earlier, 
there are control plane components on the level of the Kubernetes cluster itself that can be logged, 
as well as additional data types that can be used (events, audit logs). 
Together, these different types of data can give you visibility into how Kubernetes is performing as a system.

The following are the main system components of Kubernetes control plane:

kube-apiserver – the API server serving as the access point to the cluster
kube-scheduler – the element that determines where to run containers
etcd – the key-value store used as Kubernetes’ cluster configuration storage
Some of these components run in a container, and some of them run on the operating system level (in most cases, a systemd service).

The systemd services write to journald, and components running in containers write logs to the ""/var/log"" directory, 
unless the container engine has been configured to stream logs differently.

Kubernetes’ system components use Kubernetes’::

logging library — klog — to generate their log messages. 
These system logs were not known to follow uniform structure, 
which made it difficult to parse, query and analyze. However, Kubernetes’ 
recent v1.19 release introduced a new option in klog for structured logging in text as well as in JSON format.

Structured logging provides a well-defined structure in klog native format, 
with a list of key-value pairs for the variant payload. Using the --logging-format=json flag enables JSON output.

It’s important to note that structured logging (both string and JSON options) is still in alpha per v1.19, 
with incremental adoption, so you may encounter early stage issues such as system logs that are still unstructured, 
log formatting changes, or klog flags which are supported for JSON. Check the documentation for updated feature status and information here.





Kubernetes events::
Kubernetes events can indicate any Kubernetes resource state changes and errors, 
such as exceeded resource quota or pending pods, as well as any informational messages.

The command kubectl get events -n <namespace> returns all events within a specific namespace:


Kubernetes audit logs::
Audit logs can be useful for compliance as they should help you answer the questions of what happened, who did what and when.

Kubernetes provides flexible auditing of kube-apiserver requests based on policies. 
These help you track all activities in chronological order.

Here is an example of an audit log-policy::

{  "kind":"Event",  "apiVersion":"audit.k8s.io/v1beta1",  
"metadata":{ "creationTimestamp":"2019-08-22T12:00:00Z" },  
"level":"Metadata",  "timestamp":"2019-08-22T12:00:00Z",  
"auditID":"23bc44ds-2452-242g-fsf2-4242fe3ggfes",  
"stage":"RequestReceived",  "requestURI":"/api/v1/namespaces/default/persistentvolumeclaims",  
"verb":"list",  "user": {  "username":"user@example.org",  
"groups":[ "system:authenticated" ]  },  
"sourceIPs":[ "172.12.56.1" ],  
"objectRef": {  "resource":"persistentvolumeclaims",  "namespace":"default",  "apiVersion":"v1"  },  
"requestReceivedTimestamp":"2019-08-22T12:00:00Z",  "stageTimestamp":"2019-08-22T12:00:00Z"}



kube-apiserver --audit-log-path=/var/log/kubernetes/apiserver/audit.log --audit-policy-file=/etc/kubernetes/audit-policies/policy.yaml


The Kubernetes API server will start logging to the specified audit.log file in JSON format

ELK Stack::
The ELK Stack (Elasticsearch, Logstash and Kibana) is another very popular open-source tool used for logging Kubernetes, 
and is actually comprised of four components:

Elasticsearch::
– provides a scalable, RESTful search and analytics engine for storing Kubernetes logs
Kibana – the visualization layer, allowing you with a user interface to query and visualize logs
Logstash – the log aggregator used to collect and process the logs before sending them into Elasticsearch
Beats – Filebeat and Metricbeat are ELK-native lightweight data shippers used for shipping log files and metrics into Elasticsearch
ELK can be deployed on Kubernetes as well, on-prem or in the cloud. 
While Beats is Elasticsearch’s native shipper, 
a common alternative for Kubernetes installations is to use Fluentd to send logs to Elasticsearch (sometimes referred to as the EFK stack).

Together, these components provide Kubernetes users with an end-to-end logging solution. 
As effective as it is, deploying and managing ELK deployments at scale is a challenge unto itself




KUBERNTES SNDOUT & STDERR::

A container engine handles and redirects any output generated to a containerized application's stdout and stderr streams. 
For example, the Docker container engine redirects those two streams to a logging driver, which is configured in Kubernetes to write to a file in JSON format.




kubectl get nodes
And verify that all of the nodes you expect to see are present and that they are all in the Ready state.

To get detailed information about the overall health of your cluster, you can run:

kubectl cluster-info dump
Looking at logs::
For now, digging deeper into the cluster requires logging into the relevant machines. 
Here are the locations of the relevant log files. (note that on systemd-based systems, you may need to use journalctl instead)

Master::
/var/log/kube-apiserver.log - API Server, responsible for serving the API
/var/log/kube-scheduler.log - Scheduler, responsible for making scheduling decisions
/var/log/kube-controller-manager.log - Controller that manages replication controllers
Worker Nodes
/var/log/kubelet.log - Kubelet, responsible for running containers on the node
/var/log/kube-proxy.log - Kube Proxy, responsible for service load balancing
A general overview of cluster failure modes
This is an incomplete list of things that could go wrong, and how to adjust your cluster setup to mitigate the problems.

Root causes:
VM(s) shutdown
Network partition within cluster, or between cluster and users
Crashes in Kubernetes software
Data loss or unavailability of persistent storage (e.g. GCE PD or AWS EBS volume)
Operator error, for example misconfigured Kubernetes software or application software
Specific scenarios:
Apiserver VM shutdown or apiserver crashing
Results
unable to stop, update, or start new pods, services, replication controller
existing pods and services should continue to work normally, unless they depend on the Kubernetes API
Apiserver backing storage lost
Results
apiserver should fail to come up
kubelets will not be able to reach it but will continue to run the same pods and provide the same service proxying
manual recovery or recreation of apiserver state necessary before apiserver is restarted
Supporting services (node controller, replication controller manager, scheduler, etc) VM shutdown or crashes
currently those are colocated with the apiserver, and their unavailability has similar consequences as apiserver
in future, these will be replicated as well and may not be co-located
they do not have their own persistent state
Individual node (VM or physical machine) shuts down
Results
pods on that Node stop running
Network partition
Results
partition A thinks the nodes in partition B are down; partition B thinks the apiserver is down. (Assuming the master VM ends up in partition A.)
Kubelet software fault
Results
crashing kubelet cannot start new pods on the node
kubelet might delete the pods or not
node marked unhealthy
replication controllers start new pods elsewhere
Cluster operator error
Results
loss of pods, services, etc
lost of apiserver backing store
users unable to read API





Traffic forwarding and bridging::
Kubernetes supports a variety of networking plugins and each one can fail in its own way.

At its core, Kubernetes relies on the Netfilter kernel module to set up low level cluster IP load balancing. 
This requires two critical modules, IP forwarding and bridging, to be on.

Kernel IP forwarding::
IP forwarding is a kernel setting that allows forwarding of the traffic coming from one interface to be routed to another interface.

This setting is necessary for Linux kernel to route traffic from containers to the outside world.

HOW THE FAILURE MANIFESTS ITSELF::
Sometimes this setting could be reset by a security team running periodic security scans/enforcements on the fleet, 
or have not been configured to survive a reboot. When this happens networking starts failing.

Pod to service connection times out:

* connect to 10.100.225.223 port 5000 failed: Connection timed out
* Failed to connect to 10.100.225.223 port 5000: Connection timed out
* Closing connection 0
curl: (7) Failed to connect to 10.100.225.223 port 5000: Connection timed out




sysctl net.ipv4.ip_forward
# 0 means that forwarding is disabled
net.ipv4.ip_forward = 0



sysctl -w net.ipv4.ip_forward=1
# on Centos this will make the setting apply after reboot
echo net.ipv4.ip_forward=1 >> /etc/sysconf.d/10-ipv4-forwarding-on.conf




Bridge-netfilter
The bridge-netfilter setting enables iptables rules to work on Linux bridges just like the ones set up by Docker and Kubernetes.

This setting is necessary for the Linux kernel to be able to perform address translation in packets going to and from hosted containers.

HOW THE FAILURE MANIFESTS ITSELF
Network requests to services outside the Pod network will start timing out with destination host unreachable or connection refused errors.

HOW TO DIAGNOSE
# check that bridge netfilter is enabled
sysctl net.bridge.bridge-nf-call-iptables

# 0 means that bridging is disabled
net.bridge.bridge-nf-call-iptables = 0
HOW TO FIX
# Note some distributions may have this compiled with kernel,
# check with cat /lib/modules/$(uname -r)/modules.builtin | grep netfilter
modprobe br_netfilter
# turn the iptables setting on
sysctl -w net.bridge.bridge-nf-call-iptables=1
echo net.bridge.bridge-nf-call-iptables=1 >> /etc/sysconf.d/10-bridge-nf-call-iptables.conf



Firewall rules block overlay network traffic::
Kubernetes provides a variety of networking plugins that enable its 
clustering features while providing backwards compatible support for traditional IP and port based applications.

One of most common on-premises Kubernetes networking setups leverages a VxLAN overlay network, where IP packets are encapsulated in UDP and sent over port 8472.

HOW THE FAILURE MANIFESTS ITSELF::
There is 100% packet loss between pod IPs either with lost packets or destination host unreachable.

$ ping 10.244.1.4 
PING 10.244.1.4 (10.244.1.4): 56 data bytes
^C--- 10.244.1.4 ping statistics ---
5 packets transmitted, 0 packets received, 100% packet loss
HOW TO DIAGNOSE
It is better to use the same protocol to transfer the data, as firewall rules can be protocol specific, e.g. could be blocking UDP traffic.

iperf could be a good tool for that:

#  on the server side
iperf -s -p 8472 -u
# on the client side 
iperf -c 172.28.128.103 -u -p 8472 -b 1K


Pod CIDR conflicts::
Kubernetes sets up special overlay network for container to container communication.

With isolated pod network, containers can get unique IPs and avoid port conflicts on a cluster. You can read more about Kubernetes networking model here.

The problems arise when Pod network subnets start conflicting with host networks.

HOW THE FAILURE MANIFESTS ITSELF
Pod to pod communication is disrupted with routing problems.

$ curl http://172.28.128.132:5000
curl: (7) Failed to connect to 172.28.128.132 port 5000: No route to host
HOW TO DIAGNOSE
Start with a quick look at the allocated pod IP addresses:

$ kubectl get pods -o wide
NAME                       READY     STATUS    RESTARTS   AGE       IP               NODE
netbox-2123814941-f7qfr    1/1       Running   4          21h       172.28.27.2      172.28.128.103
netbox-2123814941-ncp3q    1/1       Running   4          21h       172.28.21.3      172.28.128.102
testbox-2460950909-5wdr4   1/1       Running   3          21h       172.28.128.132   172.28.128.101
Compare host IP range with the kubernetes subnets specified in the apiserver:

$ ip addr list
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 52:54:00:2c:6c:50 brd ff:ff:ff:ff:ff:ff
    inet 172.28.128.103/24 brd 172.28.128.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::5054:ff:fe2c:6c50/64 scope link 
       valid_lft forever preferred_lft forever
                                                      

HOW TO FIX
Double-check what RFC1918 private network subnets are in use in your network, VLAN or VPC and make certain that there is no overlap.

Once you detect the overlap, update the Pod CIDR to use a range that avoids the conflict.





Troubleshooting Tools
Here is a list of tools that we found helpful while troubleshooting the issues above.

tcpdump
Tcpdump is a tool to that captures network traffic and helps you troubleshoot some common networking problems. 
Here is a quick way to capture traffic on the host to the target container with IP 172.28.21.3.

We are going to join the one container and will be trying to reach out another container:

kubectl exec -ti testbox-2460950909-5wdr4 -- /bin/bash
$ curl http://172.28.21.3:5000
curl: (7) Failed to connect to 172.28.21.3 port 5000: No route to host
On the host with a container we are going to capture traffic related to container target IP:

$ tcpdump -i any host 172.28.21.3
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on any, link-type LINUX_SLL (Linux cooked), capture size 262144 bytes
20:15:59.903566 IP 172.28.128.132.60358 > 172.28.21.3.5000: Flags [S], seq 3042274422, win 28200, options [mss 1410,sackOK,TS val 10056152 ecr 0,nop,wscale 7], length 0
20:15:59.903566 IP 172.28.128.132.60358 > 172.28.21.3.5000: Flags [S], seq 3042274422, win 28200, options [mss 1410,sackOK,TS val 10056152 ecr 0,nop,wscale 7], length 0
20:15:59.905481 ARP, Request who-has 172.28.21.3 tell 10.244.27.0, length 28
20:16:00.907463 ARP, Request who-has 172.28.21.3 tell 10.244.27.0, length 28
20:16:01.909440 ARP, Request who-has 172.28.21.3 tell 10.244.27.0, length 28
20:16:02.911774 IP 172.28.128.132.60358 > 172.28.21.3.5000: Flags [S], seq 3042274422, win 28200, options [mss 1410,sackOK,TS val 10059160 ecr 0,nop,wscale 7], length 0
20:16:02.911774 IP 172.28.128.132.60358 > 172.28.21.3.5000: Flags [S], seq 3042274422, win 28200, options [mss 1410,sackOK,TS val 10059160 ecr 0,nop,wscale 7], length 0
As you see there is a trouble on the wire as kernel fails to route the packets to the target IP.

Here is a helpful intro on tcpdump.    




kubectl get pods dnsutils

kubectl exec -i -t dnsutils -- nslookup kubernetes.default

kubectl exec -ti dnsutils -- cat /etc/resolv.conf


kubectl exec -i -t dnsutils -- nslookup kubernetes.default


kubectl exec -i -t dnsutils -- nslookup kubernetes.default


kubectl get pods --namespace=kube-system -l k8s-app=kube-dns

kubectl logs --namespace=kube-system -l k8s-app=kube-dns


Here is an example of a healthy CoreDNS log:

.:53
2018/08/15 14:37:17 [INFO] CoreDNS-1.2.2
2018/08/15 14:37:17 [INFO] linux/amd64, go1.10.3, 2e322f6
CoreDNS-1.2.2
linux/amd64, go1.10.3, 2e322f6
2018/08/15 14:37:17 [INFO] plugin/reload: Running configuration MD5 = 24e6c59e83cce706f07bcc82c31b1ea1c




kubectl get svc --namespace=kube-system


Are DNS endpoints exposed?
You can verify that DNS endpoints are exposed by using the kubectl get endpoints command.

kubectl get endpoints kube-dns --namespace=kube-system














KUBERNETES-CHEET-SHEET-COMMANDS
===============================================================================================

INSTALL KUBECTL
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

Make sure to place the downloaded binary to a fixed location, add the location to your PATH variable and make it executable with chmod +x command.

$ kubectl version --client
$ kubectl [command] [TYPE] [NAME] [flags]
====================================================================================================



Set Context and Configuration
Before using kubectl commands on a Kubernetes cluster, we have to set the configuration and context first. It can be done with kubectl command itself.

To view kubectl current configuration, use:

$ kubectl config view
=================================================
To list all available contexts:

$ kubectl config get-contexts
==========================================
To get current context for kubectl:

$ kubectl config current-context
===========================================
We can change the context in use by using:

$ kubectl config use-context [cluster-name]
=============================================================

To authorize a new user to be added in kubeconf:

$ kubectl config set-credentials NAME [--client-certificate=path/to/certfile] [--client-key=path/to/keyfile] [--token=bearer_token] [--username=basic_user] [--password=basic_password]
=================================================================================================================================================================================================

For example, to set only the “client-key” field on the “cluster-admin” without touching other values, we can use:

$ kubectl config set-credentials cluster-admin --client-key=~/.kube/admin.key
=========================================================================================================

Or, as another example, to set basic auth for, say “cluster-admin” entry, you can specify username and password as:

$ kubectl config set-credentials cluster-admin --username=[username] --password=[password]






====================================================================================================================


Creating Objects
kubectl is used to deploy different objects supported in a Kubernetes cluster. Its manifest can be defined in a YAML or JSON file with extension .yaml or .yml and .json respectively.

Using the given manifest file, we can create defined resources using the following command:

$ kubectl apply -f [manifest.yaml]
====================================================================================================================
Or to specify multiple YAML files, use:

$ kubectl apply -f [manifest1.yaml] [manifest2.yaml]
====================================================================================================================
To create a resource(s) in all manifest files present in a directory:

$ kubectl apply -f ./dir
====================================================================================================================
Or to create resources from a URL:

$ kubectl apply -f [URL]
====================================================================================================================
Or directly from image name from the repository as:

$ kubectl create deployment [deployment-name] --image=[image-name]
====================================================================================================================
For example, to deploy a single instance of Nginx web server:

$ kubectl create deployment nginx --image=nginx
====================================================================================================================
Finally, to deploy resources directly by typing the YAML contents in CLI without referring to an actual saved manifest file, try something like:

# Create multiple YAML objects from stdin
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
name: busybox-sleep
spec:
containers:
- name: busybox
image: busybox
args:
- sleep
- "1000000"
---
apiVersion: v1
kind: Pod
metadata:
name: busybox-sleep-less
spec:
containers:
- name: busybox
image: busybox
args:
- sleep
- "1000"
EOF
====================================================================================================================
View/Find Resources
kubectl provides get command to list down the deployed resources, get their details, and find out more about them.

To list all services in the default namespace, use:

$ kubectl get services
====================================================================================================================
Similarly, for listing pods in all the namespaces, the syntax will be:

$ kubectl get pods --all-namespaces
====================================================================================================================
If we need to list down more details of deployed pods, use -o wide flag as:

$ kubectl get pods -o wide
====================================================================================================================
Syntax to get deployment details goes like this:

$ kubectl get deployment [deployment-name]
====================================================================================================================
To get a pod’s YAML content, we can use -o yaml flag like:

$ kubectl get pod [pod-name] -o yaml
====================================================================================================================
Often we need to get details on Kubernetes resources. kubectl’s describe command helps in getting those details.

We can get more details about a node as:

$ kubectl describe nodes [node-name]
====================================================================================================================
Or similarly for pods as:

$ kubectl describe pods [pod-name]
====================================================================================================================
kubectl allows sorting the output based on a particular field. To list services sorted by service name, use:

$ kubectl get services --sort-by=.metadata.name
====================================================================================================================
Or to get all running pods in the namespace, we can try:

$ kubectl get pods --field-selector=status.phase=Running
====================================================================================================================
To fetch just the external IPs of all nodes, if assigned, we can use -o jsonpath flag with the below syntax:

$ kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'
====================================================================================================================
To fetch labels attached to a resource, say pods, try:

$ kubectl get pods --show-labels
====================================================================================================================
For getting a list of events but which is sorted by timestamp, we can use -sort-by flag as:

$ kubectl get events --sort-by=.metadata.creationTimestamp
====================================================================================================================
If we want to compares the current state of the cluster against the state that the cluster would be in if the manifest was applied, we use diff command as:

$ kubectl diff -f ./manifest-file.yaml
====================================================================================================================
Modifying Resources
Deployed resources would often be modified for any configuration changes.

To perform a rolling update of, say “www” containers of, say “frontend” deployment by updating their image, we can use:

$ kubectl set image deployment/frontend www=image:v2
====================================================================================================================
We can check the history of deployments including revision as:

$ kubectl rollout history deployment/frontend
====================================================================================================================
Or to rollback to a previous deployment, use:

$ kubectl rollout undo deployment/frontend
====================================================================================================================
We can also rollback to a specific revision by specifying --to-revision flag as:

$ kubectl rollout undo deployment/frontend --to-revision=2
====================================================================================================================
And to check rolling update status, we use:

$ kubectl rollout status -w deployment/frontend
====================================================================================================================
For rolling restart of, say “frontend” deployment, use:

$ kubectl rollout restart deployment/frontend
====================================================================================================================
We can specify a JSON manifest to replace a pod by passing it to standard input as shown below:

$ cat pod.json | kubectl replace -f -
====================================================================================================================
It may happen that you need to force replace, delete, and then re-create a resource (NOTE: this will also cause a service outage) which can be done as:

$ kubectl replace --force -f [manifest-file]
====================================================================================================================
Labeling a resource (which supports labels) is easy and can be done using:

$ kubectl label pods [pod-name] new-label=[label]
====================================================================================================================
Similarly, annotation can be added to a resource using:

$ kubectl annotate pods [pod-name] icon-url=[url]
====================================================================================================================
Autoscaling a deployment is possible with:

$ kubectl autoscale deployment [dep-name] --min=[min-val] --max=[max-val]
====================================================================================================================
Here, dep-name is the name of the deployment to be autoscaled and min-val and max-val denotes the minimum and maximum value to be used for auto-scaling.

Editing resources
It is possible to edit an API resource in your preferred editor with edit command.

$ kubectl edit [api-resource-name]
====================================================================================================================
Or to use your own alternative editor, specify KUBE_EDITOR like:

KUBE_EDITOR="nano" kubectl edit [api-resource-name]
====================================================================================================================
Scaling resources
Scaling resource is one of the features supported by Kubernetes and kubectl makes it easy to do so.

For scaling a replica set named, say foo to 3, we use:

$ kubectl scale --replicas=3 rs/foo
====================================================================================================================
Or instead, we can refer to a manifest YAML file to specify the resource to be scaled as:

$ kubectl scale --replicas=3 -f foo.yaml
====================================================================================================================
We can additionally perform scaling based on the current state of deployment as:

$ kubectl scale --current-replicas=2 --replicas=3 deployment/nginx
====================================================================================================================
Deleting resources
Created resources will eventually need some modifications or deletion. With kubectl, we can delete existing resources in several ways.

To delete a pod using the specification from the JSON file, we use:

$ kubectl delete -f ./pod.json
====================================================================================================================
We can delete pods and services with the same names pod-name and service-name as:

$ kubectl delete pod,service [pod-name] [service-name]
====================================================================================================================
If the resources are labeled and we need to delete resources with a specific label, say label-name, we can use:

$ kubectl delete pods,services -l name=[label-name]
====================================================================================================================
To delete every pods and service contained in a namespace, use:

$ kubectl -n [namespace] delete pod,svc --all
====================================================================================================================
Interacting with running Pods
We can use kubectl to get details about running pods that helps administer a Kubernetes cluster.

One of the common commands is to get logs of a pod which can be done as:

$ kubectl logs [pod-name]
====================================================================================================================
Or to dump pod logs with a specific label:

$ kubectl logs -l name=[label-name]
====================================================================================================================
Or to get logs for a specific container as:

$ kubectl logs -l name=[label-name] -c [container-name]
====================================================================================================================
We can also stream logs as we do with Linux tail -f command with kubectl’s -f flag as well:

$ kubectl logs -f [pod-name]
====================================================================================================================
Running a pod interactively can be done with kubectl as:

$ kubectl run -i --tty busybox --image=busybox -- sh
====================================================================================================================
Or to run a pod in specific namespace use:

$ kubectl run nginx --image=nginx -n [namespace]
====================================================================================================================
You can attach it to a running container with attach command:

$ kubectl attach [pod-name] -i
====================================================================================================================
Port forwarding can be done for a pod at runtime with the below command:

$ kubectl port-forward [pod-name] [local-machine-port]:[pod-port]
====================================================================================================================
To execute something directly in a pod login and get the output use:

$ kubectl exec [pod-name] -- [command]
====================================================================================================================
The above command works if the pod contains a single container. For multi-container pods, use:

$ kubectl exec [pod-name] -c [container-name] -- [command]
====================================================================================================================
To show performance metrics for a given pod and its containers, we can use:

$ kubectl top pod [pod-name] --containers
====================================================================================================================
Or to sort it by a measure say CPU or memory, we can achieve it using:

$ kubectl top pod [pod-name] --sort-by=cpu
====================================================================================================================
Interacting with Nodes and cluster
kubectl can interact with the nodes and cluster. Here are some of the commands kubectl uses for the same.

For marking a node as un-schedulable, use:

$ kubectl cordon [node-name]
====================================================================================================================
To drain a node as part of the preparation for maintenance:

$ kubectl drain [node-name]
====================================================================================================================
To mark the node back as schedulable, use:

$ kubectl uncordon [node-name]
====================================================================================================================
For getting performance metrics related to a node, we can use:

$ kubectl top node [node-name]
====================================================================================================================
To get details about the current cluster:

$ kubectl cluster-info
====================================================================================================================
We can additionally dump the cluster state to standard output using:

$ kubectl cluster-info dump
====================================================================================================================
Or to dump to a file use:

$ kubectl cluster-info dump --output-directory=/path/to/cluster-state
====================================================================================================================
====================================================================================================
If you want to embed client certificate data in the “cluster-admin” entry, syntax changes to:

$ kubectl config set-credentials cluster-admin --client-certificate=~/.kube/admin.crt --embed-certs=true
====================================================================================================================
If you want kubectl to use a specific namespace and save it for all subsequent kubectl commands in that context:

$ kubectl config set-context --current --namespace=[NAMESPACE]
====================================================================================================================

        
        $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        
        
        ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
        
        
        ========================================================================================================
        $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
        
        
        
  COMMANDS CHEET SHEET
  ===========================================
  $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
  
  # Kubernetes Cheat Sheet

 A cheat sheet for Kubernetes commands.

## Kubectl Alias

Linux
```
alias k=kubectl
```

Windows
```
Set-Alias -Name k -Value kubectl
```

## Cluster Info

- Get clusters
```
kubectl config get-clusters
NAME
docker-for-desktop-cluster
foo
```

- Get cluster info.
```
kubectl cluster-info
Kubernetes master is running at https://172.17.0.58:8443
```

## Contexts

A context is a cluster, namespace and user.

- Get a list of contexts.
```
kubectl config get-contexts
```
```
CURRENT   NAME                 CLUSTER                      AUTHINFO             NAMESPACE
          docker-desktop       docker-desktop               docker-desktop
*         foo                  foo                          foo                  bar
```

- Get the current context.
```
kubectl config current-context
foo
```

- Switch current context.
```
kubectl config use-context docker-desktop
```

- Set default namesapce
```
kubectl config set-context $(kubectl config current-context) --namespace=my-namespace
```

To switch between contexts, you can also install and use [kubectx](https://github.com/ahmetb/kubectx).

## Get Commands

```
kubectl get all
kubectl get namespaces
kubectl get configmaps
kubectl get nodes
kubectl get pods
kubectl get rs
kubectl get svc kuard
kubectl get endpoints kuard
```

Additional switches that can be added to the above commands:

- `-o wide` - Show more information.
- `--watch` or `-w` - watch for changes.

## Namespaces

- `--namespace` - Get a resource for a specific namespace.

You can set the default namespace for the current context like so:

```
kubectl config set-context $(kubectl config current-context) --namespace=my-namespace
```

To switch namespaces, you can also install and use [kubens](https://github.com/ahmetb/kubectx/blob/master/kubens).

## Labels

- Get pods showing labels.
```
kubectl get pods --show-labels
```

- Get pods by label.
```
kubectl get pods -l environment=production,tier!=frontend
kubectl get pods -l 'environment in (production,test),tier notin (frontend,backend)'
```

## Describe Command

```
kubectl describe nodes [id]
kubectl describe pods [id]
kubectl describe rs [id]
kubectl describe svc kuard [id]
kubectl describe endpoints kuard [id]
```

## Delete Command

```
kubectl delete nodes [id]
kubectl delete pods [id]
kubectl delete rs [id]
kubectl delete svc kuard [id]
kubectl delete endpoints kuard [id]
```

Force a deletion of a pod without waiting for it to gracefully shut down
```
kubectl delete pod-name --grace-period=0 --force
```

## Create vs Apply

`kubectl create` can be used to create new resources while `kubectl apply` inserts or updates resources while maintaining any manual changes made like scaling pods.

- `--record` - Add the current command as an annotation to the resource.
- `--recursive` - Recursively look for yaml in the specified directory.

## Create Pod

```
kubectl run kuard --generator=run-pod/v1 --image=gcr.io/kuar-demo/kuard-amd64:1 --output yaml --export --dry-run > kuard-pod.yml
kubectl apply -f kuard-pod.yml
```

## Create Deployment

```
kubectl run kuard --image=gcr.io/kuar-demo/kuard-amd64:1 --output yaml --export --dry-run > kuard-deployment.yml
kubectl apply -f kuard-deployment.yml
```

## Create Service

```
kubectl expose deployment kuard --port 8080 --target-port=8080 --output yaml --export --dry-run > kuard-service.yml
kubectl apply -f kuard-service.yml
```

## Export YAML for New Pod

```
kubectl run my-cool-app —-image=me/my-cool-app:v1 --output yaml --export --dry-run > my-cool-app.yaml
```

## Export YAML for Existing Object

```
kubectl get deployment my-cool-app --output yaml --export > my-cool-app.yaml
```

## Logs

- Get logs.
```
kubectl logs -l app=kuard
```

- Get logs for previously terminated container.
```
kubectl logs POD_NAME --previous
```

- Watch logs in real time.
```
kubectl attach POD_NAME
```

- Copy files out of pod (Requires `tar` binary in container).
```
kubectl cp POD_NAME:/var/log .
```

You can also install and use [kail](https://github.com/boz/kail).

## Port Forward

```
kubectl port-forward deployment/kuard 8080:8080
```

## Scaling

- Update replicas.
```
kubectl scale deployment nginx-deployment --replicas=10
```

## Autoscaling

- Set autoscaling config.
```
kubectl autoscale deployment nginx-deployment --min=10 --max=15 --cpu-percent=80
```

## Rollout

- Get rollout status.
```
kubectl rollout status deployment/nginx-deployment
Waiting for rollout to finish: 2 out of 3 new replicas have been updated...
deployment "nginx-deployment" successfully rolled out
```

- Get rollout history.
```
kubectl rollout history deployment/nginx-deployment
kubectl rollout history deployment/nginx-deployment --revision=2
```

- Undo a rollout.
```
kubectl rollout undo deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deployment --to-revision=2
```

- Pause/resume a rollout
```
kubectl rollout pause deployment/nginx-deployment
kubectl rollout resume deploy/nginx-deployment
```

## Pod Example

```
apiVersion: v1
kind: Pod
metadata:
  name: cuda-test
spec:
  containers:
    - name: cuda-test
      image: "k8s.gcr.io/cuda-vector-add:v0.1"
      resources:
        limits:
          nvidia.com/gpu: 1
  nodeSelector:
    accelerator: nvidia-tesla-p100
 ```

## Deployment Example

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: my-namespace
  labels:
    - environment: production,
    - teir: frontend
  annotations:
    - key1: value1,
    - key2: value2
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

## Dashboard

- Enable proxy

```
kubectl proxy
```

# Azure Kubernetes Service

[List of az aks commands](https://docs.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest)

## Get Credentials
```
az aks get-credentials --resource-group <Resource Group Name> --name <AKS Name>
```

## Show Dashboard

Secure the dashboard like [this](http://blog.cowger.us/2018/07/03/a-read-only-kubernetes-dashboard.html). Then run:
```
az aks browse --resource-group <Resource Group Name> --name <AKS Name>
```

## Upgrade

Get updates
```
az aks get-upgrades --resource-group <Resource Group Name> --name <AKS Name>
```
        
        
        
        
        $
