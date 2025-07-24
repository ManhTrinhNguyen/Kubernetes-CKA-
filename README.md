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








