# Master services   

![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s5_administration/pictures/architecture_overview.png)    
[source](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/)

To communicate with cluster we are using `kubectl` which is api client and needs authorization.
When we send a new object like `pod definition` it will be stored in `etcd` - separate technology like data store, and rest interface 
communicate with this data store.

`Scheduler` communicates with `rest api` server for schedule new objects. You can use default k8s scheduler or another plugin.

`Controller manager` exist of multiple controllers(node controller, replication controller)

Also `rest interface` communicate with kubelet, which is live on every managed nodes

# Resource Quotas   

Using for separate access to k8s quotas, using ResourceQuota and ObjectQuota objects.   

EAch container can specify `request capacity` and `capacity limits`:    
- **Request capacity** is an explicit request for resources 
    - The scheduller can use the request capacity to make decisions on where to put the pod on  
    - You can see it as a minimum amount of resources the pod needs 
    
- **Resource limit** is a limit imposed to the container
    The container will not be able to utilize more redources than specified 
  
### Resource quotas example         
- You run a deployment with a pod with a CPU resource request of 200m   
- 200m = 200 milicpu = 0.2 of a CPU core of the running node. IF the node has 2 codes, it's still 20% of a single core  
- Memory quotas are defined by MiB or GiB

If a capacity quota has been specified by the administrator, then each pod needs to specify capacity qouta during creation.
You can specify default request values for pods. The same valid for limit quotas. A resource which is requested more then the allowed
capacity, the api will give 403.    

Administrator can specify object limits:    
- configmaps    
- persistentvolumeclaims    
- pods
- services  
- ...

# Namespaces    

Namespaces allow ypu to create virtual clusters within the same physical cluster, they are logically separates your cluster.
There is also namespace for kubernetes specific resources, called `kube-system`.

The name of resources need to be unique within a namespace, but not across namespaces.  

# User management   

You can create 2 types of users:    
- Normal User: used to access the user externally through kubectl. This user is not managed using objects   
- Service user: which is managed by an object in Kubernetes, is used to authenticate within the cluster (from inside a pod or kubelet)
This credentials are managed like `Secrets` 
  
User authentication strategies for normal users:    
- CLient Certificates   
- Bearer Tokens 
- Authentication Proxy  
- HTTP Basic Authentication 
- OpenID    
- Webhooks  

Service Users are using `Service Account Tokens`. They are stored as credentials using Secrets, those Secrets are also mounted in pods to allow communication
between the services. Service Users are specific to a namespace. They are created automatically by the API or manualy using objects.
Any API call not authenticated is considered as an anonymous user.      

Authorization configuration:    
- AlwaysAllow/AlwaysDeny    
- ABAC (Attribute-based access control) 
- RBAC (role-based access control)  
- Webhook (authorization by remote service)     

RBAC types: 
- Role: for namespace   
- ClusterRole: for cluster wide 

# Networking    

In Kubernetes, the pod itself should always be routable.
Every pod has its own IP address. Pods on different nodes need to be able to communicate to each other using those IP addresses.
This is **implemented differently depending on you network setup**  

On AWS: kubenet networking  
- Every pod can get an IP that is routable using AWS VPC    
- The k8s master allocates a /24 subnet to each node    
- This subnet is added to the VPCs route table  

If you deploy cluster on premice or on cloud without VPC you have to use:   
- CNI: container network interface - software that provides libraries/ plugins for network interfaces within containers(Calico, Weave)  
- An Overlay Network (for example `Flannel`)    
![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s5_administration/pictures/flannel.png)      
[source](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/)
  

# Node maintenance  

Node controller that is responsible for managing the Node objects:  
- it assigns IP space to the node when a new node is launched   
- it keeps the node list up to date with the available machines 
- monitoring the health of the node, if node unhealthy it gets deleted and pods of this node will be resheduled     

When adding a new node, the kubelet will attempt to register itself (self-registration).    
A new object is automatically created with: 
- the metadata (wit IP or hostname) 
- labels    
- node condition

if you want to decommision a node:  
- `kubectl drain nodename --grace-period=600`   
- `kubectl drain nodename --force`  

# TLS on AWS ELB  

It's possible to setup tls using annotations. Also you can setup logging with annotations:    
![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s5_administration/pictures/log_annotations_in_aws.png)       
[source](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/)

![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s5_administration/pictures/tls_annotations.png)  
[source](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/)

# Admission Controllers     

An admission controller can intercept requests sent to the K8s API server. For example, when you create new pod, the request can be intercepted
by an admission controller. This interception happens after the user is authenticated (token or cert) and authorized (for ex RBAC)
and before the object is persisted (saved) in the backend   

Great list of controllers in [official demo](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/)    

You can use admission controllers and their webhooks to run your custom software for this hooks:
![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s5_administration/pictures/admission_controlling.png)        
[source](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/)

# Pod security Polices  

Pod security Polices is an admission controller, which will be invoked at pod creation or modification

Pod security Polices enable you to do control the security aspects of the pods creation & updates:
- Deny using privileged mode in pods    
- Control what volumes can be mounted
- Make sure containers only run within a UID/Gid range, or make shure that containers can't run as root

# etcd

etcd is a distributed and reliable key-value store for the most critical data of a distributed system and used by k8s
as data backend. It's secure with automatic TLS and optional client certificate authorization, and using Raft consensus algorithm

