- [ETCD](#ETCD)
  
  - [ETCD for Kubernetes](#ETCD-for-Kubernetes)
 
  - [Set up Cluster using kubeadm](#Set-up-Cluster-using-kubeadm)
 
  - [High Availability Considerations](#High-Availability-Considerations)
 
- [Kube API Server](#Kube-API-Server)

  - [Deployment and Setup](#Deployment-and-Setup)
 
  - [Verifying the Deployment](#Verifying-the-Deployment)
 
- [Kube Controller Manager](#Kube-Controller-Manager)

- [Kube Scheduler](#Kube-Scheduler)

  - [Installing and Running the Kube Scheduler]
 
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































