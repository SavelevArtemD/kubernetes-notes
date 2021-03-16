# Service Discovery     

The DNS service can be used within pods to find other services running on the same cluster. 
Multiple containers within 1 pod don't neeed this service, as they can contact each other directly (via localhost:port)
To make DNS work, a pod will need a Service definition.     

![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s4_advanced_topics/pictures/dns_work.png)    

DNS service runs in the pod under `kube-system` namespace, expose as a service with some ip-address

The /etc/resolv.conf file is modified to make sure all DNS lookups go to an internal DNS server. it makes possible 
to execute command `curl service1`

# Config map    
Configuration parameters that are not secret, in key-value pairs
Can be read by app using:   
- Env variables 
- Container commandline arguments int the Pod configuration
- Volumes   

Can also contain full configuration files (like webserver), which then can be mounted using volumes.

# Ingress Controller    
Ingress is a solution that allows inbound connections to the cluster. It's an alternative to the external LoadBalancer
and nodePorts.  
There are a default ingress controllers available, or you can write your own ingress controller 

With an ingress controller you can potentially save costs. Rather than using 1 LoadBalancer per public application, 
you can use the ingress controller as a gateway for all your public apps, 
and only use 1 LoadBalancer in front of the ingress-controller

![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s4_advanced_topics/pictures/ingress.png)     

# External DNS   

On public cloud providers, it's possible to use the ingress controller to reduce the cost of LoadBalancer 
- 1 LoadBalancer that captures all the external traffic and send it to the ingress controller   
- The ingress controller can be configured to route the different traffic to all
apps based on HTTP rules (host and prefixes)    
- Works only for HTTP-based applications  

External DNS automatically create the necessary DNS records in your external DNS server (like route 53) 
For every hostname that you use in ingress, it'll create a new recod to send traffic to your LoadBalancer.  

![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s4_advanced_topics/pictures/external_DNS.png)

# Volumes    

VOlumes allow store data outside the container. When a container stops, all data on the container itself is lost.   
Persistent Volumes in Kubernetes allow you attach a volume to a container that will wxists even when the container stops.  

You have a Kubernetes cluster within a single availability zone on AWS. The pod "app1" has a persistent volume (AWS EBS) attached. 
The pod get killed, what happens with the data in the persistent volume?
**Answer:** You have a Kubernetes cluster within a single availability zone on AWS. The pod "app1" has a persistent volume (AWS EBS) attached. 

![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s4_advanced_topics/pictures/volumes.png)     
[source](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/)

# Pod Presets    

Pod presets can inject information into pods at runtime. Pod Presets are used to inject k8s resources like Secrets, COnfigMaps,
VOlumes and Environment variables.  
For example, if you have 20 application, you are able to create 1 Preset object, which will inject an environment variable
or config file to all matching pods.
When injecting Environment variables and VolumeMounts, the Pod Preset will apply the changes to all containers within the pod

You can use more then one PodPreset, the'll all be applied to matching Pods.    
If there's conflict, the podPreset will not be applied to the pod.  
Pod presets can match one or more pods. It's also posssible that no pods are currently matching, but that matching pods
will be launched at later time  

# StatefulSets   

StatefulSets give you possibility to run statefull applications:
- which needs a stable pod hostname (pod will have a sticky identity, using an index and when a pod gets resheduled, it'll keep that identity) 
- which needs stable storage with volumes on their ordinal number. Deleting and/or scaling a StatefulSet down will not delete the volumes associated
with the StatefulSet
  
Also will allow your stateful app to use DNS to find other peers. For example Cassandra clusters, ElasticSearch clusters, use DNS to find other
members of tthose clusters (DNS: cassandra-o.cassandra) 

# Daemon Sets    

Daemon sets ensure that every single node in the k8s cluster runs the same pod resource.    
When a node is added to the cluster, a new pod will be started automatically. when node is removed, the pod will not be resheduled 
on another node.    

Use cases:  
- Logging aggregators   
- Monitoring    
- Load Balancers/ Reverse Proxies/ API gateways
- Some daemon   

# Autoscaling    

K8s has the possibility to automatically scale pods based on metrics.   
Automatically scale:    
- Deployment    
- Replication Controller    
- ReplicaSet    

Autoscaling will periodically query the utilization for the targeted pods.  

If you run a deployment with a pod a CPU resource  request of 200m. 
200m = 200 millicpu 
200m = 0.2, which is 20% of a CPU core of the running node. 
You introduce auto-scaling at 50% of the CPU usage (which is 100m).
Horizontal Pod Autoscaling will increase/decrease pods to maintain a target CPU utulization of 50%

# Affinity   

It's feature allows to do more complex scheduling than the nodeSelector and also works on Pods. 
The language is more expressive.    
You can create rules that are not hard requirements, but rather a preferred rule, meaning that the scheduler will still
be able to schedule your pod, even if the rules cannot be met.  
You can create rules that take other pod labels into account. For example, a rule that makes sure 2 different pods will never be on the
same node.  

K8s can do node affinity and pod affinity/anti-affinity:    
- Node affinity is similar to the node Selector     
- Pod affinity/anti-affinity allows you to create rules how to pods should be scheduled taking into account other running nodes     
- Affinity/anti-affinity mechanism is only relevant during scheduling, once a pod is running, it'll need to be recreated to apply the
rules again 
  
Affinity/anti-affinity types:   
- requiredDuringSchedulingIgnoredDuringExecution: sets a hard requirement (like nodeSelector). The rules must be met before
the pod can be scheduled    
- preferredDuringSchedulingIgnoredDuringExecution: will try to enforce the rule, but it will not guarantee it.
Even if the rule is not met? the pod can still be scheduled, it's a soft requirement    
  
# Interpod Affinity and anti-affinity   

mechanism allows you to influence scheduling based on the labels of other pods that are already running on the cluster. 
pods belong to a namespace, so your affinity rules will apply to a specific namespace (if no namespace given in the specification,
it dafaults to the namespace of the pod)    

Types:  
- requiredDuringSchedulingIgnoredDuringExecution    
- preferredDuringSchedulingIgnoredDuringExecution   

Main use case: co-located pods (Redis near with the app, pods in the same availability zone and so on)  

When writing pod affinity rules, you need to specify a topology domain, called topologyKey which is refers to a node label.

![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s4_advanced_topics/pictures/interpod_affinity.png)       
[source](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/)

# Taints and tolerations    

Tolerations is the opposite of node affinity. Tollerations allow a node to repel a set of pods. 
Taints mark a node, tolerations are applied to pods to influence the scheduling of the pods.    

To add a taint: 
```bash
kubectl taint nodes node1 key=value:NoSchedule
```

Taint types:    
- NoSchedule: a hard requirement that a pod will not be scheduled unless there is a matching toleration 
- PreferNoSchedule: soft requirement    

If the taint is applied while there are already running pods, these will not be evicted, unless the following taint type is used:
- NoExecute: evict pods with non-matching tolerations. You can specify how long the pod can run on the evicted node.

