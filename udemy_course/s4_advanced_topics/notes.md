# Service Discovery     

The DNS service can be used within pods to find other services running on the same cluster. 
Multiple containers within 1 pod don't neeed this service, as they can contact each other directly (via localhost:port)
To make DNS work, a pod will need a Service definition. 

DNS service runs in the pod under `kube-system` namespace, expose as a service with some ip-address

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


#External DNS   

On public cloud providers, it's possible to use the ingress controller to reduce the cost of LoadBalancer 
- 1 LoadBalancer that captures all the external traffic and send it to the ingress controller   
- The ingress controller can be configured to route the different traffic to all
apps based on HTTP rules (host and prefixes)    
- Works only for HTTP-based applications  

External DNS automatically create the necessary DNS records in your external DNS server (like route 53) 
For every hostname that you use in ingress, it'll create a new recod to send traffic to your LoadBalancer.  

#Volumes    

VOlumes allow store data outside the container. When a container stops, all data on the container itself is lost.   
Persistent Volumes in Kubernetes allow you attach a volume to a container that will wxists even when the container stops.   

#Pod Presets    

Pod presets can inject information into pods at runtime. Pod Presets are used to inject k8s resources like Secrets, COnfigMaps,
VOlumes and Environment variables.  
For example, if you have 20 application, you are able to create 1 Preset object, which will inject an environment variable
or config file to all matching pods.
When injecting Environment variables and VolumeMounts, the Pod Preset will apply the changes to all containers within the pod

You can use more then one PodPreset, the'll all be applied to matching Pods.    
If there's conflict, the podPreset will not be applied to the pod.  
Pod presets can match one or more pods. It's also posssible that no pods are currently matching, but that matching pods
will be launched at later time  

#StatefulSets   

StatefulSets give you possibility to run statefull applications:
- which needs a stable pod hostname (pod will have a sticky identity, using an index and when a pod gets resheduled, it'll keep that identity) 
- which needs stable storage with volumes on their ordinal number. Deleting and/or scaling a StatefulSet down will not delete the volumes associated
with the StatefulSet
  
Also will allow your stateful app to use DNS to find other peers. For example Cassandra clusters, ElasticSearch clusters, use DNS to find other
members of tthose clusters (DNS: cassandra-o.cassandra) 

#Daemon Sets    

Daemon sets ensure that every single node in the k8s cluster runs the same pod resource.    
When a node is added to the cluster, a new pod will be started automatically. when node is removed, the pod will not be resheduled 
on another node.    

Use cases:  
- Logging aggregators   
- Monitoring    
- Load Balancers/ Reverse Proxies/ API gateways
- Some daemon   

#Autoscaling    

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