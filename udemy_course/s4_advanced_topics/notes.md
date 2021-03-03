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

