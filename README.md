- [ETCD](#ETCD)
  
  - [ETCD for Kubernetes](#ETCD-for-Kubernetes)
 
  - [Set up Cluster using kubeadm](#Set-up-Cluster-using-kubeadm)
 
  - [High Availability Considerations](#High-Availability-Considerations)
 
- [Kube API Server](#Kube-API-Server)

  - [Deployment and Setup](#Deployment-and-Setup)
 
  - [Verifying the Deployment](#Verifying-the-Deployment)
 
- [Kube Controller Manager](#Kube-Controller-Manager)

- [Kube Scheduler](#Kube-Scheduler)

  - [Installing and Running the Kube Scheduler](#Installing-and-Running-the-Kube-Scheduler)
 
- [Kubelet](#Kubelet)

  - [Installing the Kubelet](#Installing-the-Kubelet)
 
- [Kube Proxy](#Kube-Proxy)

- [Pod](#Pod)

- [ReplicaSet](#ReplicaSet)

  - [Labels and Selector](#Labels-and-Selector)
 
- [Deployment](#Deployment)

  - [Handy Syntax to create Deplpoyment](#Handy-Syntax-to-create-Deplpoyment)
 
- [Services](#Services)

- [Namepsace](#Namepsace)

- [Imperative and Declarative](#Imperative-and-Declarative)

- [Apply Command](#Apply-Command)

- [Scheduling](#Scheduling)

  - [Manual Scheduling](#Manual-Scheduling)
 
  - [What is a Binding in Kubernetes](#What-is-a-Binding-in-Kubernetes)
 
  - [Labels and Selectors](#Labels-and-Selectors)
 
  - [Taints and Tolerance](#Taints-and-Tolerance)
  
  - [Node Selector](#Node-Selector)
  
  - [Node Affinity](#Node-Affinity)
  
  - [Resource Limit](#Resource-Limit)
  
  - [A quick note on editing Pods and Deployments](#A-quick-note-on-editing-Pods-and-Deployments)
  
  - [DaemonSet](#DaemonSet)
 
  - [Static Pod](#Static-Pod)
 
  - [Priority Classes](#Priority-Classes)
 
  - [Multiple Scheduler](#Multiple-Scheduler)
 
  - [Scheduler Profile](#Scheduler-Profile)
 
  - [Admission Controllers](#Admission-Controllers)
 
  - [Validating And Mutating Admission Controllers](#Validating-And-Mutating-Admission-Controllers)
 
- [Logging and Monitoring](#Logging-and-Monitoring)

  - [Monitor Cluster Component](#Monitor-Cluster-Component)
 
  - [Managing Application Logs](#Managing-Application-Logs)
 
- [Application Lifcycle Management](#Application-Lifcycle-Management)

  - [Rolling Update and Rollback](#Rolling-Update-and-Rollback)
 
  - [Command and Arguments Docker](#Command-and-Arguments-Docker)
 
  - [Commands and Arguments in Kubernetes](#Commands-and-Arguments-in-Kubernetes)
 
  - [ConfigMap](#ConfigMap)
 
  - [Secret](#Secret)
 
  - [Multi Container](#Multi-Container)
 
  - [Multi container Design Pattern](#Multi-container-Design-Pattern)
 
  - [AutoScaling](#AutoScaling)
 
  - [Horizontal Pod AutoScaler](#Horizontal-Pod-AutoScaler)
 
  - [In place Resize Pod](#In-place-Resize-Pod)
 
  - [Vertical Pod Autoscalser](#Vertical-Pod-Autoscalser)
 
- [Cluster Maintainence](#Cluster-Maintainence)

  - [OS Upgrade](#OS-Upgrade)
 
  - [Kubernetes Release](#Kubernetes-Release)
 
  - [Cluster Upgrade Process](#Cluster-Upgrade-Process)
 
  - [Demo Cluster Upgrade](#Demo-Cluster-Upgrade)
 
  - [Backup and Restore Method](#Backup-and-Restore-Method)
 
  - [Working with ETCDCTL and ETCDUTL](#Working-with-ETCDCTL-and-ETCDUTL)
 
- [Security](#Security)

  - [Security Primitives](#Security-Primitives)
 
  - [Authentication](#Authentication)

  - [TLS](#TLS)
 
  - [TLS in Kubernetes](#TLS-in-Kubernetes)
 
  - [Kubernetes Certificate Creation](#Kubernetes-Certificate-Creation)
 
  - [View Certificate Details](#View-Certificate-Details)
 
  - [Certificates API](#Certificates-API)
 
  - [Kubeconfig](#Kubeconfig)
 
  - [API Group](#API-Group)
 
  - [Authorization](#Authorization)
 
  - [RBAC](#RBAC)
 
  - [Cluster Role](#Cluster-Role)
 
  - [Service Account](#Service-Account)
 
  - [Image Security](#Image-Security)
 
  - [Docker Security](#Docker-Security)
 
  - [Security Contexts](#Security-Contexts)
 
  - [Network Policies](#Network-Policies)
 
  - [Kubectx and Kubens](#Kubectx-and-Kubens)
 
  - [Custom Resource Definitions CDR](#Custom-Resource-Definitions-CDR)
 
  - [Operator](#Operator)

 - [Storage](#Storage)
    
    - [Docker Storage](#Docker-Storage)
  
    - [Container Storage Interface](#Container-Storage-Interface)
  
    - [Volume](#Volume)
  
    - [Persistence Volume](#Persistence-Volume)
  
    - [Persitence Volume Claim](#Persitence-Volume-Claim)
  
    - [Storage Class](#Storage-Class)
  
- [Networking](#Networking)

  - [Linux Networking Basics](#Linux-Networking-Basics)
 
  - [DNS](#DNS)
 
  - [coreDNS](#coreDNS)
 
  - [Network namespaces](#Network-namespaces)
 
  - [Docker Networking](#Docker-Networking)
 
  - [Container Networking Interface CNI](#Container-Networking-Interface-CNI)

  - [Cluster Networking](#Cluster-Networking)
 
  - [Pod Networking](#Pod-Networking)
 
  - [CNI in Kubernetes](#CNI-in-Kubernetes)
 
  - [CNI weave](#CNI-weave)
 
  - [IPAM Ip address Managment](#IPAM-Ip-address-Managment)
 
  - [Service Networking](#Service-Networking)
 
  - [Cluster DNS](#Cluster-DNS)
 
  - [How Kubernetes Implements DNS](#How-Kubernetes-Implements-DNS)
 
  - [Gateway API](#Gateway-API)
  

# Kubernetes-CKA-

## Kodeloud Note 

(https://notes.kodekloud.com/docs/CKA-Certification-Course-Certified-Kubernetes-Administrator/Core-Concepts/Cluster-Architecture)

## ETCD

ETCD is a key-value storage . 

Key-value storage organize the data in document or file . Document contain all the relevant information of the individual 

To extract tar file : `tar xzvf <tar-file>`

To install etcd on Mac `brew install etcd`

Run `etcd` that it will start a service that listens on port 2379 by default 

- To check if `etcd` is running `ps aux | grep etcd`

- To check the port 2379 is open ` netstat -an | grep LISTEN | grep 2379`

`etcdctl` is a control client to store and retrive key-value pair 

To store a key-value pair `etcdctl put key1 value1` . This will create entry in the database with the information 

To retreive information : `etcdctl get key1` 

To see `etcdctl` command run `etcdctl`

## ETCD for Kubernetes 

etcd is distributed key-value storage that maintain configuration data, state, information, and metadata for my Kubernetes cluster .

Every object like nodes, pods, configurations, secrets, accounts, roles, and rolebinding is stored within etcd. When run a command like `kubectl get` the data is retrived from this data store 

Any changes I make to the Cluster - whether adding nodes, deploying pods, or configuring ReplicaSets, are first recorded in etcd. Only after etcd is updated are these changes considered to be complete 

` --advertise-client-urls https://${INTERNAL_IP}:2379`: This is the address on which etcd listens . It happen to be on the IP of the server and on port 2379 . This is a URL should be configured on kube API server when it tries to reach the etcd server 

#### Set up Cluster using kubeadm 

`kubeadm` deploys the etcd server for me as a pod in the `kube-system` namespace

To view all the pods running in the kube-system namespace, including etcd, run: `kubectl get pods -n kube-system`

Example output : 

```
NAMESPACE     NAME                                 READY   STATUS      RESTARTS   AGE
kube-system   coredns-78fcdf6894-prwl              1/1     Running     0          1h
kube-system   coredns-78fcdf6894-vqd9w             1/1     Running     0          1h
kube-system   etcd-master                          1/1     Running     0          1h
kube-system   kube-apiserver-master                1/1     Running     0          1h
kube-system   kube-controller-manager-master       1/1     Running     0          1h
kube-system   kube-proxy-f6k26                     1/1     Running     0          1h
kube-system   kube-proxy-hnzw                      1/1     Running     0          1h
kube-system   kube-scheduler-master                1/1     Running     0          1h
kube-system   weave-net-924k8                      2/2     Running     1          1h
kube-system   weave-net-hzfcz                      2/2     Running     1          1h
```

To list all keys stored by Kubernetes : `kubectl exec etcd-master -n kube-system -- etcdctl get / --prefix --keys-only`

Example output: 

```
/registry/apiregistration.k8s.io/apiservices/v1
/registry/apiregistration.k8s.io/apiservices/v1.apps
/registry/apiregistration.k8s.io/apiservices/v1.authentication.k8s.io
/registry/apiregistration.k8s.io/apiservices/v1.authorization.k8s.io
/registry/apiregistration.k8s.io/apiservices/v1.autoscaling
/registry/apiregistration.k8s.io/apiservices/v1.batch
/registry/apiregistration.k8s.io/apiservices/v1.networking.k8s.io
/registry/apiregistration.k8s.io/apiservices/v1.rbac.authorization.k8s.io
/registry/apiregistration.k8s.io/apiservices/v1beta1.admissionregistration.k8s.io
```

Kubernetes store data in the specific directory structure 

The etcd root directory, organized as the registry, contains subdirectories for various Kubernetes components such as nodes, pods, ReplicaSets, and Deployments.



#### High Availability Considerations

In Production Kubernetes environment, high availability (HA) is paramount. By running multiple master nodes with corresponding etcd instances, I ensure that my clsuter remain resililent even if one node fails 

To enable HA, each etcd instance must know about its peer. This is achieved by configuring the `--initial-cluster` parameter with the detailes of each member in the cluster 

` --initial-cluster controller-0=https://${CONTROLLER0_IP}:2380,controller-1=https://${CONTROLLER1_IP}:2380 \`


I must also specify path to certificate files so that ETCDCTL can authenticate to the ETCD API Server. The certificate files are available in the etcd-master at the following path. We discuss more about certificates in the security section of this course.

```
--cacert /etc/kubernetes/pki/etcd/ca.crt     
--cert /etc/kubernetes/pki/etcd/server.crt     
--key /etc/kubernetes/pki/etcd/server.key
```

The commands must specific the `ETCDCTL API version`:

```
kubectl exec etcd-master -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" 
```

## Kube API Server


When I run `kubectl` command it is reaching the `kube api server` 

`Kube-api` first authenticate the request and validate it . Then fetching data from etcd cluster, and replying with the desired information

I don' really need to use the kubectl command line . Instead I could also invoke the APIs directly by sending the post request 

```
curl -X POST /api/v1/namespaces/default/pods ...[other]
Pod created!
```

When a direct API POST request is made to create a pod, the API Server:

1. Authenticates and validates the request.

2. Constructs a pod object (initially without a node assignment) and updates the etcd store.

3. Notifies the requester that the pod has been created.

Summary : Kube-api Authenticate user, Validate Request, Retreving and updating data in the etcd data store  

Kube-api server is the only component that interacts directly with etcd data store 

The others like kube-control-manager and kubelt using kube-api server to perform in the Cluster in their respective area 

<img width="600" alt="Screenshot 2025-07-09 at 17 04 49" src="https://github.com/user-attachments/assets/0d473f26-6500-482f-b580-1a4d34338932" />

#### Deployment and Setup

When setting up a cluster on your own hardware, you need to download the Kube API Server binary from the Kubernetes release page, configure it, and run it as a service on the Kubernetes master node.

```
wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-apiserver


# kube-apiserver.service
ExecStart=/usr/local/bin/kube-apiserver \\
  --advertise-address=${INTERNAL_IP} \\
  --allow-privileged=true \\
  --apiserver-count=3 \\
  --authorization-mode=Node,RBAC \\
  --bind-address=0.0.0.0 \\
  --client-ca-file=/var/lib/kubernetes/ca.pem \\
  --enable-admission-plugins=Initializers,NamespaceLifecycle,NodeRestriction,LimitRanger,ServiceAccount,DefaultStorageClass,ResourceQuota \\
  --enable-swagger-ui=true \\
  --etcd-cafile=/var/lib/kubernetes/ca.pem \\
  --etcd-certfile=/var/lib/kubernetes/kubernetes.pem \\
  --etcd-keyfile=/var/lib/kubernetes/kubernetes-key.pem \\
  --etcd-servers=https://127.0.0.1:2379 \\
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
  --v=2
```

The Kubernetes architecture consists of a lot of different components working with each other, talking to each other in many different ways so they all need to know where the other components are 

There are different mode of authentication, authorization, encryption and security 

All the components will have cetificate associated with them 

This options `--etcd-servers=https://127.0.0.1:2379` where I specify the location of the etcd servers . This is how kube-api server connect to the etcd servers

#### Verifying the Deployment

For clusters set up with kube-admin tools, the Kube API Server is deployed as a pod in the kube-system namespace. To inspect these pods, run: `kubectl get pods -n kube-system`

I can see the options within the pods definition file, located at `etc/kubernetes/manifest/kube-apiserver.yaml` folder 

```
spec:
  containers:
  - command:
    - kube-apiserver
    - --authorization-mode=Node,RBAC
    - --advertise-address=172.17.0.32
    - --allow-privileged=true
    - --client-ca-file=/etc/kubernetes/pki/ca.crt
    - --disable-admission-plugins=PersistentVolumeLabel
    - --enable-admission-plugins=NodeRestriction
    - --enable-bootstrap-token-auth=true
    - --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
    - --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
    - --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
    - --etcd-servers=https://127.0.0.1:2379
    - --insecure-port=0
    - --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt
    - --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key
    - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
    - --proxy-client-cert-file=/etc/kubernetes/pki/front-proxy-client.crt
    - --proxy-client-key-file=/etc/kubernetes/pki/front-proxy-client.key
    - --requestheader-allowed-names=front-proxy-client
    - --requestheader-client-ca-file=/etc/kubernetes/pki/front-proxy-ca.crt
    - --requestheader-extra-headers-prefix=X-Remote-Extra-
    - --requestheader-group-headers=X-Remote-Group
    - --requestheader-username-headers=X-Remote-User
```

Another way to review the active API Server configuration is by checking the systemd service file on the master node: `cat /etc/systemd/system/kube-apiserver.service`

To see a running process `ps aux | grep kube-apiserver`

## Kube Controller Manager 

The controller is a process that continuosly monitor the state of various components within the system and work towards bringing the whole system to the desired functioning state

In Kubernetes Controller Manager act like a department in an organization - Each controller tasked with handling a specific responsibility . For exampl one controller task for checking the health of the Node. And another the ensure the desire number of pods is always running . These controllers constanly observe system changes to drive the cluster toward its intended state 

For example the Node Controller is responsible for check Node status every 5 secondes via Kube-API server . If node stop sending hearbeat, it is not immediately marked as unreachable, instead, there is a grace period of 40s followed by an addition 5 mins for potentiacal recovery before its pods are rechedule onto a healthy node. 

Another the Controller is Replica Controller its' responsible for checking the number of pods desired is maintained by creating new pod when needed . This mechanism reinforces the resillience and reliability of my Kubernetes Cluster 

<img width="665" alt="Screenshot 2025-07-09 at 17 43 26" src="https://github.com/user-attachments/assets/f3b74853-ef1f-4c3b-b730-891005e4a38f" />

## Kube Scheduler

Kube Scheduler is responsible for scheduling pods on nodes . 

Scheduler is only responsible for deciding which pods goes on which node . It doesn't actually place the pod on the Nodes (That's the job of the Kubelet)

When many Workers Nodes, scheduler make sure the right Pods end up on the right Worker Nodes depending on certain criteria . 

I may have pod with different resource requirements . I can have Nodes in the Cluster dedicated to certain application 

Scheduler looks at each pod and tries to find the best Node for it 

- First Scheduler filter out the Nodes that do not fit the profile of the pod like Node doesn't have enough CPU for the pod

- Then Secheduler rank the Node to identify the best fit for the pod . It use priority function to assign a score the the Nodes on a scale 0 to 10 for example scheduler calculate the amount of resources that would be free on the Nodes after placing the pod on them

I can customize and I can write my own scheduler as well 

- Resource requirments and Limits

- Tains and Tolerations

- Nodes Selectors/affinity

#### Installing and Running the Kube Scheduler

To install the kube-scheduler, download the binary from the Kubernetes release page. Once downloaded and extracted, you can run it as a service by specifying the scheduler configuration file. Below is a sample command for downloading the binary and an example systemd service configuration: `wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-scheduler`

To extract tar file : `tar xzvf <tar-file>`

Then Run as a Service

Below is an example of the systemd service configuration for the kube-scheduler:

```
# File: kube-scheduler.service
ExecStart=/usr/local/bin/kube-scheduler \
  --config=/etc/kubernetes/config/kube-scheduler.yaml \
  --v=2
```

## Kubelet

The Kubelet on the Worker Node register the Node with a Kubernetes Cluster . When it receives intructions to load a container or a pod on the node it requests the container runtime engine to pull the required image and run an instance 

Kubelet then continues to monitor the State of the Pod and container in it and reports to the Kube API server on timely basis 

#### Installing the Kubelet

Execute the following command to download the Kubelet binary: `wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kubelet`

Configure and Run the Kubelet as a Service :

- Set up the Kubelet with the required configuration by running it as a service. Use the command below to start the Kubelet with the necessary parameters:

```
ExecStart=/usr/local/bin/kubelet \
  --config=/var/lib/kubelet/kubelet-config.yaml \
  --container-runtime=remote \
  --container-runtime-endpoint=unix:///var/run/containerd/containerd.sock \
  --image-pull-progress-deadline=2m \
  --kubeconfig=/var/lib/kubelet/kubeconfig \
  --network-plugin=cni \
  --register-node=true \
  --v=2
```

If I use kubeadm tool to deploy my Cluster it does not automatically deploy the kubelet . I must alway manually install the Kubelet on my Worker Nodes 

To see the kubelet process `ps aux | grep kueblet`

## Kube Proxy

Within Kubenetes Cluster every pod can reach every other Pod . This is accomplished by deploy pod networking solution to the Cluster . 

Pod Network is an internal virtual network span accross all the nodes in the Cluster to Which all the pods connect to .

Through this network they are able to communicate with each other . There are many solutions available for deploying such an network 

I have a web Application deploy on the First Node and DB application deployed on the second  Node . Web App reach the DB using DB Ip address of the Pod, but no garantee IP of the pod will alway remain the same so we will use a Service 

- So I created a Service to expose a DB application accross the Cluster . The web Application can now access the DB using the name of the Service

- Service also get IP address assigned to it .

- Whenever a Pod tries to reach the Service using its IP or name, it forwards the traffic to the backend pod which is DB

- Service can not join the Pod network bcs Service is virtual component only live in Kubernetes memory

How do Service should be accessible accross the Cluster from any nodes ? 

- That's where Kube Proxy comes in

- Kube-Proxy is a process that run on each Node in the Kubernetes Cluster its job to look for new Services, and everytime new Service is created it created the appropriate rules on each node to forward traffic to those services to the backend pods

- One way it does this is using IPtables rules

- This case it create a IPtables rules on each node in the cluster to forward traffic heading to the IP of the service

## Pod 

Kubernetes does not deploy containers directly on the Worker Nodes 

Container encapsulated into a Kubernetes object known as Pods

Pods is a smallest object in Kubernetes 

I can have multiple comntainers in 1 Pod . They can share network space and storage space as well 


To deploy Pod `kubectl run nginx --image nginx`

## ReplicaSet

The Replication Controller help us run multiple instances of a single pods in Kubernetes  thus provide High Availablity 

Replication Controller can help by automatically bringing up the new Pod when the existing one failed . Thus Replication Controller ensure that the specified number of pods are running at all times even just 1 or 100 

Another reason we use Replication Controller to create multiple Pod to share the load across them . When number of users increase we create multiple Pods to balance the Load . If we run out of resources on the First Node we could deploy addition Pods accross the other Node in the Cluster 

ReplicaSet and Replication Controller both have the same purpose but they are not the same 

Replication Controller is a older technologies that being replace by ReplicaSet 

#### Selector Definition 

Seletor in the ReplicaSet help the ReplicaSet identify what Pods fall under it 

Why do we have to specify it ? 

Bcs ReplicaSet can mananage Pods that that are not part of the ReplicaSet creation . 

For example there are Pod created before ReplicaSet created that match lables specified in the Selector. The ReplicaSet also take those Pod into consideration when creating Replicas 

#### Labels and Selector 

The role of the ReplicaSet is to monitor the Pod and if any of them fail, deploy a new one . The ReplicaSet is in fact a process that monitor a Pod

How does ReplicaSet know what Pods to monior ? 

Labeling our Pod comes in . We can provide labels as a filter for ReplicasSet 

## Deployment

Deployment is an Kubernetes Object come higher in the hierarchy . Deployment provide capability to upgrade the underlying instances seamlessly using **rolling update**, undo changes, and paused and resume changes as required 

Deployment automatically create a ReplicaSet 


#### Handy Syntax to create Deplpoyment

Create an NGINX Pod: `kubectl run nginx --image=nginx`

Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run): `kubectl run nginx --image=nginx --dry-run=client -o yaml`

Create a deployment: `kubectl create deployment --image=nginx nginx`

Generate Deployment YAML file (-o yaml). Don't create it(--dry-run): `kubectl create deployment --image=nginx nginx --dry-run=client -o yaml`

Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run) and save it to a file: `kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml`

Make necessary changes to the file (for example, adding more replicas) and then create the deployment: `kubectl create -f nginx-deployment.yaml`

OR: 

In k8s version 1.19+, we can specify the --replicas option to create a deployment with 4 replicas.: `kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml`

## Services

Kubernetes Serives enable communication between components inside and outside of the applications 

Kubernetes Services help us connect application together

Service acts as a built-in load balancer to distribute load accross different Pods 

## Namepsace

Kubernetes create a set of pods and services for its internal purpose such as those required by the networking solution, DNS service etc ... 

To isolate these from the user and to prevent me from accidentally deleting or modifying these services . Kubernetes create them under another namepsace created at cluster startup name `kube-system`\

Third namespace created by Kubernetes is `kube-puclic` this is where resources should be made available to all user are created  

I also can create my own namepsace . For exampl I will create a Dev environment and production environment namespace so when I am modifying each environment they not collapse with each other 

Each of namepsaces can have its own set of policies that define who can do what . I can also assign quota of resources to each of  these namespaces . That way each namespace is guaranteed a certain amount and it does not use more that its allowed limit

The resources within a namespace can refer to each other simply by their names 

For Pods can reach Service in another namespace : `servicename.namespace.svc.cluster.local`. 

- I am able to do this bcs when the Service is created, a DNS entry is added automatically in this format

- the last part `cluster.local` is the default domain name of the Kubernetes Cluster

- `svc` is a subdomain for `Service`

To switch to namespace Permanently : `kubectl config set-context $(kubectl config current-context) --namespace=dev`

To views pod in all namespace : `kubectl get pods --all-namespaces`

To limit resources in a namepsace . Create a resource quota 

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namepsace: dev
spec:
  hard:
    pods: "10"
    request.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```

## Imperative and Declarative 

### Imperative Commands with Kubectl

(https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/15018998#overview)

Reference:
https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

https://kubernetes.io/docs/reference/kubectl/conventions/

## Apply Command

The `apply` command takes into consideration the local configuration file, the live object definition on Kubernetes and the last applied configuration before making a decision on what changes are to be made 

If I run `apply` command if the object does not already exist, the object is created . When the Object is created, an object configuration, similar to what we create locally is created within Kubernetes but with additional fiedls to store status of the object . This is the live configuration of the object on the Kubernetes Cluster 

This is how Kubernetes internally stores information about an object no matter what approarch you use to create an object 

When I use `apply` command it does something bit more . The YAML file from local is converted into a JSON format and it then strored as the last applied configuration 

For any updates to the object all the three are compared to identify what changes are to be made on the live object .

(https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/)

## Scheduling

### Manual Scheduling

Every Pod have a field call `nodeName` that not set by default . 

The sheduler goes through all the Pods and looks for those that don't have this property set . Those are the candidates for scheduling 

It then identifies the right Node for the Pod by running the scheduling algorithm . Once identified it schedules the pods on the Node by setting the `nodeName` property to the name of the node by creating a binding object .

If there is no scheduler to monitor and shedule Nodes the Pod continue to be in a pending State . Then I can manully assign pods to Nodes myself 

Without a scheduler, the easiest way to schedule a pod is to simple set the `nodeName` field to the name of the Node in my pod specification file while creating the Pod . The Pod then get assigned to the specified node 

I can only specify the `nodeName` at create time 

Kubernetes won't allow me to modify the `nodeName` property of the a Pod . 

Another way to assign a node to an existing Pod is to creating a Binding Object and send a POST request to the Pod's binding API thus mimicking what the actual scheduler does 

In the `Binding Object` I specify the target Node with a name of the Node for example : 

```
apiVersion: v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: node02
```

Then I will send the POST request to the Pod's Binding API in the JSON format (must convert the YAML file into a JSON format)

`curl --header "Content-Type: application/json" --request POST --data {"apiVersion": "v1", "kind": "Binding", ...} http://$SERVER/api/v1/namespaces/default/pods/nginx/binding`

#### What is a Binding in Kubernetes

Binding object is an internal, low-level API resource that Kubernetes Scheduler use to bind a Pod to specific Node 


When you create a Pod, it starts out unscheduled — it has no Node assigned yet.

The Scheduler:

- Watches for Pods with nodeName not set.

- Picks the best Node according to scheduling rules (resources, affinities, etc.).

- Writes a Binding object to the API server to commit the Pod to a Node.

Kubernetes API server then sets the `nodeName` field on the Pod spec.

#### Labels and Selectors

Labels and Selectors is a standard methods to groups thing together 

Labels are properties attached to each item . 

Selectors help me filter these items 

Kubernetes objects use labels and selector internally to connect different objects together . 

Annotiations are used to record other details for informatory purpose . 

- For example tool details like name, version, build inforamtion etc.... that maybe used for some kind of integration purpose 

## Taints and Tolerance

In Kubernetes, taints and tolerations are used solely for scheduling control, not security. 

Taints and Tolerance used to set restrictions on what pods can be scheduled on a Node 

Taints are set on Node, and Toleration are set on Pod 

To taints the Nodes : `kubectl taints node node-name key=value:taint-effect`

- `taint-effect` define what would happen to the Pod if they do not tolerace the taints

- There is 3 `taint-effect` :

  - `NodeSchedule`
 
  - `PreferNoSchedule` which mean the system will try to avoid placing a Pod on the Node, but that not garantee
 
  - `NoExecute`: which means that new Pods will not be schedule on the Node and existing Pods on the Nodes if any will be evicted if they do not tolerace the taints   

Example : `kubectl taints nodes node1 app=blue:No-Schedule`

To add `Toleration` to Pods : 

```
apiVersion: apps/v1
kind: Pod
metadata:
  app: name
spec:
  containers:
  - name: nginx-container
    image: nginx
  tolerations: ## All of these value need to be encoded in ""
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```

When the Pods are created or updated they are either not scheduled on Nodes or evicted from the existing Node depending on the Effect Set 

Taints and Toleration are only meant to restrict Nodes from from accepting certain pods . For example If Node 1 can only accept Pod D but it doesn't guarantee that Pod D will alway be place on Node 1 

If the requirment is to restrict a Pod to certain Nodes it is acchive through another concept called as `Node affinity` 

## Node Selector

We can set limitations on the Pods so that they are only run on particular Node. There is 2 ways to do this 

- First : Using `Node Selector`

```
apiVersion: apps/v1
kind: Pod
metadata:
  app: name
spec:
  containers:
  - name: nginx-container
    image: nginx
  nodeSelector:
    size: Large
```

Where does `size: Large` come from and How Kubernetes know which is the Large Nodes ?

- The key-value pair of size and large are in fact labels assigned to the Nodes . The `Scheduler` use labels to match and identify the right Node to place to Pods on

How we can Labels the Nodes : `kubectl labels nodes node-name key=values`

- For example: `kubectl labels nodes node01 size=large`

## Node Affinity 

The primary purpose of `Node Affinity` is to ensure that Pods are hosted on particular Nodes .

`Node Affinity` provide us advance cabability to limit pod placement on specific nodes 

```
apiVersion: apps/v1
kind: Pod
metadata:
  app: name
spec:
  containers:
  - name: nginx-container
    image: nginx
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoreDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In # 
            values:
            - Large
```

`operator: In`: Ensure that the pod will be place on Node whose labels `size` has any value in the list of `values` . I can add multiple values to a list of values 

`operator: NotIn` : To do something like size not in small 

`operator: Exist`: Will simply check if the Labels `size` exist on the Nodes and I don't need a value section for that as it does not compare the value

What if `Node Affinity` could not match a Node with a given expression . What if there are no Nodes with a labels called Size ? 

Let's say we have labels and the pods scheduled . What if someone changes labels on the Nodes at the future point in time ? will the pod continue to stay on the Node ? 

- All of this is answered by property under nodeAffinity which happen to be a type of nodeAffinity

- The type of nodeAffinity defines the behavior of the scheduler with respect to nodeAffinity and the stages in the life cycle of the Pod . There is 2 type available :

  -  `requiredDuringSchedulingIgnoreDuringExecution`  and `preferredDuringSchedulingIgnoreDuringExecution`
 
  -  `DuringScheduling` is when the pod is not existed and it created for the first time .
 
  -  What if the Nodes with matching Labels are not available ?
  
    -  If I select the `required` type the scheduler will mandate that the pod be placed on the Node with a given affinity rule . If it can not find one . The Pod will not be scheduled

    - If I select the `preferred` in cases where a matching node is not found the `Scheduler` will simply ignore nodeAffinity rules and place the pod on any available Node
 
  - `DuringExecution` is the state where a Pod has been running and a change is made in the environment that effect nodeAffinity such as a change in the label of a Node
 
    - For example if the Node removed the required Labels Pod will continue to run bcs the type set to `Ignored`
   
    - If I choose `RequiredDuringExecution` which will evict any Pod that are running on Node that do not meet affinity rules   

## Resource Limit

`Resource Request`: With this I can specify the amount of CPU and Memory required for a Pod when creating one . So the minimum amount of CPU or Memory requested by the container 

To do a `Resources Request`:

```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
  labels:
    name: simple-webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
    - containerPort: 8080
    resources:
      requests:
        memory: "4Gi"
        cpu: 2
```

What does 1 count of a CPU mean ? 

- I can specify any value as low as `0.1`. So `0.1 CPU` can also be expressed as 100m (m stand for milli)  . I can go as low as 1m

- 1 count of `CPU` = 1 vCPU (AWS) = 1 GCP (Google) = 1 Azure Core  = 1 Hyperthread

Memory : 

- I can specify 256Mi (megibyte) or specify the same value in memory like this

```
1G = 1,000,000,000 bytes

1M = 1,000,000 bytes

1K = 1,000 bytes

1Gi = 1,073,741,824 bytes

1Mi = 1,048,576,bytes

1Ki = 1,024 bytes
```

I can set a limit for a resources usage on these Pods using `Resources Limit`

```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
  labels:
    name: simple-webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
    - containerPort: 8080
    resources:
      requests:
        memory: "4Gi"
        cpu: 2
      limits:
        memory: ""
        cpu: ""
```

A containers can not use more CPU limits than its limit 

However containers can use more its memory limit 

`Default behavior` :  By default Kubernetes does not have a CPU or memory request or limit set . This mean that any Pod can consume as much resources as require on any node and suffocate other pods or processes running on the nodes 

How to ensure that every pod created has some default set ? 

- By using `LimitRange` .

- `LimitRange` help me define default values to be set for containers in Pods that created without a request or limit specified in the Pod definition files . This is applicable at the `namespace` level 

```
# limit-range-cpu.yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-resource-constraint
spec:
  limits:
    - default:
        cpu: 500m
      defaultRequest:
        cpu: 500m
      max:
        cpu: "1"
      min:
        cpu: 100m
      type: Container
```

```
# limit-range-memory.yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: memory-resource-constraint
spec:
  limits:
    - default:
        memory: 1Gi
      defaultRequest:
        memory: 1Gi
      max:
        memory: 1Gi
      min:
        memory: 500Mi
      type: Container
```

The way to restrict the total amount of resources that can  be consumed by applications deployed in Kubernetes Cluster 

- For example If I have to say all the Pods together shouldn't consume more than this much CPU or Memory

- To do that we create `Quotas` as a namespace level `ResourceQuota`

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-resource-quota
spec:
  hard:
    requests.cpu: 4
    requests.memory: 4Gi
    limits.cpu: 10
    limits.memory: 10Gi 
```

### A quick note on editing Pods and Deployments : 

(https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/14937592#overview)

## DaemonSet

`DaemonSet` help to deploy multiple instance of Pods but its run one copy on every single Node in my Cluster . Whenever new node added to a Cluster a replica of the Pods automatically added to that node and when the Node is removed the Pods is automatically removed 

Use case :

- Deploy Monitoring agent

- Deploy logs Collector app

For example one of the Worker Nodes process is required on every Worker Nodes is `kube-proxy`. `kube-proxy` can be deploy as a DaemonSet 

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
 name: monitoring-daemon
spec:
  selector:
    matchLabels:
      labels:
        app: monitoring-daemon
  template:
    metadata:
      labels:
        app: monitoring-daemon
    spec:
      containers:
        - name: monitoring-daemon
          image: monitoring-image
```

How do DaemonSet work ? How do it ensure that every Node has a Pod

- We could set a `nodeName` on the Pod to bypass the scheduler and get the Pods place on a Node directly

- From version 1.12 forward DaemonSet use the default scheduler and node affinity rules to schedule pod on Node 

## Static Pod 

The `kubelet` relies on the `kube-apiserver` for instructions on what Pods to load on its Node which was base on the decison made by `kube-scheduler` which was stored in the `etcd data store` . But what if there is no Master Node ? 

I can configure the to read the Pod definiation files from a directory on the Server designated to store information about Pods . I will place the Pods defination file in this locations `/etc/kubernetes/manifests` . The `kublet` check this directory for files, read this file and create pod 

Not only it create the Pod it can ensure that the Pod stay alive, if the application crashes the kublet attempt to restart it . 

If I make a change to files on this directory . Kubelet will regenrate the Pod 

If I remove the files from `/etc/kubernetes/manifests` the Pod is deleted automatically 

Kubelet work at Pod level so it can only understand Pod . 

**What is that designated folder and how to configure it ?**

Could be any directory on the host and the location of that directory is passed in to the Kubelet as an option while running the Service 

Option name : `--pod-manifest-path=/etc/Kubetnetes/manifests`

Another way to configure instead of specify the option directly in the kubelet.service file I could provide a path to another config file using `--config` option and then define the directory path as staticPodPath in that file `--config=kubeconfig.yaml` . Cluster set up by the `kube admin` tool uses this approach 

If I am inspecting an existing cluster, I should inspect this option of the kubelet to identify the path to the directory . I will thenknow where to place the definition file for my staticPod 

- First, check the `--option pod-manifest-path` in the kubelet service file . If it is not there then look for the `--config` option and identify the file used as the config file . and then within the config file look for the staticPod option

Once static Pod create I can view it by running the `docker ps` command . Bcs we don't have the Control Plane and kubectl work with `kube-apiserver`

How does it work when the Node is part of a Cluster when there is an API server requesting the kubelet to create Pods . Can kubelet create both kinds of Pods at the same time ? 

- The way kubelet works is it can take in requests for creating Pods from different inputs .

  - First through the Pod definition files (from Static Pod folder)
 
  - Second is through an HTTP API endpoint
 
  - Kubelet can create both kinds of Pods at the same time

Is `API Server` aware of the static Pod created by the Kubelet ? Yes 

- When `Kubelet` create a static Pod, if it is a part of a Cluster, it also creates a mirror object in the `Kube-Apiserver`.

- What I see from kube-apiserver is a read-only mirror of the Pod .

- I can view the Pod but I can not add, edit, or delete like usual Pods . I can only delete them by modify the files from Nodes manifest folder

!!! NOTE: The name of the Pod is automatically appended with the Node name.

Why do we want to use StaticPod ? 

- Since Static Pod is not part of the Control Plane . I can use it to deploy the Control Plane component itself

- Start by installing `Kubelet` on all the master Nodes, then create Pod definition files that uses docker images of various control Plane components such as API server, Controller, ETCD, .... Place the definition file in the designated manifest folder and the Kubelet takes care of deploying the Control Plane Components themselves as Pod on the Cluster 


## Priority Classes

We need the way to make sure that higher priority workloads always get scheduled without being interrupted by lower priority workloads 

Priority Classes help us to defines priority for different workloads . If the higher Priority Pod cannot be scheduled, it will terminated the lower Priority Pod and make higher Priority Pod happend 

Priority are not within the speicfic namepsace (Cluster Wide)

To define Priority we are using range of numbers : As high as 1000,000,000 and as low as -2,147,483,648 (This range is for Applications and Workloads that are deployed as app in the cluster)

There is a separate range as high as 2,000,000,000 that's dedicate for internal system critical parts (Such as Control Plane) . They alway get highest priority 

To list existing priorityclass : `kubectl get priorityclass`

To create a new priority class: 

```
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000000
description: "Priority class for mission critical pods" (optional)
```

To associate Priority Class to Pod by using: `priorityClassName`

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 8080
  priorityClassName: high-priority
```

IF I don't specify priority class name for a pod. By default it is assumed to have a priority class value, a priority value of 0 , if I want to modify the default value I must create a new priority class and assign the `globalDefault: true` (It only be defined in a single priority class)

If we have a higher priority job and there are no more resources available on the Cluster . The behavior is defined by the preemption policy defined in the priority class assigned to the new workload

- `preemptionProlicy: PreemptLowerPriority` .

- If preemptionPolicy is not set it default set to `preemptionProlicy: PreemptLowerPriority`. It mean it would kill the lower priority and take its place

- If I don't want it to kill the exsiting workload and instead wait for the cluster resources  to free up then I must set `preemptionProlicy: never`

To compare the priority Class on both pods using : `kubectl get pods -o custom-columns="NAME:.metadata.name,PRIORITY:.spec.priorityClassName"`

## Multiple Scheduler

If I have specific application that requires its components to be placed on Nodes after performing some addition checks  . 

I can write my own Kubernetes Scheduler Program package it and deploy it as the default scheduler or as an addition shceduler . That ways Applications will go through the default scheduler and some specific Applications that I may choose can use my own custom sheduler 

When there are multiple Scheduler they must have different name so that we can identify them 

To deploy addition scheduler : `wget https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-scheduler`. I will point the configuration to the custom configuration file :

```
custom-scheduler.yaml

apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler-2
```

- Each scheduler use a separate configuration file and with each file having its own scheduler name .  

Another way is deploy scheduler as a Pod, and specify the kubeconfig property which is the path to the `scheduler.conf` file that has the authentication information to connect to the Kubernetes API server 

- Then I pass `--config=/etc/kubernetes/my-scheduler-config.yaml` as a config option to scheduler

- NOTE : We have the scheduler name specified in the file .

```
apiVersion: v1
kind: Pod
metadata:
  name: custom-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --config=/etc/kubernetes/my-scheduler-config.yaml ## This is how the name got pick up by the scheduler 
    image: k8s.gcr.io/kube-scheduler-amd64:v1.11.3
    name: kube-scheduler
```

Another option in custom-scheduler.yaml is `leaderElection` is used when I have multiple copies of the scheduler running on different master node as a High Availability set up where I have multiple Master Node 

```
custom-scheduler.yaml

apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler-2
leaderElection:
  leaderEffect: true
  resourceNamespace: kube-system
  resourceName: lock-object-my-scheduler
```

- If multiple copies of the same scheduler are running on different nodes only 1 can be active at the time that where `leaderElection` choosing the leader who will lead the scheduling activities 

How to deploy Configure Multiple Scheduler docs (https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/)

Once we have deployed that custom Scheduler, the next step is to configure a pod or deployment to use this new scheduler . I can configure like this:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
  schedulerName: my-custom-scheduler ###
```

- If custom scheduler configure incorrect . The pod will remain in the Pending state 

To know which scheduler pick it up particular pod : `kubectl get events -o wide`. This will list all the events in the current namespace and look for the scheduled events 

Also I can view the logs of the scheduler : `kubect logs my-custom-scheduler -n kube-system`

## Scheduler Profile 

First when Pods created it end up in the  `Scheduling Queue` . This is where the Pod wait to be scheduled . At this stage pod are sorted base on `Priority` defined on the Pods

Then the Pod enter the `Filter Phase` . This is where Nodes can not run the Pod are filtered out 

Next is the `Scoring Phase` this is where Nodes are scored with different weights . 

- Scheduler associate score to each Node based on the free space that it will have after reserving the CPU required for that Pod

Finally Pod will go to binding phase . This is where the Pod is finally bound to a Node with the highest score . 

All of these operations are achived with certain plugins 

- In `Scheduling Queue`, the `PrioritySort` plugin that sorts the pods in an order based on the priority configured on the Pods . This is how Pods with Priority class get higher Priority over the other Pods when scheduling

- In the `Filtering` stage, the `NodeResourcesFit` that identifies the Node that has sufficent resources required by the Pods and filters out the Node that doesn't

  - Other plugin within this stage is `nodeName` plugin that check if the Pod have a nodeName mentions in the Pods Spec and filter out all the Nodes that does not match this name
 
  - Other Plugin is `NodeUncheduable` plugin that filters out nodes that has the `Unschedulable: true`. Make sure no Pods are set on those Nodes
 
- In the `Scoring phase`, `NodeResourcesFit` plugin associate a score to each node base in the resources available on it and after the pod allocated to it

  - Other plugin is `ImageLocality` plugin associate the high score to the Node that already have the container Image used by the Pods among the different nodes
 
  - At this phase the plugins do not really reject the pod placement on a particular node . For example `ImageLocality` plugin ensure the Pod are place on the Node that already has the Image but if there no Node available, it will anyway place the Pod on the Node that does not even have the image
 
- In `Binding` phase we have `DefaultBinder` plugin that provide the binding mechanism

The highly extensible nature of Kubernetes allow us to customize what plugin go where . And for us to write our own plugin and plug them in there . That achieved with the help of `Extension Points`

At each Stage there is a `Extension Points` for it to which the plugins can plug into

In `Scheduling Queue` we have `QueusortExtention` to which `PrioritySort` plugin to plugged to 

In `Filtering` phase we have Filter Extention

There are extentions before entering the Filter Phase called `preFilter` extension and after filter phase called `postFilter`

There are extentions before entering the Scroing phase called `preScore` extention and after `reserve`

Then `Permit` and `preBind` before Binding Phase adn `bind`, `postBing` after the Binding Phase 

To changed the behavior the default Behavior of how these plugins are called and how we can get our own plugins in there :

-  I can configure `Multiple Profile` within a single Scheduler in the configuration file by adding more entry to the list of `profiles`

To configure scheduler `profiles` to work differently 

```
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
  - schedulerName: my-scheduler-2
    plugins:
      score:
        disabled:
          - name: TaintToleration
        enabled:
          - name: MyCustomPluginA
          - name: MyCustomPluginB
  - schedulerName: my-scheduler-3
    plugins:
      preScore:
        disabled:
          - name: '*'
      score:
        disabled:
          - name: '*'
  - schedulerName: my-scheduler-4
```

Docs (https://kubernetes.io/docs/concepts/scheduling-eviction/scheduling-framework/)

## Admission Controllers

Admission Controller help us to implement better security measure to enforce how cluster is used 

Apart from simply validating configuration, `Addmission Controller` can suggest change the request itself or perform additional operations before the Pod created 

There are number of `Addmission Controllers` that come prebuilt with Kubernetes such as :

- `AlwaysPullImages`: that ensure everytime pod created image always pulled

- `DefaultStorageClass`: Observes the creation of PVC and automatically adds a default Storage Class to them if one is not specified

- `EventRateLimit` : Help set the limit number of requests with `API-Server` can handle at the time to prevent `API-Servers` become from flooding with requests

- `NamespaceExists`: Reject the requests to namespace does not exist

- `NamespaceAutoProvision` (not enable by default) this will automatically create a namespace it if does not exist 


To see a list of `Admission Controller` enabled by default : `kube-apiserver -h | grep enable-admission-plugins`

If I am running this in `kubeadm` based setup I can do : `kubectl exec kube-apiserver-controlplane -n kube-system -- kube-apiserver -h | grep enable-admission-plugins`

To add the addmission controller : on the `/usr/local/bin/kube-apiserver` add flag `--enable-admission-plugins=<admission plugins (could be many)>`

If using `Kubeadm-Based Setup (API Server as a Pod)`

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-apiserver
    - --authorization-mode=Node,RBAC
    - --advertise-address=172.17.0.107
    - --allow-privileged=true
    - --enable-bootstrap-token-auth=true
    - --enable-admission-plugins=NodeRestriction,NamespaceAutoProvision ### Admission controller here 
    image: k8s.gcr.io/kube-apiserver-amd64:v1.11.3
    name: kube-apiserver
```

## Validating And Mutating Admission Controllers


The `NamepsaceExists` or `NamespaceLifecycle` admission controller , It can help validate if a namespace already exists and reject the request if it doesn't exist . This is known as `Validating Admission` controller

Another type of `Admission Controller` is `DefaultStorageClass` . Example I am submitting a request to create a PVC the request go through `Authentication`, `Authorization` and `Admission Controllers` the `DefaultStorageClass Admission Controller` will watch for a request to create a PVC and check if it has a StorageClass mention in it . If not It will modify my request to add the DefaultStorageClass to my request . This one known as `Mutating Admission Controller` 

`Mutating Admission Controller` are those that can change the request 

`Valiadate Admission Controller` are those that can validate the request and allow or deny

There are maybe `Admission Controller` can do both .  

Generally `Mutating Admission Controller` are invoked first followed by `Validating Admission Controller` . This is so that any change made by `Mutating Admission Controller` can be considered during the `Validation process` 

If I want my own `Admission Controller` with my own mutations and validations that has my own logic . To support external `Admission Controllers` there are 2 special `Admission Controllers` available 

- `MutatingAdmissionWebhook`

- `ValidatingAdmissionWebhook`

We can configure these webhooks to point to a Server that's hosted either within the Kubernetes Cluster or outside it, and our server will have our own `Admission Webhook Service` running with our code and logic. After the request go through it hit the webhook that configured . Once it hit a webhook it makes a call to the `Admission Webhook Server` by passing an `AdmissionPreview` object in a JSON format . This Object has all the details about the request, such as the user that made the request, and the type of Operation the user is trying to perform and on what Object and details about the object itself . On receiving the request the admission webhook server response with an `AdmissionReview` object with a result of whether the request is allowed or not . If allowed field set to True then the request is allowed 

To set this up : 

1. Deploy `Admission Webhook Server` which will have our own logic .

2. Then I will configure the Webhook on Kubernetes by creating a webhook Configuration Object 


#### Configuration Admission Webhook 

`clientConfig`: This is where we configure the location of our `Admission Webhook Server` . 

- If we deploy this Server externall on our onw and that is not a part of deployment in Kubernetes Cluster then we can provide `url: <>` path to that server

- If we deploy a Server as another Service on our Cluster then we can do . The communication between `API-server` and the `Webhook` server has to be over TLS . So `caBundle` should be configured

`rules`: Is what We must specify when to call our `API Server` . 

- To configure exactly when we want our webhook Server to be called for validation 

```
apiVersion: admissionregestration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  name: "pod-policy.example.com"
webhooks:
- name: "pod-policy-example.com"
  clientConfig:
    url: "https://external-server.example.com" ## This is for External Server

    service: # This is for deploy as another Service in Cluster 
      namespace: "webhook-namespace"
      name: "webhook-service"
    caBundle: "TLS" # Certificate bundle should be configured  

  rules:
  - apiGroups: [""]
    apiVersion: ["v1"]
    operations: ["CREATE"]
    resources: ["pods"]
    scope: "Namespaced" 
```

## Logging and Monitoring

## Monitor Cluster Component 

I want to know `node-level` metric . Such as number of Nodes in the Cluster, How many of them are healthy . Also Performance metrics such as CPU, Memory, Network, and disk utilization 

I also want to know `pod-level` metrics. Such as number of Pods, CPU and Memory consumption on them

To monitor resoruce consumption on Kubernetes I can use `Metrics Server`

I can have 1 Metrics Server per Kubernetes Cluster . The `Metrics Server` retrieves metrics from each of the Kubenernetes Nodes and Pods, aggregates them and stores them in the memory 

!!! NOTE: `Metric Server` is an only **in-memory** monitoring solution and does not store the metrics on the disk . As the result I can not see historical performance data 

How are the Metrics generated for the pods on these Noodes ? 

- Kubernetes run `Kubelet` on each Node which responsible for receving intructions from `kube API-server` Control Plane and running Pod on the Node

- `Kubelet` also contain sub Component known as `Container Advisor`. `cAdvisor` responsible for retriveing performance metrics from pods and exposing them through the `kubelet API` to make the metrics available for the `Metric Server`

To deploy Metric Server: 

- First I need to clone the Git Metric Server Repo `git clone https://github.com/kubernetes-incubator/metrics-server.git`

- And then I will `kubectl create -f deploy/1.8+/` . This command deploy a set of pods, services, and roles to enable Metrics Server to pull from performence metrics from  the Nodes in the Cluster

Once Processed Cluster Performce can be viewed by running `kubectl top node` (This provide CPU and memory consumption) 

`kubectl top pod` to performance metrics of pods in Kubernetes

## Managing Application Logs

`kubectl logs <pod-name>`

If multiple containers in the Pod `kubectl logs <pod-name> <container-name>`

## Application Lifcycle Management

## Rolling Update and Rollback

When I first create a Deployment it triggers a `rollout`. 

New `rollout` create a new Deployment Revision 

- In the future if I update the Deloyment the new `Rollout` get triggered and the new Deployment Revision get created

- This help us keep track of our Deployment and Rollback to our previous version of Deployment if necessary 

To see the status of `Rollout` : `kubectl rollout status <Deployment name>`. 

To see the revisions and history of `Rollout`: `kubectl rollout history <Deployment Name>` 

There are 2 types of Deployment Strategy 

- For example I have 5 replicas of my Web app instance deployed . One way to update a newer version is to destroy all of these and then create newer versions of Applications instances . The problem with this is Application Down during the period of time creating a new one

- Second Strategy that we don't destroy all of them at once instead we take down the older version and bring up the newer version one by one

!!! If I am not specify a strategy while creating the deployment, it will assume it to be `Rolling Update` (this is a Default Deployment Strategy)

To see the difference between the recreate and rolling update strategies can also be seen when I view the Deployment in details : `kubectl describe deployment` to see detail information regrading the deployments

**Deployment Upgrades Under the Hood**

When the new Deployment created , Deploy 5 replicas 

- It first create a replicaSet automatically which in turn create the number of pods required to meet a number of replicas

- When I upgrades my application the Kubernetes Deployment create a new ReplicaSet and start deploying the containers there at the same time taking down the Pods in the old ReplicaSet

- Once Upgrade application if something wrong with a new Version Application I can `Rollback` my upgrades : `kubectl rollout undo <deployment name>`

## Command and Arguments Docker

For example I run an Ubuntu Image `docker run ubuntu` it run an ubuntu instance and exit immediately . 

- If I were to list a running container `docker ps` I won't see it

- If I list all container including stopped container `docker ps -a` I will see the ubuntu container in an `Exited State`

- Bcs Unlike Virtual Machine container are not meant to host an OS , container meant to run specific task or process like Application, DB or carry out some computation or analysis. Once task completed the container get exited . The container only live as long as the process inisde it is alive

The `CMD` in Dockerfile define process is run within the container within the container when it starts 

When I ran the Ubuntu container earlier, Docker created the container from the Ubuntu image and launched the bash program, by default Docker does not attach a terminal to a container when it is run and so the bash program does not find the terminal so it exited 

To specify a diffent Command to start a Container 

- Option 1: Append the command to docker command . This way it overrides the defaults command specified within the image Example : `docker run ubuntu sleep 5` (Docker run ubuntu image sleep for 5s then exited)

  - To make that changed permantnent . If I want the Docker image to run sleep command when it starts I would create my image from based Ubuntu Image and specify `CMD`
 
```
# ubuntu-sleeper

FROM ubuntu

CMD sleep 5
```
- To change a number of second : `docker run ubuntu-sleeper sleep 10`

If I want to pass in only the number of second the container should sleep like this : `docker run ubuntu-sleeper 10` . I can use the `ENTRY POINT` instead of `CMD`

- `ENTRY POINT` instruction is like `CMD` I can specify the program that will be run when the container starts . Whatever I specify in the command line it will be appended to the `ENTRY POINT`

To configure a default value for the command if one was not specified in the command line I can use both `ENTRY POINT` and `CMD`

```
# ubuntu-sleeper

FROM ubuntu

ENTRYPOINT ["sleep"]
 
CMD ["5"]
```

- In this case the CMD instruction will be appended to the entrypoint instruction

!!! NOTE : Should alway specify the `ENTRY POINT` and `CMD` in the JSON format 

To modify `ENTRY POINT` during runtime I can override it `docker run --entrypoint sleep2.0 ubuntu-sleeper 10`

## Commands and Arguments in Kubernetes

To specify additional argument in the pod definition file :

- Anything that is appended to the `docker run` command will go into `args: ["10"]` in the pod definition file

```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
  - name: ubuntu-sleeper
    image: ubuntu-sleeper
    args: ["10"]
```

To relate that to the Dockerfile we created earlier . The Dockerfile has an `ENTRYPOINT` as well as a `CMD` specified

- The `ENTRYPOINT` is the command that is run at startup

- `CMD` is a default parameter passed to the command

With the `args` options in the Pod definition file, we override the `CMD` instruction in the Dockerfile 

If I need to override the `ENTRYPOINT` I can use `command: ["sleep2.0"]`

- The `command` corresponds to `ENTRYPOINT`

```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
  - name: ubuntu-sleeper
    image: ubuntu-sleeper
    command: ["sleep2.0"]
    args: ["10"]
```

## ConfigMap

ConfigMap is use to pass configuration data in the form of key-value pair in Kubernetes 

To create Configmap using kubectl `kubectl create configmap [configmap-name]  --from-literal=APP_COLOR=blue`

- `--from-literal` : is use to specify the Key=Value pair in the command itself . If I want to create another Key=Value pair I can create multiple `--from-literal` multiple times

Another ways is to input configuration data is through the file . `kubectl create configmap [configmap-name] --from-file=app_config.properties`

## Secret 

To create Secret : `kubectl create secret generic --from-literal=[key]=[value]`

Another ways is to use : `kubectl create secret generic --from-file=[path-to-file]`

To encode Secret Value `echo -n [secret-value] | base64`

To decode Secret Value  echo -n [secret-encoded-value] | base64 decode`

To mount Secret as a Volume in the Pod . Each attribute in the Secret is created as a file with the value of the secret as its content 

- If we have 3 attribute in the Secret . 3 files will be created

Docs for Encrypted Data At Rest (https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)

##  Secret Store CSI Driver

(https://www.youtube.com/watch?v=MTnQW9MxnRI)

If want to use tools like Vault, AWS Secret Manager ... I have to use the Secret Store CSI Driver to pull those Secret into my Native Kubernetes 

Secret Store CSI Driver synchronizes Secrets from external APIs and mounts them into containers as `Volumes` 

I don't need to worry about checking Secrets into Git 

The main Advandtage of use Secret Store CSI Driver is we No longer store Credentials in Kubernetes Secrets 

If I use external `Secrets Operators` or `Seal Secret` what will happen is we will grap our Secret from a Central Secret Store and I will sync it with Kubernetes Secret 

With the Secret Store CSI Driver I don't actually create a Kubernetes Secret . It just going to pull it dynamically from my Central Secret Store 

- The benefit of when we don't create a Kubernetes Secret is minimize attack surface as much as possible

- Great from a compliance and regulatory perspective 

How does Secret CSI Driver work ? 

We will use Helm to install CSI Driver . 

What it will install ? 

- A couple of `CRD` 

Let's say I have my Secrets store in AWS Secrets Manager . I have to tell Secret store CSI Driver what Secret do I want synced and how do we actually access those Secrets .

- So I can use the CRD provided by Helm Chart called `SecretProviderClass`

```
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: db-secrets
spec:
  provider: aws
  parameters:
    objects: |
      - objectName: "db-creds"
        objectType: "secretmanager"
```

<img width="600" height="689" alt="Screenshot 2025-07-23 at 12 39 44" src="https://github.com/user-attachments/assets/48f6120a-b496-410c-be44-838b53d4f940" />

- Next we have to create our podspec and basically tell the Pod that we want this specific Secret mounted within the container within that Podspec .

- In my Pod Spec I will create a Volume like normal  
  
<img width="600" height="666" alt="Screenshot 2025-07-23 at 12 42 42" src="https://github.com/user-attachments/assets/5087a177-eb16-4552-8edb-fbd201abe5d4" />

- `secretProviderClass : db-secrets` : This name reference from the name of the `SecretProviderClass` . This say like Whatever Secret we pull from AWS by using `SecrectProviderClass` I want to mount that in the specific container

After we create our Pod Spec and then we actually deploy it to Kubernetes what will happen is the `Kubelet` is responsible for create that Pod and it will see that we want a volume created and so it will talk to that Secret Store CSI Driver bcs it see the configuration for that, the CSI then see configuration of that and see that it points to the Secret Provider, Then the `SecretProvider` will talk to AWS to get the value of the Secrets and return to CSI Driver and now we can create our pod and the CSI is going to Mount the Volume onto that Pod and that volume will contain the Secret 

<img width="600" height="569" alt="Screenshot 2025-07-23 at 12 51 46" src="https://github.com/user-attachments/assets/b9f07f5f-11a8-42aa-9361-40d910233743" />


### AWS Secret Manager 

I will go to AWS Secret Manager UI to create my secret 

- Click Store new Secret -> Choose Other type of Secret -> Then I will put my Secrets as /Key Value pair 

Secret Store CSI Driver Docs (https://secrets-store-csi-driver.sigs.k8s.io/)

I will use Helm to install CSI Driver . See in the Docs 

To see `CRD` that provided by CSI Driver Helm Chart I can do : `kubectl get crd -n kube-system`

```
secretproviderclasses.secrets-store.csi.x-k8s.io      
secretproviderclasspodstatuses.secrets-store.csi.x-k8s.io
```

I the docs I can see some `Optional Values` 

- `Sync as Kubernetes Secret` What it does is it will create a Kubernetes Secret and keep it synced up with the Secret that is in AWS 

- `Secret Auto rotation`: Anytime I make change in the Secret Store it will continuously pull the Secret Store to see if there is any changes and then update Secret within my Pod accordingly

Set up AWS intergration with the Secret Store CSI driver (https://github.com/aws/secrets-store-csi-driver-provider-aws)

The way that the AWS Provider works in The Secret Store CSI driver is that it allows us to do is actually bind in a to a Kubernetes `ServiceAccount` . That's specifically with EKS Cluster 

First I will create my Policy : I will go to IAM -> Policy -> Secret Manager -> Choose READ -> Describe Secret and GetSecret Value -> Specify Resources -> Region us-west-1 | Secret my-db-creds 

Second I have to create an IAM `ServiceAccount` this will bind that iam policy to `ServiceAccount` that exists on our Kubernetes Cluster so it will create `Service Account` and then assign the necessary permission that way our Pod can then retrieve the Secrets from AWS : `eksctl create iamserviceaccount --name api-sa --region=us-west-1 --cluster eks-cluster --attach-policy-arn arn:aws:iam::660753258283:policy/csi-eks-access-secrets-manager --approve --override-existing-serviceaccounts` 

- To see that Service Account : `kubectl get sa`

Now I will configure Secret Store CSI driver to talk to AWS by using : `SecretProviderClass`

```
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: db-aws-secrets
spec:
  provider: aws
  parameters:
    objects: |
      - objectName: "my-db-creds"
        objectType: "secretsmanager"
```

This is my example Deployment 


## Encrypting Secret Data at Rest

```
kind: Service
apiVersion: v1
metadata:
  name: nginx-irsa-deployment
  labels:
    app: nginx-irsa
spec:
  selector:
    app: nginx-irsa
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-irsa-deployment
  labels:
    app: nginx-irsa
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx-irsa
  template:
    metadata:
      labels:
        app: nginx-irsa
    spec:
      serviceAccountName: api-sa-allow-all
      volumes:
      - name: secrets-store-inline
        csi:
          driver: secrets-store.csi.k8s.io
          readOnly: true
          volumeAttributes:
            secretProviderClass: "db-aws-secret"
      containers:
      - name: nginx-irsa-deployment
        image: nginx
        ports:
        - containerPort: 80
        volumeMounts:
        - name: secrets-store-inline
          mountPath: "/mnt/secrets-store"
          readOnly: true
```

Now let's say I have to change my DB password 

- I go to my AWS Secret Manager -> My Secret - and Edit my password . But the Secret will not automatically update in my Kubernetes Pod

To make a Kubernetes automatically update my Secret I will use `Secret Auto Rotation`. This will continuously monior AWS and see that if there is a change in my AWS Secret it will automatically update for me .

To get a values file of the csi driver helm chart : `helm show values secrets-store-csi-driver/secrets-store-csi-driver >> values.yaml`

When I have a values.yaml file I will set `enableSecretRotation: True`

Then I will upgrade a Helm `helm upgrade <name-release> <name-of-chart> --values values.yaml -n kube-system`

## Multi Container

We need one web Server per main app instance pair together so they can scale up and down together that's why we have multi container Pod that share the same lifecycle 

- They share the same network space

- They can refer to each other as localhost

- They can share the same storage volume

```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
spec:
  containers:
  - name: web-app
    image: web-app
    ports:
      - containerPort: 8080
  - name: main-app
    image: main-app
    ports:
      - containerPort: 3000 
```

## Multi container Design Pattern 

The original form of Multi Container : As simple as 2 containers running in the same Pod . Usually use when 2 containers are depend on each other 

Next we have `initContainer`: This used when there are initilazion step need to perform when a Pods start before the Main Application itself 

- For example there could be a `initContainer` that wait for the DB ready before starting the main application . The `initContainer` start it job and end its job and then main application start

Next we have `Side Car Container`: Sidecar container is set up like a `initContainer` where the sidecar start first, does its job but instead of ending, its continue to run throughout the life cycle of the pod . The main container Start after the side car container start 

- This is useful when I have a logs shipper of sorts that needs to be run along with the main application that need to start before the main app start continue to run after the main app run and end after the main app end 

Example of `initContainer`

```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
spec:
  containers:
  - name: web-app
    image: web-app
    ports:
      - containerPort: 8080
  initContainers:
  - name: busy-box
    image: busybox
    command: 'wait-for-db-to-run.sh'
```

Example of `Side Car Container`

```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
spec:
  containers:
  - name: web-app
    image: web-app
    ports:
      - containerPort: 8080
  initContainers:
  - name: log-shipper
    image: log-shipper
    command: 'set-up-log-shipper.sh'
    restartPolicy: always
```

## AutoScaling

When the loads increases and run out of existing resources on the Server . We need to scale up the Server by took down the Application and added more CPU or Memory resources to it and then powered it back up (Vertical Scaling)

If the application supported running in multiple instances, another thing we could have done is we could have avoided having to shut down the Server . We could add more Server to it and shared load between them (Horizontal Scaling)

One of the major purposes of Kubernetes is to host applications in the form of Containers and scale up and down based on demand `(Scaling Workloads)` . 

Another Scale is `Scaling the Underlying Cluster` itself so this is adding or removing more Servers 

There are two ways of Scaling those `(Scaling Workloads)` and `Scaling the Underlying Cluster` so that is Horizontal and Vertical Scaling 

`Scaling Cluster Infra`: 

- Horizontal Scaling refers to adding more Nodes to the Cluster

- Vertical Scaling refer to increasing resources to Nodes in the Cluster


`Scaling Workloads`:

- Horizontal Scaling refer to creating more pods

- Vertical Scaling refer to increasing resources allocated to existing Pods 

There are 2 ways of Scaling : `Manual way` and `Automated way`

Manual way of Horizontal Scaling Cluster Infra would be to provision manually provision new Nodes and then use the `kubeadm join ` command to add new Node to the Cluster 

Manual way to Horizontal Scaling Workloads is to run `kubectl scale ` to scale up or down the number of Pods 

Manual way to Vertical Scaling Workload is to run `kubectl edit ` command to go in to that Deployment or StatefulSet or ReplicaSet and change the Resource Limits and Request associated with the Pods 

Automate way of Horizontal Scaling Infra is `Kubernetes Cluster Autoscaler`

Automate way of Horizontal Scaling Workloads is `Horizontal Pod Autoscaler`

Automate way of Veritcal Scaling Workloads is `Vertical Pod AutoScaler`

## Horizontal Pod AutoScaler 
 
To monitor the resource consumption of the Pod `kubectl top pod <my-app-pod>` (Must have Metrics Server) to be able to monitor Pod Resource

To manual way to scale a Workload  : `kubectl scale deployment my-app --replicas=3` (Not Recommended)

I will use Horizontal Pod Autoscaler to continue monitor the Metrics as we did manually using the `top` command. It the automatically increase or decrease the number of pods in a Deployment, StatefulSet base on the CPU memory or custom metrics and this balance the Thresholds and also track multiple different types of metrics 

To configure Horizontal Pod Autoscaler by running the `kubectl autoscale deployment my-app --cpu-percent-50 --min=1 --max=10`. With this command Kubernetes will create a Horizontal Pod AutoScaler for this Deployment that first read the limits configure on the Pod, it thencontinuously pulls the metrics server to monitor usage, and when the usage goes beyond 50% it mofiies the number of replicas to scale up or down (This is a imperative approach)

To view status of HPA : `kubectl get hpa`

- The `Targets` colume show the current CPU usage

To delete HPA : `kubectl delete hpa my-app`

To create HPA with Declarative approach . Create HPA Definition file with APIversion 

```
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

`scaleTargetRef`: This is the target Resource we want the HPA to monitor 

`min and max` replicas

NOTE : `HPA` relies on the Metric Server 

Custom Metric Adapter that can retrieve information from other internal soruces like workload deployed in a Cluster 

External soruces such as : Datadog, dynatrace 

## In place Resize Pod

If I changed Resource Requirement of a Pod in a Deployment the default behavior is to delete the existing Pod then spin up a new Pod with new changes 

There is an improvement worked upon called `In-place update of resources`. This is a feature currently in alpha . Not enable by default . 

To enable this feature : `FEATURE_GATES=InPlacePodVerticalScaling=true` . Once this is enable the Pod defination support a set of resize policy parameter 

```
resizePolicy:
- resourceName: cpu
  restartPolicy: NotRequired
- resourceName: memory
  restartPolicy: RestartContainer
```

`resizePolicy` option allow me to specify a restart policy for each resource. We have defined that a change in CPU resources will not require the Pod to be restart 

Limitation :

- Only work for CPU and Memory resources

- Pod QoS Class can not be changed

- InitContainer and Ephemeral can not be resize

- Resource requests and limits cannot removed once set

- A container's memory limit may not be reduced below its usage. If a request puts a container in this state, the resize status will remain in InProgress until the desired memory limit become feasible 

## Vertical Pod Autoscalser

To monitor the Pod consumption resource : `kubectl top pod my-app-pod`

To scale the Pod Vertically : `kubectl edit deployment my-app`. I will change the resoures limit and requests . It will kill the Pod and create a new Pod 

Vertical Autoscaler will continuously monitors the metrics and then it automatically increase or decrease the resource  assigned to the Pod in the Deployment and then Balance the Workload 

Vertical autoScaler does not come built-in 

To apply Vertical Pod Autoscaler : `$ kubectl apply -f https://github.com/kubernetes/autoscaler/releases/latest/download/vertical-pod-autoscaler.yaml`

To see the Pods `kubectl get pods -n kube-system | grep vpa`

The VPA deployment consist of multiple components : `VPA Admission Controller`, `VPA Updater`, `VPA Recommender`

`VPA Recommender` : responsible for continuously monitoring resource usage from the Kubernetes metrics API and collect historical and live usage data for pods and then provide recommendation on optimal CPU memory values (Only suggest changes)

`VPA Updater`: Detect Pod that are running with suboptimal resources and evicts them when update is need . It get information from Recommender and monitor the Pod and If the Pod need to be updated, it evict them 

`VPA Admission Controller` intervenes the Pod creation process and uses the recommendation from the Recommender to then mutate the pod spec to apply the recommended CPU and memory value at startup . This ensure the newly created pods with the correct resource requests

```
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: my-app-vpa
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  updatePolicy:
    updateMode: "Auto"
  resourcePolicy:
    containerPolicies:
      - containerName: "my-app"
        minAllowed:
          cpu: "250m"
        maxAllowed:
          cpu: "2"
        controlledResources: ["cpu"]
```

`kubectl describe vpa my-app-vpa`

When to use which Horizontal and Vertical 

## Cluster Maintainence

## OS Upgrade

What happen when the Nodes gone down ? 

- The Pods on them will now accessible . Users will be impacted

- For example If that Pod only run in that Node users will be impacted

- If the Node come back online immediately the `kubelet` process starts and the Pod come back online . However If the Nodes down for more than 5 mins then the Pods are terminated from that Node . Kubernetes consider them as dead .

- If the Pod were part of ReplicaSet then they are recreated on other Nodes the time to wait for the Pod to come back online is known as the `Pod-eviction-timeout` and it set in the `Controller Manager` : `kube-controller-manager --pod-eviction-timeout=5m0s` . So whenever the Node goes offline, The master Node wait for 5 mins before consider the Node dead

- When the Nodes come back online after `Pod-eviction-timeout` it come back blank without any Pod sheduler on it

- If the Pod not part of ReplicaSet it just gone .

- Thus If I have maintainence task to be performed on a Node . If I do not know if the Node back up online in 5 mins, I can not for sure say I will be back at all

The Safer way to do it is drain of all the Workloads so that Workloads are moved to other Nodes in the Cluster : `kubectl drain node-1`

- When I drain the Node the Pod gracefully termiated from the Node that they are on and recreated on another

- The Node is also cordoned or marked as unschedule, meaning no Pod can be schedule on this Node untill specifically remove the restriction .

- Now the Pod are safe on the other Nodes I can reboot the First Node

- When it comeback online it is still unchedulable then I need to `kubectl uncordon node-1` so the Pod can schedule on this Node .

- The Pod that are moved to other Nodes don't automatically fall back 

`kubectl cordon node-1` simple mark the Node unscheduable . It does not termiate or move the Pod to an existing Node It just make sure new Pod not shedule on that Node 

## Kubernetes Release

## Cluster Upgrade Process 

The components can be at diffent release versions  

`Kube-Apiserver` is a primary Component in the Control Plane bcs that is a Components that other Componets talk to . None of others Components should be in a higher Version than `Kube-Apiserver`

If `Kube-Apiserver` = `X` -> `Controller Manager` and `Kube-scheduler` can be = `X - 1` -> and `Kubelet` and `Kube-proxy` can be = `X - 2`

But `kubectl` can be higher or lower than `Kube-ApiServer` 

This permissible skew in Versions allow us to carry out live upgrades. We can upgrade component by component if required 

I should Upgrade when: 

- Let's say I'm at 1.10, Kubernetes release versions 1.11 and 1.12. At any time, Kubernetes support only up to the recent 3 minor versions . So if v1.12 is a latest Kubernetes support v1.11 and v1.10 and v1.12 . So before v1.13 release would be a good time to upgrade Cluster to the next Release 

 To upgrade as kuadm: 

 - The recommend approach is to upgrade 1 minor version at the time

 - I have a cluster with master and worket nodes, running in Production, hosting Pods. serving users . The Nodes and components are at version 1.10 . Upgrade Cluster invole 2 major steps

 - First upgrade Master Node and then upgrade Worker Node

`kubeadm` has an upgrade command that helps in upgrading Cluster `kubeadm upgrade plan` (`kubeadm` not install or upgrade `kubelet`)

To update `kubeadm` version `apt-get upgrade -y kubeadm=1.12.0-00`

Then upgrade the Cluster using : `kubeadm upgrade apply v1.12.0`. It pull the nessesary image and upgrades the Cluster components 

If I run `kubectl get nodes` I still see master node at `1.11` bcs in the output of this command, it is showing the versions of `kubelets` on each of these nodes registered with the `API server` 

Next we should upgrade `kubelet`. Cluster deployed using `kubeadm` has `kubelets` deploy on master node : `apt-get upgrade -y kubelet=1.12.0-00` Once the package upgraded restart the `kubelet` service `systemctl restart kubelet`

Now I will upgrade the Worker Nodes . 

- First I need to move the work loads from first Worker Node to the other Nodes : `kubectl drain node-1` . Then upgrade the `kubeadm` : `apt-get upgrade -y kubeadm=1.12.0-00`  then upgrade the `kubelet`: `apt-get upgrade -y kubelet=1.12.0-00`. And then update the Node configuration for the new kubelet version `kubeadm upgrade node config --kubelet-version v1.12.0` then Restart the `kubelet` service : `systemctl restart kubelet` then `uncordon` the node : `kubectl uncordon node-1`

!!! Note: Not nessessary that the Pods come right back to this Node . It is only marked as scheduable . Only when the Pods are deleted from the other nodes or when new pods are scheudled 

## Demo Cluster Upgrade

Docs to upgrade the Cluster (https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

Docs to install `kubeadm` (https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl)

To see what distribution my servers are using : `cat /etc/*release*`

Download the public signing key for the Kubernetes package repositories. : `curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg`

Add the appropriate Kubernetes `apt` repository.: `echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list`

Upgrade kubeadm: 

```
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.33.0-*' && \
sudo apt-mark hold kubeadm
```

Upgrade `kubelet` and `kubectl`: 

```
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.33.0-*' kubectl='1.33.0-*' && \
sudo apt-mark hold kubelet kubectl
```

## Backup and Restore Method 

`ETCD` cluster is all cluster-related information is stored . If my applications are configured with persistent storage then that os another candidate for backups 

We used the `Declarative Approach` by create the defination `yaml` file then running the `kubectl apply -f` (Recommended)

- With this approach I have all the Objects required for single Application in the form of object definition files in a single folder . This can be reuse

- I must have a copy of these files saved at all times . Good practice is to store these on Souce code like  Github .

- Source code Repo should be configured with the right backup Solutions with managed or public source code Repositories like Github I don't have to worry about it

- With that even when I loose my entire Cluster I can redeploy my application on the cluster by simoly applying these configuration files on them 

A better approach to backing up resource configuration is to query the KubeAPI server : `kubectl get all --all-namespaces -o yaml >> all-deploy-services.yaml`

- There are tools like Velero by Heptio they  can help taking backups of my Kubernetes Cluster using the Kubernetes API 

**Back up ETCD**

ETCD Cluster store information about the state of my Cluster . Instead of backing up resource as before, I may choose to backup the `ETCD` server itself 

The `ETCD` Cluster is hosted in Master Nodes . While configuring `ETCD` we specified a location where all the data would be stored, the data directory `--data-dir=/var/lib/etcd` This is a directory that can be configured to be backed up by my backup tool 

`ETCD` come with `built-in` snapshot solution 

- To take snapshot of ETCD database : `etcdctl snapshot save snapshot.db`

- To view a status of the backup : `etcdctl snapshot status snapshot.db`

To restore the Cluster from backup

- First stop the kube-apiserver : `service kube-apiserver stop` as the restore process will require me to restart the etcd cluster and the `kube-api`server depends on it

- Then run `etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup`

When etcd restores from a backup it initialize a new cluster configuration and configures the memebers of etcd as new members to a new Cluster this to prevent a new member from accidentally join a new cluster 

We then configure the etcd configuration file to use a new data directory : `--data-dir=/var/lib/etcd-from-backup`

Then reload the service daemon `systemctl daemon-reload` then restart `service stcd restart`

Finally start kube-api service : `service kube-apiserver start`

NOTE : With all the etcd command 

- Remember to specify the certficate files for authentication

- Specify the endpoint to the etcd cluster and CS certificate

- the etcd server certificate and the key

## Working with ETCDCTL and ETCDUTL

(https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/18478622#overview)


- Back up etcd volume
  
```
etcdctl \
--endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
snapshot save /opt/snapshot-pre-boot.db
```

- Check snapshot status :
  
```
etcdctl snapshot status /opt/snapshot-pre-boot.db \
  --write-out=table
```

- Restore etcd
  
```
etcdutl --data-dir /var/lib/etcd-restore snapshot restore /opt/snapshot-pre-boot.db
```

I create new `--data-dir /var/lib/etcd-restore` so I have to update in `/etc/kubernetes/manifests/etcd.yaml` 

- Need to update the `--data-dir`, `volume-mount-path`, `volumes` location

Then I have to restart `kubeapi`, `controller-manager` and `scheduler` . Bcs those are Static file so I will move those `mv /etc/kubernetes/manifests/*.yaml /tmp/` then I will move it back 

Then reload the service daemon `systemctl daemon-reload`

## Security Primitives

**Secure Hosts**
All access to the Hosts must be secured 

- Root access disable

- Password based authentication disabled

- and only SSH key based authentication to be made available

**Kube-apiserver**

Center of all operations within Kubernetes . Through that we can perform any operations in the Cluster

**We need to make 2 type of decisions :**

Who can access ?

- Define by the Authentication Mechanisms

- I can authenticate with User ID and password stored in statics file, tokens, certificates, or External Authentication providers (LDAP)

- For machine or Applications I create `Service Account`

And what can they do ?

- Defined by Authorization mechanisms RBAC

- Users associated to groups with specific Permissions

- Others Authorization like : ABAC, Node, Webhook Mode authorization  


All communications within the Cluster between the various components such as the ETCD Cluster, the Kube-Controller, the Schedulers, API Server and on Worker Nodes, Kubelet and KubeProxy is secured using TLS encryption 

**Communications between Applications within the Cluster**:

By default all Pods can access all other Pod within the Cluster 

I can restrict access between them using `Network Policies`

## Authentication 

To secure the Cluster by securing the communication between internal components and securing management access to the Cluster through authentication and authorization mechanisms 

Human users such as Admin and Developers 

Applications Users such as services or applications need to do something in the Cluster  

Kubernetes do not manage Users natively . It depend on external source like file with user detail or certificates, or a third party identity service like LDAP to manage these users 

In case of `ServiceAccount` Kubernetes can manage them . 

All user access is managed by the `API-server` . Wheather I am accessing the Cluster through `kubectl` or `api` directly

**For `Kube APIserver` authenticate:**

- I can have a list of username and password in a static password file or static token file

  - I can create a list of user and passowrd in `.csv` file and use that as a source for user information .
 
  - File has 3 columes: password, username, user ID .
 
  - Then I pass the file as an option to `Kube APIserver` : `--basic-auth-file=user-details.csv`
 
  - To authenticate using the basic credentials while accessing the API server, specify user and password in curl command `curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123"`
 
  - I can also have a fourth colume with the group detailes to assign users to specific groups 
 
```
user-details.csv

password123,user1,u0001,group1
password456,user2,u0002,group2
``` 

    - To authenticate with a token : `curl -v -k https://master-node-ip:6443/api/v1/pods --header "Authorization: Bearer KpjCVbI7cFAHYPkByTIzRb7gulcUc4B"`

```
user-token-details.csv

KpjCVbI7cFAHYPkByTIzRb7gulcUc4B,user10,u0010,group1
rJjncHmvtXHc6MlWQddhtvNyyhgTdxSC,user11,u0011,group1
mjpoFTEiFOkL9toikaRNTt59ePtczZSq,user12,u0012,group2
PG41IXhs7QjqWkmBkvGT9gclOyUqZj,user13,u0013,group2
```

!!! NOTE If try the authenticate user in the `kubeadm` setup, I must also consider volume mounts to passing the auth file (https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/14296208#overview)

- Or I can authenticate using certificate


- Or I can connect to third party authentication protocol like LDAP 

## TLS 

Certicate use to garuantee trust between two parties during a transaction  

- For example the communication between User and Server, TLS ensure the communication is encrypted

**Asymmetric EnCryption**: Instead of using a single key to encrypt and decrypt data, this use a pair of keys 

- Private key and Public Key

- The key only with me so it's private , Public key is anyone can access

- If I encrypt the data with the Public Key I can only open it with a Private Key . (Never share Private Key)

- For example : An use case of SSH server . I generate an Public key and the Private key . I will give an copy of my Public Key to a Server (locate at `cat ~/.ssh/authorized.key`) . Then I use my Private Key to SSH to the Server . If I want to ssh to another Server with the same Private key I must copy another Public key (locate at `cat ~/.ssh/authorized.key`) and put it in the other Server

- If ther User want to access my Server . They can generate they are own Public and Private key and put their Public key on my Server at `cat ~/.ssh/authorized.key` 

The problem with **Symmetric EnCryption** was that the key used to encrypted data have to be sent to the Server over network along with encrypted data . So the risk of the Hacker to get the key to decrypt data 

To securely transfer the Sysmmetric key from Client to Server we use **Asymmetric EnCryption**

- We generate Public and Private key pair on the server

- We will use the Open SSL: command `openssl genrsa -out my-bank.key 1024`, `openssl rsa -in my-bank.key -pubout > mybank.pem` to generate Private and Public key

- When user first access the web Server using `HTTPS` he get the Publics key from the Server . Let's say hacker also get the `Public Key` 

- The user's browser then encrypt the `Sysmmetric key`  using the Public Key provided by the Server . The `Sysmetric Key` now secure

- Users then send `the Encrypted his Private key and Public key (from a server)` back to the Servers . Hacker also have the copy

- The Server then use the private key to decrypt the message and retrieve the `Symmetric key` from it

- However the hacker does not have the `Server's Symetric Key` to decrypt and retieve the `User's Symetric Key`

- The `User's Sysmetric Key` only now available to User and Server . They can now use `Sysmetric Key` to encrypt data and send to each other

- Now the Server can use the `User's Symetric key` to decrypt data and retrieve information
 
But now Hacker create his own website that look exactly like my Bank website for example . He host his website on his own Server 

- So now he generate his own `Private` and `Public` and configure them on his Web Server

- Finally he somehow manages to tweak my Environment or my Network to route my request going to my bank's website to his Server

- Now me as User has his Server Public key sent over . Then I will send `User's Sysmetric Key` to his Server .

- Now he has `User's Symetric Key` then User sent `Encrypted Data with User's Symetric Key` to his Server, he now can decrypted that with `User's Sysmetric Key`

- User have been communicate securely but with the Hacker Server 

What if I could look at the key I received from the Server and say If it is a `Legitimate Key` from real Bank Server ? 

- Now Certificate come in to play

- Server will send the Certificate that has the Key in it 

How do we look at a Certificate and verify if it is Legit ? 

- The most Important is who signed and issued the Certificate

- My Browser will look at the Certificate and check Who signed that Certificate Bcs All of the Browsers built in with a certificate validation mechanism while in the browser text the Certificate received from the Server and Validates it to make sure it is Legitimate

- If it identifies it to be a fixed certificate, then it will warn me

How do I create my Legit Certificate for my Server that Browser will trust ? 

- This is  where Certificate Authorities or CA come in . They are well known organization that can sign and validate my Certificate for me (Symantec, DigiCert, Comodo, Globalsign)

- They it work is I generate a Certificate sigining a Request (CSR) using the key generated earlier and the domain name of my Website : `openssl -new -key my-bank.key -out my-bank.csr -subj "/C=US/ST=CA/O=MyOrg, Inc./CN=mydomain.com"`

- The CA verify my details and once it checks out, they sign the Certificate and send it back to me

- If hacker tried to get his certificate signed the same way, he would fail during the validation phase and his certificate would be rejected by CA . So the website he hosted won't have the Valid Certificate

How do Browser know CA itself was Legitimate ? 

- The CA themelves have a set of `Public Private key pairs`

- CA use their Private Key to sign the Certificates . The public keys of all the CAs are built in to the browser .

- The Browser uses the Public Key of the CA to validate that the Certificate that actually signed by the CA themselves

- I can see them in my Web Browser setting under Certificates .

- These are Public CA help us ensure the Public websites we visit .

However Public CA don't help me validate sites hosted privately . For example for accessing my payroll or internal email application .

- For that I can host my own Private CAs

- Most of those companies have a Private Offering of their Services, a CA server that I can deploy internally within my company

- I can then have the Public Key if my Internal CAs Server installed on all my employees browsers and establish secure connectivity within your organization

The Server's keeper, the client was able to validate that the Server is who they say they are . But the Server does not know if the Client is who they say they are . It could be a hacker impersionating a user by somehow gaining access to his credentials (not over the network) . So what can the Server do to validate that the Client is who they say they are ? 

- As part of the initial trust building exercise, The Server can request a certificate from the Client . And so the Client must generate a pair of keys and a signed Certificate from a valid CA . The client then sends the Certificate to the Server for it to verify that the Client is who they say they are

- TLS Certificate client certficates are not implement on the Web Server . Even if they are, it's all implemented under the hood, So a normal user don't have to generate and manage certificate manually 

This whole Infrastructure, including the CA, the Servers, the People, and the Process of generating, distributing and maintaining digital Certificate is known as `Public Key Infrastructure or PKI`

## TLS in Kubernetes

The Kubernetes Cluster consist of set of Master and Worker Nodes . All communication between these Node need to be secure and must be encrypted 

All interaction between all Services and their Clients need to be secure 

For example : Administrator interacting with the Kubernetes Cluster through the `kubecctl` or while accessing the Kubernetes API directly must establish secure TLS connection 

2 Primary requirement to have all the Various Services within the Cluster to use `Server Certificate` and Client to use `Client Certificate` to verify they are who they say they are 

With `Kube-apiserver`, the `Kube-API server` exposes an HTTPS service that other components, as well as external users use to manage the Kubernetes Cluster 

- So it is a Server and it requires certificates to secure all communication with its client

- So we generate a certificate and key pair. We call it `apiserver.cert` and `apiserver.key`

- Another Server in the Cluster is the `etcd Server` . Its store all Cluster information . It's require a pair of certifcate and key for itself, `etcdserver.crt` and `etcdserver.key`

- The other Server component in the Cluster is on the Worker Nodes . `Kubelet` also expose on HTTPS API endpoint that `Kube-api Server` talk to also required pair of Certificate and Key . `kubelet.crt` and `kubelet.key`

Clien Components: 

- Clients who access the `Kube-api Server` are us Administrator through `kubectl`.

- Administrator required certificate and key pair to authenticate the the `Kube-API server` . `admin.cert` and `admin.key`

- The `Scheduler` talks to the `Kube-Apiserver` to look for pods that require scheduling and then get the `API server` to schedule the Pods on the right Worker Nodes . The Scheduler is a Client that accesses the `kube-apiserver` . So Scheduler also need its own pair of certificate and keys . `schedudler.cert` and `scheduler.key`

- `Kube-Contoller Manager` is another Client also need pair of cert and key `kube-controller.cert` and `kube-controller.key`

- `Kube-proxy` is another Client  need pair of cert and key `kube-proxy.cert` and `kube-proxy.key`

`Kube Api Server` is the only Server that talk to `ETCD Server`

`Kube API Server` also talk to `Kubelet` Server on each of the individual Nodes 

**We need CA authority to sign all of these Certificates (I can have more than one)**

- CA has it's own pair of Certificate and key `ca.cert` and `ca.key`

## Kubernetes Certificate Creation

Tools generate Cert : `EasyRSA`, `openssl`, `cfssl`

**Client side Cert**

Start with `CA Certificate` :

- Create CA Private Key: `openssl genrsa -out ca.key 2048`

- Request to generate a Certficate signin Request : `openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr` . The certificate signing request is like a Certificate with all of my details but with not signature

  - We specify the name of the Component the certificate is for and the common name or CN field
 
- Sign Certificate : `openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt` .

  - Specify the Certificate Signing Request I generated previous command
 
  - It is self-signed by the CA using its own private Key that it generated in the first step
 
- Going forward for all other Certificates we will use CA key pair to sign them

- The CA now has its Private key and root certificate file

Now I will generating Client Certificate (Admin):

- Create Admin Private Key: `openssl genrsa -out admin.key 2048`

- Generate CSR (Certificate Signing Request): `openssl req -new -key admin.key -subj "/CN=KUBE -ADMIN" -out admin.csr`

- Generate Signed Certificate : `openssl x509 -req -in admin.csr -CA ca.crt -CAkey. ca.key -out admin.crt`

  - This time I will Sign using the CA certificate and CA key
 
To differenciate Admin user from any other users :

- The User account need to be identify as an Admin user and not just another basic user do that by adding the group details for the user in the Certificate

- In this case grouped named System Master exists on Kubernetes with Administrative privileges

- I must mention this information in my certificate signing request

- I can do that by adding group detail with `O` parameter while generating CSR : `openssl req -new -key admin.key -subj "/CN=KUBE-ADMIN/O=system:masters" -out admin.csr`.

- Once it signed we now have our Certificate for the Admin user with Admin Privileges

Follow the same process to generate Certificates for all other components that access the `Kube API` server 

`Kube Scheduler` is a system component part of Control Plane so its name must be prefixed with keyword `system`. The same with Controller Manager

What do we do with these Certs ? 

- For example Admin Cert . I can use this certificate instead of `username and password` in a REST API call :

```
curl https://kube-apiserver:6443/api/v1/pods \
--key admin.key --cert admin.crt
--cacert ca.crt
```

- The other way to is to move all of these Parameters to a `kubeconfig` file . Within that specify the API server endpoint

For the Client to validate the Certificate sent by the Server and vice versa, they all need a copy of the `Certificate Authority` . The one is already installed within user's browser in case of web . Similarly in Kubernetes for these various components to verify each other they all need a copy of the `CA's root certificate` 

**Server side Cert**

ETCD server : 

- Follow the same procedure as before to generate a Certificate for ETCD . `etcd-server`

- ETCD can be deploy as a Cluster accorss multiple Servers as in High Availability Environment. In that case to secure communication between the different members in the Cluster . We must generate addition `peer certificates`

- Once certificate are generated specify them while starting the `ETCD server`

- It requires `CA root certificate` to verify that the Client connecting to ETCD server are valid 

```
cat etcd.yaml

- --cert-file=
- --key-file=
- --trusted-ca-file=
- --peer-client-cert=
- --peer-key-file=
- --peer-trusted-ca-file=
``` 

`Kube API server`:

- We generate Certificate for API server like before

- Everyone talk to API server. Every Operation goes through API server

- The Kube API server is the primary access point for the cluster and is known by several aliases such as "kubernetes", "kubernetes.default", and "kubernetes.default.svc.cluster.local", as well as its IP address. This diversity requires that its certificate includes multiple Subject Alternative Names (SANs).

- Generate key : `openssl genrsa -out apiserver.key 2048`

- CSR: `openssl req -new -key apiserver.key -subj "/CN=kube-apiserver" -out apiserver.csr`

  - To specify all the alternate names I will create `OpenSSL config file` then pass this config file to CSR : `openssl req -new -key apiserver.key -subj "/CN=kube-apiserver" -out apiserver.csr -config openssl.cnf`
 
- Finally signd the certificate using the CA certificate and key : `openssl x509 -in apiserver.csr -CA ca.crt -CAkey ca.key -out apiserver.crt`
 
```
openssl.cnf

[req]
req_extensions = v3_req
distinguished_name = req_distinguished_name


[v3_req]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltN ame = @alt_names


[alt_names]
DNS.1 = kubernetes
DNS.2 = kubernetes.default
DNS.3 = kubernetes.default.svc
DNS.4 = kubernetes.default.svc.cluster.local
IP.1 = 10.96.0.1
IP.2 = 172.17.0.87
```  

Where to specify these keys : 

- To consider the API client Certificates that are used by the `API server` while communicating as a Client to the `ETCD` and `kubelet servers` . The location of these certificates are passed in to the `kube api server` executable or service configuration file

- `CA file` need to be pass in : `--client-ca-file=/var/lib/kubernetes/ca.pem`

- Then provide the `API server certificates` :

```
--tls-cert-file=/var/lib/kubernetes/apiserver.crt
--tls-private-key-file=/var/lib/kubernetes/apiserver.key
```

- Then specify `Client Certificate` used by `KubeAPI server` to connect to `ETCD` and again with `CA` file

```
---etcd-cafile=/var/lib/kubernetes/ca.pem
---etcd-certfile=/var/lib/kubernetes/apiserver-etcd-client.crt
---etcd-keyfile=/var/lib/kubernetes/apiserver-etcd-client.key
```

- And finally the `Kube API server` connect to `Kubelet`

```
--kubelet-certificate-authority=/var/lib/kubernetes/ca.pem \
--kubelet-client-certificate=/var/lib/kubernetes/apiserver-kubelet-client.crt \
--kubelet-client-key=/var/lib/kubernetes/apiserver-kubelet-client.key \
```

`Kubelet` Server 

- `Kubelet` run on each Node, responsible for managing the Node who `API server` talk to . To monitor the Node as well as send information regrading what pods to schedule on this Node

- I need a Key Cert pair for Each node in the Cluster

- What to name these Cert ? They will be name after their Nodes

- Once the Certificates are created, use them in the kubelet config file .

- Alway sepcify the root CA certificate

- We must do this for each node in the Cluster 

```
kubelet-config.yaml(node01)

kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
authentication:
  x509:
    clientCAFILE: "/var/lib/kubernetes/ca.pem"
authorization:
  mode: Webhook
clusterDomain: "cluster.local"
clusterDNS:
  - "10.32.0.10"
podCIDR: "${POD_CIDR}"
resolvConf: "/run/systemd/resolve/resolv.conf"
runtimeRequestTimeout: "15m"
tlsCertFile: "/var/lib/kubelet/kubelet-node01.crt"
tlsPrivateKeyFile: "/var/lib/kubelet/kubelet-node01.key"
```

We also talk about sort of Client certificates that will be used by the `Kublet` to communicate with the `KubeAPI server`. These are used by `Kubelet` to authenticate into the `Kube API server`. They need to be generated as well 

What do we name these cert ? The `API SERVER` need to know which node is authenticating and give it the right  set of Permissions so it requires the Nodes to have the right names in the right formats . Since the Nodes are system components like `kube-scheduler` and the `controller manager` the format start with the keyword `system:node:<node-name>`

For `API SERVER` to give it the right set of permission . The Nodes must be added to a Group named `System Nodes`

Once certificate generated they go into `kubeconfig` files as we disscuessed 

## View Certificate Details 

To perform a health check of all the Certificates in the entire Cluster 

First I need to know how do Cluster set up 

If I deploy my Cluster from srcatch I have to generate all the certificates by myself  

If I use **kubeadm** it take care of automatically generating and configuring cluster for me 

The idea to create a list of certificate files used . There are **Paths**, **Names** , **Organization**, **Issue**, **Expiration** 

For **kube-apiserver** : `cat /etc/kubernetes/manifests/kube-apiserver.yaml`

- The command used to start the **apiserver** has information about all the certificates it uses. Identify the certificate file used for each purpose and note it down

Next take each certificate and look inside it to find more details about that certificate : `openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout`. I provide a certificate file as input to decode the certificate and view details

- Start with a name on the Certificate under the **Subject** section: `Subject: CN=kube-apiserver`

- Then look for the **Alternative Name**

- Then check the **Validity** section of the certificate to identify the **Expire Date**

- And then the **Issuer** of the Certificate . This should be the **CA** who issued the certificate .. **Kubeadm** name **Kubernetes CA** as **kubernetes** 

**Inspect Service Logs**: 

- When running into issue I want to start looking at logs

- If I set up CLuster from scratch and the Service are configured as **native services** in the OS, I want to start looking at the service logs using the OS logging functionallity : `journalctl -u etcd.service -l`

- In case setup Cluster with **kubeadm** then the various components are deployled as Pods : `kubectl logs etcd-master`

- Sometimes if the core components, such as **Apiserver** or **etcd** servers are down . The **kubectl** won't function

- In this case I have to go one level down to Docker to fetch the logs : `docker logs <container-id>`

## Certificates API 

What if new Administator come in to my team and she need a pair of Certificate to access to my Cluster 

- She create her own private key and generate **Certficate Signing Request** and send it to me . Since I am the only Admin I then take the **Certificate Signing Request** to my **CA server**, get it sign by the **CA Server** using the **CA Server Private Key** and **Root Certificate** thereby generating a Certificate and then sends the Certificate back to her

- She now has her own valid pair of Certificate and key that she can use to access the Cluster

The certificates have a **validity period**, it end after a validity period of time . Everytime it expire we follow the same process of generating a new **CSR** and getting it signed by **CA**

**What is CA Server and where is it located in K8 Setup?**

**CA** is just a pair of key and certificate files we have generated . Whoever gain access to these pair of files can sign any Certificate for the Kubernetes environment 

These files need to be protected and store in a Safe Environment 

Let's say we place them on the Server that is fully secure . Now this Server become **CA Server** 

Everytime I want to Sign Certificate I only can do it by logging into that Server  

**We need the automate ways to generate the Certificate** 

With **Certificate API** I now send a Certificate Signing Request directly to Kubernetes through API call 

This time Admin receives a **Certificate signing request** instead of logging onto the Master node and signing the Certificate by himself, he creates a **Kubernetes API object** called **Certficate Signing Request** . 

- Once the **Kubernetes API Object** created all **certificate signing request**  can be seen by Admin of the Cluster .

- The request can be review and approve easily using **kubectl** command

- This Cert can be shared with the User 

User first create a **Key** : `openssl genrsa -out kelly.key 2048`

Then generate **Certificate Signing Request** using her **Key**: `openssl req -new -key kelly.key -subj "/CN=kelly" -out kelly.csr`, Then send the request to Admin 

Admin take the key create **CertificateSigningRequestObject** 

**request** where I specify the Cert signing request sent by user . Specify as **base64** encoded  : `cat kelly.csr | base64`

```
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: Kelly
spec:
  expirationSeconds: 600 # Seconds
  usages:
  - digital signature
  - key encipherment
  - server auth
  request:
    kelly.csr | base64 ## Its content 
```

Once Ojbect created all **Cert Signing Request** can be seen by Admin by : `kubectl get csr`. 

Identify the new Request and Approve the Request : `kubectl certificate approve kelly`

Kubernetes signs the Certificate using the **CA key pairs** and generate Certificate for the User . This Cert then can be extracted and shared with the User 

To view Certificate : `kubectl get csr kelly -o yaml`

To decode : `echo "cert content" | base64 --decode`

**Who does all of it ?** 

All the Certificate Operation carry out by **Controller Manager**

There is **CSR-APPROVING** and **CSR-SIGNING** Controller . They are responsible for carrying out these specific tasks 

If anyone has to sign Certificate. They need the **CA Server** root certicate and private key 

**Controller Manager Configuration** has 2 options where I can specify these:

```
- --cluster-signing-cert-file=/etc/kubernetes/pki/ca.crt
- --cluster-signing-key-file=/etc/kubernetes/pki/ca.key
```

## Kubeconfig 

**Query the Kubernetes REST API**

```
curl https://my-kube-playground:6443/api/v1/pods \
  --key admin.key \
  --cert admin.crt \
  --cacert ca.crt
```

**using the kubectl command-line tool**

```
kubectl get pods \
  --server https://my-kube-playground:6443 \
  --client-key admin.key \
  --client-certificate admin.crt \
  --certificate-authority ca.crt
```

But it will take so much time to do that so I configure **Kubeconfig** file so every I make a request I don't need to specify those options 


By default the **kubectl** tools look for the **config** file under `$HOME/.kube/config` 

**Config** file have 3 section : 

**Cluster**:

- Cluster are various Kubernetes Cluster that I need access to

- `--server` option will go in here

- `--certificate-authority` will go in here 

**User**

- Are user account with which I have access to the Cluster

- These User may have different **Privileges** on different Cluster

- `--client-key` option will go here

- `--client-certificate` will go here 

**Context**

- Context marry User and Cluster together

- Context define which User account will be used to access which Cluster

- For example I could create a Context name `Admin@Production` that will use Admin Account to access Production Cluster  

Each of those is an Array format . That way I can specify multiple Cluster, Users, or Context in the same file 

```
apiVersion: v1
kind: Config
clusters:
- name: production
  cluster:
    certificate-authority: ca.crt
    server: https://172.17.0.51:6443
contexts:
- name: admin@production
  context:
    cluster: production
    user: admin
users:
- name: admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
```

**kubectl** know which context to choose by using **current context** field 

To view and modify **kubeconfig** file : `kubectl config view`

If I do not specify which **kubeconfig** file to use it ends up using the default file located in the folder **.kube/config** 

Alternatively I can specify **kubeconfig** file `kube config view --kubeconfig=my-custom`. 

We will move our Custom config to **.kube** so it become a default config file 

To update **Current Context** : `kubectl config use-context admin@production`

Other options of kubectl config : `kubectl config -h`

**Each Cluster may be configured with multiple namespace in it**

To configure Context to switch to paticular namespace  

**Context** section can take addition field called **Namespace**


```
apiVersion: v1
kind: Config
clusters:
- name: production
  cluster:
    certificate-authority: ca.crt
    server: https://172.17.0.51:6443
contexts:
- name: admin@production
  context:
    cluster: production
    user: admin
    namespace: default
users:
- name: admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
```

NOTE: Better to use full path for Certificate 

**Another Way to specify Cert credentials**

- We will get a content of `ca.crt` file . Instead of using **Cert Authority** and the path to the file . I may optional use : `certificate-authority-data: <content of cert as base64>`

**To set my-kube-config as the default kubeconfig file without overwriting your existing ~/.kube/config and make it persistent across all shell sessions**

```
echo 'export KUBECONFIG=~/.kube/config:~/path/to/my-kube-config' >> ~/.bashrc
source ~/.bashrc
```

## API Group

**What is Kubernetes API?**

To interact with **API SERVER**: 

- Via **REST** or via **kubectl**

To see the Version : `curl https://kube-master:6443/version`

To see the Pods : `curl https://kube-master:6443/api/v1/pods`

The **Kubernetes API** is grouped into multiple such groups based on their purpose 

- `/apis`

- `/metrics`: To monitor heath of the Cluster 

- `/heathz`: To monitor health of the Cluster

- `/version` : **Version API** for viewing the version of the Cluster 

- `/logs`: To intergating with third-party logging applications 

**Kubernetes API** categorized into 2 : 

**/api** is the **CORE** group

- This is where all the **CORE** functionality exists : `/api/v1/pods,namespace,events,rc,PVC,secret..etc`

**/apis** is the **Name** group 

- The **Name Group API** are more organize . Going forward all the available features are going to made available through these named Groups

- It has group under : `/apps`, `/extensions`, `networking.k8s.io`, `/storage.k8s.io/`,  `/authentication.k8s.io`, `/certificates.k8s.io` (These are **GROUP**)  

- Within `/apps` I have : `/apps/v1` I have `Deployment`, `ReplicaSet`, `StatefulSets` (These are **Resources in that Group**)

- Within `networking.k8s.io` : `/v1` -> `/networkpolicies`

- Each **Resource** has a set of **Action GET, LIST, CREATE, DELETE, UPDATE, WATCH** asscociate with them

The **Kubernetes API reference** page can tell me what the **API Group** is for each object

I can also view these on my Kubernetes Cluster :

- Access **Kube API Server** at port 6443 with any path : It will list the available API groups

```
curl http://localhost:6443 -k \
--key admin.key \
--cert admin.crt \
--cacert ca.crt
```

- Within the **Named API** group it return all the supported resource groups : 

```
curl http://localhost:6443/apis -k | grep name \
--key admin.key \
--cert admin.crt \
--cacert ca.crt
```

Alternative option is : `kubectl proxy` launches a proxy service locally on port 8001 and uses credentials and certificates from my **kubeconfig** file to access the cluster 

Now I can use `curl http:localhost:8001 -k` and the proxy will use the credentials from **kubeconfig** file to forward request to **KubeAPI Server**

**kubectl proxy** is a HTTP proxy service create by **kubectl** the access the **KubeAPI Server**

## Authorization

 When Human or Machine gained access to the Cluster . What can they do ? 

 When we share our Cluster between different organizations or teams, by logically partitioning it using **namespace** we want to restrict access to the user to their **namespace** alone 

 **Authorization Mechanisms**

**Node Auth**:

- **Kube API** server is accessed by Users and **Kubelet** 

- **Kubelet** access **API SERVER** to read information about Services and EndPoints, Nodes and Pods

- **Kubelet** also report to **Kube API Server** with information about the Node such as **status** .

- These requests are handled by Specicial Authorizer known as **Node Authorizer**

- **Kubelet** should be part of the **System Nodes Group** and have a name prefixed with ``system:node:<node-name>`

- Any request coming from a **User** with a name **System Node** and part of the **System Nodes Group** is authorized by the **Node Authorizer** and are granted these privileges  

**Attribute-base auth (ABAC)**

- Dev User is a **External access to the API** .

- **Attribute-base auth (ABAC)** is where I associate a User or Group of users with a set of permissions  Let's say Dev user can View, Create, Delete Pods

- To do this I create a Policy file with set of policy define in JSON format : `{"kind": "Policy", "spec": {"user": "dev-user", "namespace": "*", "resource": "pods", "apiGroup": "*"}}` . Then I will pass this file to **API Sever**

- Everytime I need to add or make change in Security I must edit this policy file manually and restart the **Kube API Server** 

**Role Base Auth (RBAC)**

- Instead of directly associating a User or Group with a Set of Permission we define a **Role**

- For Developers : We create a **Role** with a set of Permission required for Developers then we associate all the Developers through that **Role**

**Webhook**

- To **outsource** all the Auth Mechanisms . Let;s say I want to manage authorization externally and not through the built-in mechanisms that we just discussed

- For example : Open Policy Agent is a third-party tool that help with admission control and authorization . I can have Kubernetes make an API call to Open Policy Agent with the information about the user and his access requirment and have the Open Policy Agen decide if the user Permitted or not . Based on that response the user is granted access 


**AlwaysAllow** : Allow all request withou performing any Authorization checks 

**AlwayDeny**: Deny all the Requests 

The **Mode** are set with options : `--authorization-mode=Node,RBAC,Webhook`. If I don't set this option it will set to **AlwaysAllow** by default 

When I have **multiple Mode** configure my request is authorized using each one in the order it is specified 

- For example : When Users send request it will handle by **Node Auth**, bcs **Node Auth** only handle Node Requests it will deny the requests then it forward to the nex one is **RBAC**, this one perform its checks and grants the User permission . Authorization is complete and user is given access to the requested object . As soon as **requests approved** no more checks are done and user is granted permission   

## RBAC 

To create **Role** by creating **Role Object** 

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```

Each **rules** has 3 sections :

- **apiGroups** :

  - For **CORE Group** I can leave the API Group section as blank . For any other Group I specify the Group Name  

- **resources**

- **verbs** 

I can add multiple Rules for single Role 

To create **Role**: `kubectl create -f developer-role.yaml`

To link the **User** to that Role using **RoleBinding**

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```

**subjects** : Where we specify the user details 

**roleRef** is where provide the details of the roll we created 

Note: **Role** And **RoleBind** fall under the scop of Namespaces 

- If I want to limit access User in different namespace then specify the **namespace** within the **metadata**

To view the **Roles**: `kubectl get roles`

To view the **RolesBinding**: `kubectl get rolebindings`

To view more detail about the **Role** : `kubectl describe role developer` 

To view more detail about the **RoleBinding** : `kubectl describe rolebinding developer`

If me as a User would like to see if I have access to particular resource in the Cluster : `kubectl auth can-i create deployment` , `kubectl auth can-i delete deployment`

If I am as a Admin and want to test If that use gained access or not I can use : `kubectl auth can-i create deployment --as dev-user`

To allow access to specific Resources I will use **resourceName**:

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
  resourceNameL ["Pod Name"]
```

## Cluster Role 

Nodes are Cluster Wide resources . 

Cluster Scope Resources : Nodes, PV, ClusterRoles, ClusterRoleBinding, Certificatesigningrequests, Namespace ... (Where I don't sepcify a namespace)

To see resources with a **Namespaced**: `kubectl api-resources --namespaced=true` 

To see resources with **Non Namepsaced**: `kubectl api-resources --namespaced=fales`

To Authorize users to **CLuster wide** resources I will use **Cluster Role** and **Cluster RoleBinding** 

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-administrator
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["list", "get", "create", "delete"]

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin-role-binding
subjects:
- kind: User
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-administrator
  apiGroup: rbac.authorization.k8s.io
```

NOTE: I can create **ClusterRole**  for namespaced resources as well . When I do that the user will have access to these resources across all **namespaces**

Use `kubectl api-resources` to check **API GROUP** and **Resources Name**

## Service Account

To create ServiceAccount: `kubectl create serviceaccount <serviceAccount-name>`

To see Service Account: `kubectl get serviceaccount`

When **SA** created, it also create a Token automatically . The **SA Token** must be used by **external application** while authenticating to **Kube API** 

The Token is store as **Secret Object** .. 

When **SA** created it first create **SA Object** and then generate **Token** for a SA . It then create **Secret Object** and stores that token inside the **Secret Object** . **Secret Object** is then linked to **SA** 

To view the **Token** : view the **Secret Object**  `kubectl describe secret <secret-object-name>` . 

This **Token** can be use as **Authentication Bearer Token** while making the risk call the **Kuber API** 

For example using **CURL** I could provide the **Bearer Token** as an Authorization Header while making a risk call to the **Kube API** 

```
curl https://192.168.56.70:6443/api -k \
--header "Authorization: Bearer eyJhbgG…"
```

I can create **SA** , assign Permission by creating **RBAC** and export **SA Token** and use it to configure to Third Part Application to Authenticate to **Kube API** 

#### What if external Application hosted in K8 itself ?

This case, the whole Process of exporting the **SA Token** and configuring the Third Party Application to use it can be made simple by automatically mounting the **SA Token Secret** as a Volume inside a Pod hosting the Third Party Application  

For every **Namespace** in Kubernetes, a **SA default** is automatically created. Each **Namespace** has its own **Default SA** 

- Whenever a Pod is created, the **Default SA** and its Token are automatically mounted to that Pod as a Volume mount 

The Secret containing the Token for the **Default SA** is mounted at the Location inside the Pod : `/var/run/secrets/kubernetes.io/serviceaccount` 

From insde the Pod, To lis the `serviceaccount` directory : `kubectl exec -it my-kubernetes-dashboard -- ls /var/run/secrets/kubernetes.io/serviceaccount`

- The secret mounted as 3 Seprate Files:

- **token** file is the one with acutal Token

NOTE: The **Default SA** is very much restricted . It only has permission to run basic **Kube API** queries 

If I want to use different **SA**, modify the Pod definition file to include **SA** field : `serviceAccountName: <sa-name>`

Note : Can not edit **SA** of a existing Pod . I can do that in case of **Deployment** 

To not to Mount **SA** automatically : `automountServiceAccountToken: false`

**From K8 v1.22**

Introduce a mechanism for provision Kubernetes **SA** tokens that are more secure and scalable via an API **TokenRequestAPI** 

So tokens generated by the **TokenRequestAPI** are **audience bound**, **time bound**, **object bound** more secure 

When a new pod is created, it no longer relies on **SA Secret Token**. 

- Instead , a Token with a defined lifetime is generated through the **Token Request API** by **SA Admission Controller** .

- And this Token then mount as a prjected volume onto the Pod 

**With v1.24** 

Another the enhancement was made . In the past when **SA** was created, It automatically created a **Secret with a Token** that has no expire and is not bound to any audience 

- This was then automatically mount as a Volume to any Pod that use that **SA**

IN **v1.22** that was changed the automatic mounting of the **Secret Object** to the pod was changed. and Instead it then moved to **TokenRequestAPI** 

With **v1.24** a change was made where when I create a **SA** . It no longer automatically create **Secret or Token Access Secret** 

- So I must run : `kubectl create token <service-account-name>` to generate the Token for that **SA**

- This time the Token will have an **expiry date** defined 

## Image Security

This Image `image: nginx` it acutally `image: library/nginx` .

- First part stand for User or Account Name . If I don't provide User or AccountName It assume to be `library`

- **Library** is a name of Default Account where Docker's offical images are stored 

**Where are these Images store and pull from?** 

Since we have not specified the location, it assume to be Docker Registry **docker.io** (DockerHub)

To use Image from our **Private Registry** the Image name should be a full path in the Private Registry: `private-registry.io/apps/internal-app`

For Kubernetes get the **Credentials** and authenticate to **Private Registry** : 

- First we create **Secret Object** with Credentials in it . **Secret Type: docker-registry**

```
kubectl create secret docker-registry <docker-name> \
--docker-server= \ 
--docker-username= \ 
--docker-password= \ 
--docker-email= 
```

- **docker-registry** is a built-in Secret Type that was built for storing Docker Credentials

- Then we specify the Secret inside our Pod definition file :

```
imagePullSecrets:
- name: <docker-name>
```

When the Pod created **Kubelet** use the credentials from Secret to pull Images 

## Docker Security 

Containers and Host share the same **Kernal** 

Containers are isolated using namespaces in **Linux** 

- Host has its own Namespace and Containers have its own Namespace

- All the Processes run by Containers are infact run by the Hosts itself but in their own namespace

When I list the **Processes within the Docker container** `ps aux` I will see the **Container Process** with the Process ID : 1 

So when I list the **Processes on the Host**, I see a list of Processes including **Container Process** but with different Process ID

This is bcs Processes can have different **Process IDs** in different **Namespaces** 

**Users in context of Security** 

**Docker Host** has a set of users, **Root** as well as **Non-Root** 

By default, Docker run processes within Containers as the **Root User** 

If I don't want the **Processes within the Container** to run as **Root User** I may set the user using the **User Option within the Docker run command** and specify the **New UserID** : `docker run --user=1000 ubuntu sleep 3600`

Another the way to enfore **User Security** is to have this defined in then **Docker Image** itself at the time of Creation

- For example I can define the and set the `UserID: 1000` . Then build the custom image 

```
FROM Ubuntu

USER 1000 
```

- We can not run the Image without specify the `userID` and the **Process** will be run with the **UserID: 1000**

**What happen if I run Container with a Root User?**

Docker implements a set of Security features that limits the abilities of the **Root User** within the Container .

The **Root User within the Container** isn't really like the **Root User On the Host** 

To see a full list of **Linux Capability** : `/usr/include/linux/capability.h`

I can now control and limit what capabilities are made available to a user . By default, Docker run a container with limited set of Capabilites, and so the **Processes run within the Container** do not have the privileges to say reboot the host or perform operations that can disrupt the host or other containers running on the same host 

To provide additions privileges use the : `docker run --cap-add` . Also to drop the privileges: `docker run --cap-drop`

If I want to run container with all **Privileges** : `docker run --privileged`

## Security Contexts

In K8s, Container are encapsulated in Pods . I may choose to configure the **Security Setting** at a container Level or at Pod Level 

If I configure at **Pod Level** the setting will carry over to all the Containers within the Pod 

If I configure at both **Pods and Containers** the settings on the container will override the settings on the Pods 

```
apiVersion: v1
kind: Pod
metadata:
  name: web-pod
spec:
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
      securityContext:
        runAsUser: 1000
        capabilities:
          add: ["MAC_ADMIN"]
```

**securityContext** to configure **Security Context on the Containers**

- **runAsUser** To set **UserID** for the Pod 

- To add **capabilites** and specify a list of capabilities

```
capabilities:
  add: ["MAC_ADMIN"]
```

## Network Policies

**Traffic** 

There is 2 types of Traffics **Ingress and Egress** 

**Ingress** is the incoming traffic from the Users 

**Egress** is the outgoing request to the **App Server** 

In case of **Backend API** server, it receives **Ingress traffic** from **Frontend** and **Engress traffic** to **DB server** 

In the **DB Server** perspective it recieve **Ingress Traffic** from **Api Server** 

**Network Security in Kubernetes** 

So we have Cluster with a **Set of Nodes** hosting a **Set of Pods and Services**

Each Node, Pod and Service have an **IP Address** 

One of prerequisite for Networking in Kubernetes, is whatever solution I implement the Pod should be able to communicate with each other without having to configure any additional settings like **Routes** 

- For example: All pods are on a Virtual Private Network that span accross the Node in Kubernetes Cluster . By default **its can reach each other using the IP address or Pod name or Services**

- Kubernetes is configured by default with **All Allow Rules** that allow traffic from any Pod to any other Pod or Service within the Cluster 

**Network Policy** to implement to allow traffic to DB server only from API server 

**Network Policy** is another Object in Kubernetes namespaces like Pods, Replicas Sets, I link a **Network Policy** to one or more Pods . I can define rules within the **Network Policy** .

- In this case I would say only allow **Ingress Traffic** from the API pod on port 3306. Once this policy created it block all other traffic to the pod 

To apply or link a **Network Policy** to Pod we use **Labels and Selector** 

- We **Labels** the Pod and use the same **Labels** on the **Port selector** field in the **Network Policy** and the we build our Rule

- Under **Policy Types** specify whether the rule is to allow **Ingress or Egress** traffic or both

- Next we define **Ingress Rule** that allow traffic from API Pod using **Labels and Selector**

- And finally the Port allow traffic on 3306

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod
    ports:
    - protocol: TCP
      port: 3306
```

Note: Ingress or Engress isolation only comes into effect if I have **ingress** or **egress** in the **policy types** 

**Network Policy** are enforced by the Network Solution implemented in Kubernetes Cluster 

The supported : **Kube-router**, **Calio**, **Romana**, **Weave-net**

**More Details Network Policy** 

For example I have Front end, API backend, and DB Pod . My goal is to protect the **DB pod** so that it does not allow access from any other Pod except the **API pod** and only on Port 3306 

First I want to block everything going in and out of the DB pod . So I will create a **Network Policy**

- First step is to associate with the Pod I want to protect by using **Labels and Selector**

- Then I need to figure out what **Types of Policies** should be defined on this **Network Policy** object to meet our requirement . **Ingress and Egress**

- From **DB Server** Perspective I want to allow incoming traffic from API Pod **(Ingress)** then the **DB Server** return results **Egress** .

- But do I need separate rule for the Results to go back to the **API Pod** ? No, bcs once I allow incoming traffic the response to that traffic is allow back automatically . We don't need a separate rule for that . So in this case I only allow **Ingress rule from API pod to DB Pod** . And that allow **API Pod** connect to DB, run queries and also retrieve the result of the queries 


What if there are multiple **API Pods** in the Cluster with the **Same Labels** but in different **Namespaces** .

The current **Network Policy** would allow Any Pod in any **Namespace** with **matching Labels** to reach **DB Pod** . But I only want to allow **API pod** from **Production Namepsace** to reach **DB Pod**.

- To do this I add the **namespaceSelector** 

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector: 
    matchLabels:
      role: db ## Labels on the DB Pod 
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod
      namespaceSelector:
        matchLabels:
          name: prod 
    ports:
    - protocol: TCP
      port: 3306
```

What if I only have **namespaceSelector** but not **podSelector**, in this case all Pod within the **Specified Namespace** will be allow to reach **DB Pod** 

Another Use Case :

- Let's say I have a **Backup Server** outside of K8 Cluster, and we want to allow this Server to connect to **DB Pod** . **Backup server** is not a Pod, the **Namespace and Selector** filed won't work . However I knows the **IP Address** I could configure **Network Policy** to allow traffic originating from certain **Ip addresses**

- For this I add new Type **ipBlock** .

- **IP Blocks**  allow me to specify a **range of IP addresses** from which I could allow traffic to hit **DB pod**  

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector: 
    matchLabels:
      role: db ## Labels on the DB Pod 
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod
      namespaceSelector:
        matchLabels:
          name: prod
    - ipBlock:
        cidr: 192.168.5.10/32
    ports:
    - protocol: TCP
      port: 3306
```

These **Types** can be passed in separately as individual rules or together as part of Single rules 


Another user Case:

- Let's say we have an **Agent** on the **DB Pod** that pushes backup to the **Backup Server** . In this case traffic is originating from **DB Pod** to **Backup Server**

- First we add **Egress** to **policyTypes**

- Instead of **from** we now have **to**

- Under **to** I could use any **Types** of **Selector**  

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector: 
    matchLabels:
      role: db ## Labels on the DB Pod 
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod
      namespaceSelector:
        matchLabels:
          name: prod 
    ports:
    - protocol: TCP
      port: 3306
  egress:
  - to:
    - ipBlock:
        cidr: 192.168.5.10/32
    ports:
    - protocol: TCP
      port: 80
```

## Kubectx and Kubens

(https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/27430646#content)

## Custom Resource Definitions CDR

**Controller** is a process that runs in the background and its job is to **continuously monitor the status of resources** that it is supposed to manage 

I can't create any **Resource** that I want without configuring it in the **Kubernetes API** 

For that we need **CDR** . I will use **CRD** to tell Kubernetes that I want create **Object of Kind** 

**scope** defines if the Object is **namespaced** or not 

- **namespaced scope** : Pod, ReplicaSet, Deploymnet, Services, ....

- **non-scope namepsace**: PVC, Nodes, namespace itself ....

**group**: is the **API Group** that we provide in the API Version 

**names**: Name of resources

- `kind:`

- `singular`: to display the resource type in the output of **kubectl** command 

- `plural:` is used by the Kubernetes API resource. If I run `kubectl api resources` it will show here

- `shortNames`: to provide short version of the Name

**versions**: Each resource can be configured with multiple version as it passes through the various phases of its lifecycle 

- **served**: With multiple Versions configured for the same resource. We must choose which ones are served through the **API Server** .

- **storage**: And we also define which is the Storage Version if I have multiple Version only one version can be marked as the storage version

- **Schema**: The Schema define all the parameters that can specified under the **spec** section when I create the Object .

  - Schema define what fields are supported and what type of value that fields support ...
 
  - Schema use **OpenAPI v3** which specify the different properties using an **Object type**
  
```
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: flighttickets.flights.com
spec:
  scope: Namespaced
  group: flights.com
  names:
    kind: FlightTicket
    singular: flightticket
    plural: flighttickets
    shortNames:
      - ft
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                from:
                  type: string
                to:
                  type: string
                number:
                  type: integer
                  minimum: 1
```

**CDR** only allow me to create these Resources and store them in **ETCD** . It's not actually going to do anything about these resources bcs we don't have a **Controller** 

## Custom Controller 

What I need to do is **monitor the status of the Objects in ETCD and perform actions**

**Controller** is any process or code that runs in loop and is continuously monitoring the Kubernetes Cluster and listening to events of specific objects being changed 

Developing in Go with Kubernetes Go Client provide support for other libraries like shared informers that provided caching and queuing mechanisms that can help build controller easily 

There is Github repo: (https://github.com/kubernetes/sample-controller) . 

First clone this repo then modify the `controller.go` with my custom code and build and run it 

## Operator 

**CRD** and **Custom Controller** can be packaged together to be deployed as a single entity using the **Operator Framework** 

By deploy the flight operator it internally creates the **CRD** and the **Resources**, also deploys the **Custom Controller as a Deployment** .

**Operator Framework** does much more than just deploying these 2 components 

For example: **ETCD Operator** it is used to deploy and mange an **ETCD Cluster within Kubernetes** 

- It has a **ETCD CRD** and **ETCD Controller**

- It can take a **backup** of the ETCD Cluster, as well as resotre a **backup** to the ETCD Cluster by simply creating a **CRD** 

So **Kubernetes Operator** do what a **Human Operator** typically would do to manage a specific application such as Install, maintaining, taking backups and restoring backup in case of disasters 

All Operator are available at (operatorhub.io)

## Docker Storage 

**How Docker store data on the local file system**

When I install Docker it create the folder structure at **/var/lib/docker/** 

- I have multiple folders under it called : **aufs, containers, image volumes, etc ...** This is where Docker stores all its data by default

- When I say data, I mean **files** related to Images and Containers running on the Docker host

When I run the container based off of this image using **docker run** . **Docker create a Container base of these layers** and creates a new **writable layer** ontop of the **image layer** . 

**The writable layer** is used to store data created by the container such as **log files written by application, any temporary files generated by the container or any file modified by the user on that container**

When I use `docker volumes create data_volume`

- Docker create a folder called `/var/lib/docker/volumes/data_volume`

- Then when I run container I could **mount the volume using -v** inside the container `docker run -v data_volume:/var/lib/mysql mysql`

- New way to mount a volume is to use **--mount** `docker run --mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql`

**Storage Driver** is responsible for doing all of these operations, maintaining the layer architecture, creating a writable layer, moving files across layers to enable copy and write, etc ...

Some common **Storage Driver**  are **AUFS, ZFS, BTRFS, Device Mapper, Overlay, Overlay2** 

The selection of the **Storage Driver** depends on underlying OS being used 

**Volumes** are not handle by **Storage Driver**. It handle by **Volumes Driver Plugin** 

- Default **Volume Driver Plugin** is **local**

- There are many other **Volume Driver Plugin**  allow me to create a Volume on third party solution : **Azure File Storage, Convoy, Digital Ocean Block Storage, Flocker, Google Compute Persist Disks** 

## Container Storage Interface

In the past Kubernetes used Docker alone as a Container Runtime Engine and all the code to work with Docker was embedded within the Kubernetes source code .

With other Container Runtime coming in . Kubernetes extend support to work with different Container Run time and not be dependent on the Kubernetes source code and that's how **Container Runtime Interface** came 

**Container Runtime Interface** is a standard that defines how an orchestration solution like Kubernetes would communicate with container runtime like Docker 

Similarly to extand support for different networking solutions the **Container Networking Interface** was introduced 

Similarly the **Container Storage Interface** was developed to support multiple Storage Solution . 

With **CSI** I can now write my own driver for my own Storage to work with Kubernetes 

**CSI** is not Kubernetes specific standard . It is a **universal standard**  

**CSI** define a set of **RPCs or Remote proedure calls** that will be called by the Container Orchestrator and these must be implemented by the **Storage Driver** 

**CSI** say that when a pod is created and requires a volume, the container orchestrator (Kubernetes) should call the create volume **RPCs** and pass a set of details such as the Volume name, the **Storage Driver** should implement this RPC and handle that request and provision a new volume on the Storage Array and return the results of the Operations 

## Volume

The Pod created in Kubernetes are transient in nature, the data created and deleted as well 

For this we **attach a volume** to the Pod .

- Data generated by the Pod is now stored in the Volume 

**Volume** need a **Storage** . When I create a **Volume**, I can choose to configure its storage in different ways 

- To configure it to use a directory on the **Host** I use **hostPath** 

- Once Volume created, to access it from a Container we mount the Volume to a Directory inside the Container, at the **image level** I use **volumeMounts**

```
apiVersion: v1
kind: Pod
metadata:
  name: random-number-generator
spec:
  containers:
  - image: alpine
    name: alpine
    command: ["/bin/sh", "-c"]
    args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
    volumeMounts:
    - mountPath: /opt
      name: data-volume
  volumes:
  - name: data-volume
    hostPath:
      path: /data
      type: Directory
```

To configure **AWS EBS** as the Storage option, we replaced **hostPath** filed with the **awsElasticBlockStore** . The Volume Storage will now be on **AWS EBS**

```
apiVersion: v1
kind: Pod
metadata:
  name: random-number-generator
spec:
  containers:
  - image: alpine
    name: alpine
    command: ["/bin/sh", "-c"]
    args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
    volumeMounts:
    - mountPath: /opt
      name: data-volume
  volumes:
  - name: data-volume
    awsElasticBlockStore:
      volumeID: <volume-id>
      fsType: ext4
```

## Persistence Volume 

**Persistence Volume** is a Cluster-Wide pool of Storage Volumes configure by Admin to be use by User deploy Application 

The User can now select to **Storage Pool** using the **Persistence Volume Claim** 

- **accessModes** define how the Volume should be mounted on the **Host** : **ReadOnlyMany, ReadWriteOne, ReadWriteMany**

- **capacity**: Specify the amount of Storage to be reserved for this **PersistenVolume**

- **hostPath**: This is one of the **Volume Types**

- **awsElasticBlockStore** : This is one of the **Volume Types**

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  accessModes:
    - ReadWriteOnce
  capacity:
    storage: 1Gi
  awsElasticBlockStore: ## Never use in Production 
    volumeID:
    fsType: ext4
```

## Persitence Volume Claim 

Admin create the **PV** then User create **PVC** to use the storage 

Once **PVC** created Kubernetes bind the **PV** volume to claim based on the request and propeties set on the Volume 

Every **PVC** is bound to a **PV** during the binding process, Kubernetes tries to find a , PV that has **sufficent capacity** that request by the claim. And any other request properties such as **Access Mode, Volume Mode, Storage Class etc...** 

If there are multiple matches for a single Claim and I would like to use particular **PV** I can still use **Labels and Selectors** to bind to the right Volumes 

Note: Smaller claim may get bound to larger Volume if all the criteria matches and there are no better options . 

There is one to one relationship between claim and volume .

If there is no Volume available the **PVC** remain in the **pending** state . Until newer volume are made available to the Cluster . Once newer volume are available the Claim automatically bound to a newly available volume 

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
 name: myclaim
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 500mi
```

What happended to the **PV** when the **PVC** is deleted ? 

- I can choose what happen to the **PV** : **PersistentVolumeReclaimPolicy: Retain**  (Default). Meaning the **PV** will remain until Admin manually deleted

- To delete it automatically : **PersistentVolumeReclaimPolicy: Delete**

- To recyle . In this case the data in the data volume will be srubbed before making it available to other Claim : **PersistentVolumeReclaimPolicy: Recycle**

To use **PVC** in Pod (https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/17328568#content)

## Storage Class

**Storage Class** I can define a provisioner such as **Google Storage, AWS EBS, Azure etc..** that can automatically provision storage on AWS EBS for me and attach that to Pods when a claim is made 

```
apiVersion: storage.k8.io/v1
kind: StorageClass
metadata:
  name: ebs-aws
provisioner: kubernetes.io/ebs-aws
```

When I have **Storage Class** I don't need a PV, bcs now I have StorageClass will automatically create a Storage when a Pod need

For **PVC** to use **StorageClass** we specify **storageClassName: ebs-aws** 

With each of these **Provisioners** I can pass in additionall **parameters** such as the type of disk to provision or the replication type 

```
apiVersion: storage.k8.io/v1
kind: StorageClass
metadata:
  name: ebs-aws
provisioner: kubernetes.io/ebs-aws
parameters:
  type:
  replication-type: none
```

## Linux Networking Basics

For Computer A to reach Computer B we connect them to a **Switch** . **Switch** create a network containing the two systems 

To connect them to the **Host** we need a **ehth0: Interface (first Ethernet (wired) network device)** on each **Host** 

To see the **Interface** for the Host : `ip link`

Let's assume it is a network with the address `192.168.1.0`. We then assign the system with IP address on the same network : `ip addr add 192.168.1.10/24 dev eth0` and `ip addr add 192.168.1.11/24 dev eth0`

Once the links are up and **IP addresses** are assigned . The computer can now communicate with each other through the **Switch** : `ping 192.168.1.11`

The **Switch** can only enable communication within a network, whichm eans it can receive packets from a host on the network and deliver it to other systems within the same Network 

For the System to reach the System in **another Network** we will use the **Router** .

**Router** help connect 2 network together . Since it connect to 2 Network it get **2 IPs assigned** 1 on each Network 

**When System B in one Network try to reach System C in another Network . How does it know where the Router is on the Network ?**

- We need to configure the system with a **Gateway**

- If the Network is a room, the **Gateway** to the outside world

- The System need to know where that door is to go through that

- To see the existing routing configuration on a system : `route` it will display the kernal **routing table**

- To configure a Gateway on **System B** to reach the **systems C on network 2.0** : `ip route add 192.168.2.0/24 via 192.168.1.1`

  - `192.168.2.0/24` : Is a IP block from **System C**
 
  - `192.168.1.1`: Is a **Router** on **System B**
 
- This has to be configured on all the System

Suppose these System need access to the Internet . I can say for any Network that I don't know a Route to use this Router as the **Default Gateway**  

If I have multiple **Routers** in my Network . 1 for **Internet** the other is for **Internal Private Network** then I have need to have 2 Separates Entries for each Network 

If I am having issues reaching internet from my Systems, this **Routing Table** and the **Default Gateway** configuration is a good place to start 

**How we can set up Linux Host as a Router** 

I have 3 Hosts A, B and C . 

A and B connected to network `192.168.1.0`. and B and C to another on `192.168.2.0`

So host B is connected to both the Networks using 2 Interfaces **eth0** 

A ip address `192.168.1.5` , C ip address `192.168.2.5` . 

How do I get A to talk to C  . We need to tell host A that the **Gateway** to network is through Host B : `ip route add 192.168.2.0/24 via 192.168.1.6`

By default in Linux, package are not forwarded from one interface to the next . 

- For example: Package received on **Eth0** on host B are not forwarded to elsewhere through **Eth1** (For security reason)

Whether a Host can forward packets between interface is governed by a settings in this system at file `cat /proc/sys/net/ipv4/ip_forward`

- By default the value in this file is set to **0** meaning no Forward

- `echo 1 > ip addr add 192.168.1.10/24 dev eth0` set to one so I can see **ping** go through

- I have to modify value in thr `/etc/sysctl.conf -> net.ipv4.ip_forward = 1` 

## DNS 

We have 2 computer **A and B** in the same network with IP address `192.168.1.10` and `192.168.1.11` . 

I want to use a **Name** instead of Ipaddress to **ping** or connect to each other . 

- I need to set a **Name** for both system . And tell the Both system that the other System has assigned that **Name**

- To tell System A when I say **db as a Named on System B** I mean **System B Ip address** : `cat >> /etc/hosts`

```
192.168.1.11     db
```

NOTE: Whatever we put in the `cat >> /etc/hosts` file it a source of truth for Host A but that may not the Truth . Host A does not check to make sure if System B acutaly name is db 

Translating hostname to **IP address** this way is known as **Name resolution** . Within a small system I can easily set in the **/etc/hosts file** 

Untill the environment grew and these files got filled with too many Entries and managing these became too hard . If one of the Servers IP changed I would need to modify the entries in all of these Hosts, and that's where we decided to move all these Entries  into a single Server who will manage it centrally **DNS Server** 

And then we point all hosts to look up that Server if they need to resolve the **Host name** to an **IP address** instead of its own `/etc/hosts` files .

**How do we points our host to a DNS Server?** . 

Every Hosts has a **DNS Resolution configuration file** at **/etc/resolv.conf** . I add an entry into it specifying the address of the DNS Server .

```
/etc/resolv.conf

nameserver 192.168.1.100
```

Once this is configured on all of my hosts, every time a host comes up across a host name that it does not know about it looks it up from the **DNS Server**  

If I want to provision a **test server** for my own needs . 

If I try to **ping** a server that is not in my **DNS Server** or **/etc/hosts** 

For example : I try to ping **facebook.com**  but I dont have **facebook.com** in my **/etc/hosts** and Also I don't have its in my **DNS Server** this case It will failed 

- I will add another entries into my **/etc/resolv.conf** to point to a **Name Server** that knows **facebook.com**.

```
nameserver 8.8.8.8
```

**8.8.8.8** is a common well-known public Name Server available on the Internet hosted by Google that knows about all websites on the  Internet 

I can have multiple **Name Server** like this configure on my host . But then I will have to configure on my hosts in the Network 

I already have a **Name Server** within my network configured on all the hosts . So in that case, I can configure the **DNS server**  itself to forward any unknown host names to the Public name server on the Internet 


**Domain Name** 

How IPs translate to names that we can remember on the **Public Internet**

The reason they are in this format separated by dots like **.com , .edu, .org** is to group like things together 

**.com, .net, .edu, .org, .io etc...** are **Top level Domain** . They represent the intent of the website 

In Google case the **.** is the **root** . Where everything start 

- **.com** is a Top Level Domain .

- Google is a domain name assigned to Google

- **www** is a **Subdomain**

  - **Subdomain** help in further grouping things together under **Google**. For example **maps.google.com** is google map service so **map** is a subdomain
  

When I try to reach **apps.google.com** . My request first hit my **Organization Internal DNS server**, it doesn't know who apps or google is. So it forward a request to the Internet 

- On the Internet, the IP address of the Server serving **apps.google.com**  maybe resolved with the help of multiple **DNS server**

- The **Root DNS Server** looks at my request  and point me to a **DNS server serving .com**

- **.com DNS Server** looks at my request and forward me to Google

- Then **Google DNS Server** provide me the IP address of the Server serving the **apps.google.com**

- In order to speed up all fututre results, my **organizations's DNS Server** may choose to cache this IP for a Period of time 

- This is out for the **Public**

**What about my Orgianization ?**

- My Organization can have a similar structure too

- For exaple **mycompany.com** is my organization and have multiple **subdomains** for each purpose

- All of these will be configured in my organization's **internal DNS Server**  

To configure **web** to resolve my **web.mycompany.com** . I want to when I say **web**, I mean **web.mycompany.com** for that I make an **entry** into my host's **/etc/resolv.conf** file called **search**

```
/etc/resolv.conf

nameserver 192.168.1.100 
search mycompany.com 
```

Next time, when I try to **ping web** , I will see it actually tries **web.mycompany.com** 

**Record Type** 

How are the record stored in the DNS Server ? 

It's store **IP address** to host name known as **A records** 

Store **IPv6** to host names is known as **AAAA records**

Mapping one name to another name is called **CNAME records**

## coreDNS 

(https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/14410330#content)

## Network namespaces

**Network namespaces** are used by containers like Docker to implement network isolation 

Container are separate from the underlying host using **namespace** 

When I create a container, I want to make sure that it is isolated, that it doesn't see any other processes on the Host or any other container . So we create a special room for it on our host using **namespace** 

The Underlying Host, has visibility into all of the **processess** including those running inside the containers . This can be seen when I list the **processes** from **within the container**

When I list the same **processes** as a Root User from the Underlying Host, I see all the other processes along with the process running inside the container but with **different process ID** 

When it come to networking out hosts has **its own intefaces (eth0)** that connect to the local area network . Our host has its own routing and **ARP tables** with information about rest of the network . I want to see all of those detail from the container  

When the container is created, we create a **network namespace** for it, that way it has no visibility to any network-relate information on the host.

- **Within its namespace** the container can have its own virtual interfaces, **routing** and **ARP tables**. The container has its own **interface**

To create a new **network namepsace** on Linux host : `ip netns add <network-name>` 

To list **network namespace**: `ip netns`

To list the **Interface** on my host: `ip link`

To view the **Interface** within the **network namespace** that we created : `ip netns exec <namespace-name>`

- Then exectue `ip link` command inside the namespace

- The other options is : `ip -n <namespace-name> link`

The same with **ARP table** 

- If I run `arp` command on the  Host I see a list of **entries**

- If I run inside the container  I will see **No entries** and the same for **Routing Table**

As of now the **Network namespace** have no network connectivity . They have no **Interfaces** of their own and they can not see the **Underlying Host Network** 

- First look at establishing connectivity between **namespaces** themselves .

  - Just like how we connect 2 Physical Machine together using a cable the an **eth** on each machine . I can connect 2 **namespace** together using a **Virtual Ethernet Pair or Virtual Cable**. It is often referred to as a **Pipe**
 
  - To create the cable: `ip link add veth-red type veth peer name veth-blue`
 
  - Next is to attach each **Interface** to the **appropriate namespace**: `ip link set veth-red netns red`. Same with the **Blue Namespace**
 
  - We can then assign **IP addresses** to each of these **namespaces** : `ip -n red-namepsace addr add 192.168.15.1 dev veth-red`. The same with **Blue nameSpace** : `ip -n blue addr add 192.168.15.2 dev veth-blue`
 
  - Then bring up the Interface: `ip -n red-namespace link set veth-red up`. Also for **Blue Namespace** : `ip -n blue-namespace link set veth-blue up`
 
  - Try to ping from **Red Namespace to Blue Namespace** : `ip netns exec red ping <blue-ipaddress>`
 
  - If I look at the **ARP Table** on the **Red Namespace** . I can see its identified its blue neighbor with **BLue IP address with MAC address** .
 
  - **Host ARP table** has no idea about these new **namespaces** we have created
 
  - **What can I do when I have more of them ? How to enable all of them to communicate with each other**.
 
  - Just like in the Physical world, I create a **Virtual Network** inside my host . To create a Network I need a **Switch** . To create **Virtual Network** I need a **Virtual Switch**
 
  - So I create a **Virtual Switch** within our host and connect the **namespaces** to it
 
  - (Linux Bridge) To create **Internal Bridge network** we add a new **Interface** to the Host: `ip link add v-net-0 type bridge`. As far as our **Host** is concerned, it is just another **Interface** . I will appear in the output of the `ip link` command along with the other **Interfaces** .
 
  - To bring the **v-net-0 Interface up** : `ip link set dev v-net-0 up` .
 
  - For **namespaces** this **Interfaces** is like a **switch** that it can connect to . Think of it as an **Interface** of the Host and a **Swtich for the namespace**
 
  - The next step is to connect the **namespaces** to this **Virtual Network Switch**
 
  - Earlier, we create the cable or the **Eth pair** with the **veth-red and veth-blue Interfaces** to connect to each other
 
  - Now we will connecting all **namespaces** to the **Bridge network** . So we need new cables for that purpose . Before that I will delete the **veth-red and veth-blue eth** : `ip -n red link del veth-red` . When I delete the link with one end the other end gets deleted automatically
 
  - Now I will create new Cables to connect the **namespaces** to the **Bridge** : `ip link add veth-red type veth peer name veth-red-br` the same for **blue namespace**
 
  - To attach one end of this, of the **Interface** to the **Red Namespace** : `ip link set veth-red netns red`
 
  - To attach the other end to the **Brigde Network** : `ip link set veth-red-br master v-net-0` . The same for **Blue namepsace**
 
  - Now set **ip addresses** for these links and turn them up : `ip -n red addr add 192.168.15.1 dev veth-red` . Same for **Blue namespace**
 
  - And finally turn the devices up : `ip -n red link set veth-red up` .
 
  - The container can now reach each other over the network .
 
  - Follow the same proceed the connect remaining two namespace to the same network. '
 
  - The now have all **namespaces** connect to our **Bridge network** and they can all communicate with each other
 
  - From my **Host** What if I try to reach one of these interfaces in these **namespace*** ? . It will Not work
 
  - But what if I really want to establish connectivity between my host and these **namespace** ?
 
  - The **Virtual Swtich** is actually a **network Interface** for the host . So we do have an **Interface** on the `192.168.15.0` network on our host . Since this just another **interface** all we need to do is assign an IP address to it so we can reach the **namespaces** through it : `ip addr add 192.168.15.5/24 dev v-net-0` .
 
  - Note: This entire network is still **Private** and restricted within the **Host** from within the **namespaces** you can't reach the outside world nor can anyone from the outside world reach the services or applicaions hosted inside .
 
  - The only door to the outside world is the **Ethernet Port** on the Host
 
  - How do we configure this bridge to reach the **LAN network** through the **Ethernet Port**
 
  - What happens if I try to Ping this host from my **blue namespace**. The **Blue Namespace** see that I am trying to reach a network  which is different from my Current network . So it look at the **Routing Table** : `ip netns exec blue route` to see how to find that Network . The Routing table has no information about other Network . So it comes back saying that the Network is unreachable
 
  - So we need to add an entry into a **Routing Table** to provide a **Gateway or door** to the outside world
 
  - To find that **Gateway** . **Gateway** is the system on the **Local Network** that connects to the **other network**. What is a system that has **one Interface on the Network local** to the **Blue Namespace**  and it also connected to the **Outside LAN Network** ?
 
  - It's the **Local Host** that have all these **namespaces** on so I can ping the **namespace** . Out **Localhost** has an **Interface** to attach the **Private Network** so I can ping the **Namespace**,  so **Our LocalHost** is the **Gateway** that connects the 2 Networks together .
 
  - We can now add a **Route Entry** in the **blue namespace** to say **Route** all traffic to `192.168.1.0` network through the **Gateway** at `192.168.15.5`.
 
  - **The Host** has 2 IP addresses one on the **Brigde Network** at `1925.168.15.5` and another on the External Network at `192.168.1.2`
 
  - Can I use any in the Route ? No, Bcs the **Blue namespace** can only reach the **Gateway** in its **Local Network** at `192.168.15.5`
 
  - The **Default Gateway** should be reachable from my **namespace** when I try to add my Route
 
  - When I try to **ping** now I no longer get the **Network unreachable message** . But I still don't get any respose back from the ping  . The Problem is from our **Home Network** we tried to reach the **external Internet** through our **Router** . Our **Home network** has our internal Private IP addresses that the destination network don't know about so they can not reach back .
 
  - For this we need **NAT** enable on our host acting as a **Gateway** here so that it can send the messages to the LAN in its own name with its own address
 
  - To add **NAT** functionality to our Host. We should do that using `iptables -t  nat -A POSTROUTING -s 192.168.15.0/24 -j MASQUERADE` add the new rule in the **NAT IP table** in the post routing chain to **masquerade or replace** the **From Address** on all packets coming from the source network `192.168.15.0` with its own ip address . That way anyone receiving these Packets outside the network will think that they are coming from the Host and not from within the **namespace**
 
  - Finally say the **LAN** is connected to the internet . We want the **namespaces** to reach the internet 

 ## Docker Networking 

 I have a Server with Docker installed on it . 

 - A server have **eth0** to connect to a **Local Network** with **IP 192.168.1.10**

 - Run the container with **None Network** : `docker run --network none nginx` : With **None Network** Docker container is not  attached to any network . The container can not reach the outside world and vice versa

 - With **Host Network** the container attached to the **Host Network** . There is no **Network Isolation** between the Host and the Container

   - If I deploy Web App on port 80 : `docker run --network host nginx` then the Web App is available on port 80 on the host without having to do **Port Mapping**
  
   - If I try to run the same container at the same Port , It won't work as they share the **Host Networking**
  
- With **Bridge** an **Internal Private Network** is created which the **Docker host** and containers attach to

  - The network has an **IP 172.17.0.0** by default and each device connecting this network get their own **Internal Private Network address** on this network    

**How Docker created and managed Bridge Network ?**

When Docker is installed on the host, it creates an **internal private network** called **Bridge** by default . To see that : `docker network ls`

Docker call the network **Bridge**. But on the Host the network is created by the name **Docker0**. To see it : `ip link`

Docker uses : `ip link add docker0 type bridge` 

Note: The Interface, or network is currently down . 

The **Bridge Network** is like an **Interface** to the Host but it switch to the **namespaces or containers** within the Host . So the **Interface Docker0** on the host is assigned an **IP 172.17.0.1** : `ip addr`. 

Whenever a container is created, Docker create **Network Namspace** for it 

To list the **namespace**: `ip netns`

Inpsect the containers : `docker inspect <container-id>` To see the **namespace** associated with each **container** 

**How does Docker attach the container or its network namespace to the bridge network?**

Docker create a **Virtual Cable** with **Two Interfaces** 

Run the `ip link` on the Docker Host . I can see **1 end of the Interface** just attached to the **Local Bridge Docker0** 

Run the `ip -n <namespace-id> link` I can see **the other end of the Interface** within the **container Namespace** .

The **Interface** also gets an IP assigned within the Network . `ip -n <container-id> addr`

The **Interface** pair can be identified using their numbers **Odd and Even** . 

**Port Mapping** 

The container I created : `docker run nginx` . It's a Web Application serving webpage on Port 80 . 

Since my container is within a **Private Network** inside the host, only other contaiers in the same Network or the host itself can access this webpage 

To allow external User to access the applications hosted on containers, Docker provides a **port mapping** . `docker run -p 8080:80 nginx`

**How does Docker do Port Mapping**  

Docker create **NAT rules** for that . Using **iptables** , we create an entry into the **NATs table** to append the rules to the prerouting chain to change the destination port from 8080 to 80 

```
iptables -t nat -A PREROUTING -j DNAT --dport 8080 --to-destination 80 
```

To see the rules Docker created : `iptables -nvL -t nat`

## Container Networking Interface CNI 

We created a Program that performs all the required tasks to get the container attached to a **Brigde Network** 

I can run this program and specify that I want to add this container to a particular **namespace** : `bridge add <container-id> /var/run/netns/<container-id>`. The **Brigde Program** take care of the rest so that the container runtime environments are relieved of those tasks 

For example, When Kubernetes created a new container, they call the **Bridge Program** and pass the **Container ID** and namespace to get networking configured for that container 

**CNI** is a set of Standard that define how programs should be developed to solve networking challenges in a **Container Runtime Environment** . The **Program** are referred to as plugins . In this  case **Bridge program** is a Plugin for **CNI** . 

**CNI** defines a set of responsibilities for **container run times and plugins**. 

- For **Container Run Time** , **CNI** specifies that it is responsible for creating a **Network namespace** for each container . It should then identify the networks the container must attach to, **container runtime** must then invoke the plugin when the container is created using **add command** and also invoke the plugin when the container deleted **del command**

- It also specifies how to configure a network plugin on the **Container runtime environment** using JSON file.

- On the **Plugin side** it defines that the plugin should support **add, del and check command** line arguments and these should accept parameters like **container and network namespace** . The **plugin** should take care of assigning IP addresses to the Pods and any associated routes required for the containers to reach other containers in the network.

As long as the **Container Runtime** and **Plugin** adhere to these standards they can all live together in harmony . Any runtime should be able to work with any **Plugin** 

When Kubernetes create Docker container, I create them on the **Non-network** . It then invoke the configured CNI plugins who take care of the rest of configuration 

## Cluster Networking 

K8 Cluster consist of **Master and Worker Nodes** . Each Nodes must have at least **1 Interface** connected to a network 

Each **Interface** must have an address cinfigured . The **hosts** must have a unique **host name set and unique MAC address** 

NOTE: Especically if I created the VMs by cloning from existing ones . 

There are some **Ports** that needs to be opened as well . These are used by various components in the **Control Plane** 

The Master should accept on **6443 Port for API-Server** . The Worker Nodes, kubectl tools, external users and all other control plane components access the **Kube API server at port 6443** 

The **Kubelet** on the Master and Worker Nodes listen on Port **10250** 

The **Kube Scheduler** requires port **10259** to be open 

The **Kube Controller Manager** require port **10257** to be open 

The Work Nodes expose services for external access on **port 30000 to 32767** 

The **ETCD Server** listens on port **2379** . Also need an additional port **2380** open so the **ETCD Client** can communicate with each other as well (In case multiple Master Node) 

## Pod-Networking

There is Network that connect the **Nodes** together . But there is also another layer of Networking that is crucial to the Cluster functioning **Pod Networking** 

**K8s Requirment** :

- To get its own unique UP address . And that every Pod should be able to reach every other Pod within **the same node** using that IP address

- And every Pod should be able to reach every **other Pod on the Other Nodes** 

**How to implement?**

Let's say we have 3 Nodes Cluster 

The Nodes are part of an **External Network 192.168.1.x** 

## CNI in Kubernetes

CNI define the responsibilities of container runtime.

As per CNI, container runtime, Kubernetes is responsible for **creating container Network Namespace**, **Identify and attaching those namespace** to the right Network by calling the Network Plugin

**Where to specify Network Plugin** 

The CNI plugin must be invoked by the component within Kubernetes that is responsible for creating Container bcs that component must then invoke the appropriate Network plugin after the container is created 

**How to configure container runtime to use a particular plugin?** 

**Network Plugins** are all installed in **/opt/cni/bin** . That's where the container runtimes find the plugins 

But which Plugin to use and how to use it is configured in the **/etc/cni/net.d** . There may be multiple configuration files in this directory that's responsible for configuring each plugin . 

**bridge con file** : `cat /etc/cni/net.d/10-bridge.conf`

- **isGateway** defines whether the bridge interface should get an IP address assigned so it can act as a Gateway 

- **ipMasq**: defines if a NAT rule should be added for IP masquerading

- **ipam**: Define IPAM configuration . Thus is where I define a Subnet that will be assigned to Pods in nessesary route

- **type: host-local** indicates that the IP addresses are managed locally on this host

- **type: DHCP server**: maintaining it remotely

```
{
  "cniVersion": "0.2.0",
  "name": "mynet",
  "type": "bridge",
  "bridge": "cni0",
  "isGateway": true,
  "ipMasq": true,
  "ipam": {
    "type": "host-local",
    "subnet": "10.22.0.0/16",
    "routes": [
      { "dst": "0.0.0.0/0" }
    ]
  }
}
```

## CNI weave 

`kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml`

We had our own custom CNI script that we've built and integrated into Kubelet through CNI 

The **Networking Solution** we set up manually had a routing table which mapped what networks are on what hosts . So when packet is sent from one pod to the other, it goes out to the network, to the Router . And finds its way to the Node that hosts that Pod 

But in larger Environment with hundreds of Nodes, in a Cluster and hundreds of pods on each Node, this is **Not Practical**

The **Weave CNI plugin** is deployed on a Cluster, it deploys an **Agent or Service on each Node**. They communicate with each other to exchange information regarding the Nodes and Network and Pods within them 

Each **agent or peer** stores a topoly of the entire setup . That way they know the Pods and their IPs on the other Nodes . 

**Weave** creates its own bridge on the Nodes and names at **Weave**, then assigns IP address to each network . 

**Single Pod** maybe attached to multiple **Bridge Network** . For example I could have a Pod attached to **Weave Bridge** as well as the **Docker Bridge** created by Docker  

**Weave** makes sure that Pods gets the correct route configured to reach the Agent, And the Agent then takes care of the other Pods 

When packet send from 1 Pod to another Pod on another Node . **Weave** intercepts the packet and identifies that it's on a separate network, it then encapsulates this packet into a new one with new source and destination and sends it across the network . Once on the other side, **The other Weave Agent** retrieves the packet, decapsulates it, and routes the packet to the right Pod 

**How to Deploy Weave** 

**Weave and Weave Peer** can be deploy as Services or Daemons on each Node in the Cluster Manually 

Or if Kubernetes is set up already, then an easier way to do that is to deploy it as Pod in the Cluster 

Once the base Kubernetes system is ready, with Nodes and Networking configured correctly between the Node and the basic control Plane components are deployed, **Weave** can be deployed in the Cluster with : `kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml`

## IPAM Ip address Managment

The reponsible of IPAM is of the CNI plugin . The Network solution provider to take care of assigning IP's to the containers . 

To manage these **IP's** is to store the list of **IP's in the file** and make sure we have necessary code in our script to manage this file properly . This file will be place on each host and manages the IP's of parts on those Nodes 

Instead of coding that ourselves in our script, CNI comes with 2 built-in plugins to which I can outsource this task. 

- In this case the plugin that implement the approach that we followed for managing the IP addresses locally on each host is the **Host local Plugin**

- But it is still our responsibility to invoke that plugin in our script or **we can make our script dynamic to support different kinds of plugin**

- The CNI configuration file has a section called IPAM in which we can specify the type of plugin to be used. The **Subnet and Route** to be used

- This details can be read from our script to invoke the appropriate plugin instead of hardcoding it 

```
{
  "cniVersion": "0.2.0",
  "name": "mynet",
  "type": "net-script",
  "bridge": "cni0",
  "isGateway": true,
  "ipMasq": true,
  "ipam": {
    "type": "host-local",
    "subnet": "10.244.0.0/16",
    "routes": [
      {
        "dst": "0.0.0.0/0"
      }
    ]
  }
}
```

Different network solution providers does it differently 

**How Weavework manage Ip addresses** 

Some IP's assigned by Weave

**Weave** by default allocates the IP ranges **10.32.0.0/12** for the entire network 

From this range the peers decide to split the IP addresses equally between them and assigns one portion to each node . Pods created on these nodes will have IP's in this range . these ranges are configurable with additional options pass while deploying Weave Plugin to a Cluster 

## Service Networking

Service is accessible from all Pods on the Cluster 

While a Pod is hosted on a Node, a **Service** is hosted across the cluster . **Service** not bound to specific Node 

**Cluster IP** is Only accessible from within a Cluster 

**Node Port**  just like **Cluster IP** but in addition it also **expose** the application on a **Port** on **all Nodes** in the Cluster . This way external User have access to a Service 

**How are the Services getting these IP Addresses?** | **How do they made available across all the Nodes in the Cluster?** | **How do Service available to external User through Port on each Node** 

**Kubelet** run on every Worker Nodes. Each **Kubelet service on each Node watches **the change** in the Cluster through the **Kube API Server** 

- Every time new Pod created. It then invoke **CNI Plugin** to configure networking for that Pod

Similarly each Node runs another component known as **kube-proxy** 

- **Kube-proxy** watch **the changes** in the Cluster through **Kube API-Server** . Everytime new **Service** created **Kube-proxy** get action

- **Services** are Cluster-wide concept .

- There is **no processes or namespaces or interfaces** for a **Service** . It's a **Virtual Object**

When we create **Service Object** in Kubernetes . It is assigned an IP address from a **predefined range**. 

The **kube-proxy** components running on each Node gets that IP and creates **forwarding rules** on each Nodes in the Cluster  

To create **Forwarding Rules** : 

- **Kube-proxy** support **userspace** where **kube-proxy** listen on a Port

- For each **Service** and proxy's connection to the Pods by creating **ipvs** rules

- Default Option is using **iptables**

  - The proxy mode can be set using the proxy mode option while configuring the **kube-proxy** service . If this not set : `kube-porxy --proxy-mode [userspace | iptables | ipvs]` it defailt to **iptables**

**How iptables are configured by kube-proxy and how can view them**

For example I have a Pod name DB with IP 10.244.1.2 

We create a service of that Cluster IP to make this Pod available within the Cluster . When the Service is created, Kubernetes assigns an IP address to it . It set to `10.103.132.104`

This range is specified in the **Kube API Service** option called `--service-cluster-ip-range ipNet default: 10.0.0.0/24`

When check **API server optione** : `--service-cluster-ip-range=10.96.0.0/12`

When I set up my Pod networking, I provided a Pod network **CIDR 10.244.00/16** 

Whatever range I specify for each of these Network, it shouldn't overlap 

**POD and Services** should have its own dedicated of IP to work with. 

I can see the rules created by **kube-proxy** in the IP tables's NAT table output : `iptables -L -t nat | grep db-service`

Any traffic going to the IP address `10.103.132.104:3306` destinate to `10.244.1.2`. This done by adding a **DNAT rule to iptables**

To see kube-proxy logs : `cat /var/log/kube-proxy.log`
 
## Cluster-DNS 

Kubernetes deploy a built-in DNS Server by default when I set up a Cluster 

Whenever a service is created, the Kubernetes DNS Service create a record for a Service . It maps a Service Name to the IP address 

If Service is in a separate Namespace. To talk to that Service I would do : `<service-name>.<service-namespace>` 

For each **Namespace** the DNS Server create a **Sub domain**. All **Services** are group together into another **Sub domain called SVC** : `<service-name>.<service-namespace>.svc`

All Pods and Services for a **namespace** are grouped together within a **Sub domain** in the name of a **namespace** 

## How Kubernetes Implements DNS 
 
**How do CoreDNS set up?**

The **CoreDNS** Server is deploy **as a Pod** in the Kube system namespace in the Kubernetes Cluster 

This Pods run **CoreDNS executable** .

**CoreDNS** require a configuration file . `cat /etc/coredns/Corefile`

```
cat /etc/coredns/Corefile
.:53 {
    errors
    health
    kubernetes cluster.local in-addr.arpa ip6.arpa {
        pods insecure
        upstream
        fallthrough in-addr.arpa ip6.arpa
    }
    prometheus :9153
    proxy . /etc/resolv.conf
    cache 30
    reload
}
```

The Plugin help coreDNS work with Kubernetes is `kubernetes cluster.local in-addr.arpa ip6.arpa`. This is where Top Level Domain Name of Cluster is set : `cluster.local`

## Gateway API

Ingress is 2 or more **Services** manage by the same **Ingress Resource** 

In multi Environment Ingress can not : 

- Namepsace Isolation

- No RBAC for features

- No Resource Isolation

- Rules configuration . Ingress only support **HTTPS base rules** 

What if each **Services** manage by different teams ?  

**Gateway API** is an official Kubernetes project focused on **Layer 4 and Layer 7 routing** . This Service focus on the next generation of **Ingress, Load balancingm Service Mesh APIs** 

**Infrastucture Providers GatewayClass**: What the underlying network infastructure would be such as **Nginx, Traefik, or other Load balancer** 

**Cluster Operators Gateway**: Which are instances of the **Gateway Class** 

And then we have **HTTP, TCP, GRPC routes** by the application Developers 

Similar to **Ingress** we must deploy a **Controller for Gateway**

```
# gateway-class.yaml
apiVersion: gateway.networking.k8s.io/v1
kind: GatewayClass
metadata:
  name: example-class
spec:
  controllerName: example.com/gateway-controller
```

```
# gateway.yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: example-gateway
spec:
  gatewayClassName: example-class
  listeners:
    - name: http
      protocol: HTTP
      port: 80
```

```
# http-route.yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: example-httproute
spec:
  parentRefs:
    - name: example-gateway
  hostnames:
    - "www.example.com"
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: /login
      backendRefs:
        - name: example-svc
          port: 8080
```












































