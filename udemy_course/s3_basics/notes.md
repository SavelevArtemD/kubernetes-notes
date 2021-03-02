[lecture url](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6058574#overview)  

# Node Architecture     
Containers can communicate with each other throw `localhost` and port number    

`Kubelet` is responsible to launch nodes and commmunicate with the master node  
`Kube-proxy` feed information about pods to `iptables` - like firewall whis is root traffic.
Also have rules to route traffic between nodes.     

## Scaling  
If application stateless - you can scale it horizontally.   
Stateless - application, doesn't have state, it doesn't write any local files/ keeps local sessions.

Session management needs to be done outside the container.  

Stateful apps uses volumes and can be vertically scaled     

# Deployments
`Replica Set` is the nex generation Replication Controller(which is depricated).
It supports a new selector that can do selection based on filtering according a set of values   

A deployment declaration in Kubernetes allows you to do app deployment and updates.
In deployment you define the state of application? and Kubernetes will then make sure
the clusters matches this state.    

Deployments actions:    
- Create
- Update    
- Rolling updates
- Roll back to previous version 
- Pause/Resume a deployment     

# Services  

`Services` - logical bridge between the "mortal" pods and other services or end-users   

Types:  
- `CLusterIP`: a virtual IP address only reachable from within the cluster
- `NodePort`: give ability to reach node externally 
- `loadBalancer`: LB    

# Health checks 

Indicates whether a container is running, if the check fails, the container will be restarted

Types of health checks:     
- Periodically command  
- periodic checks on a URL

# Readiness probe   

Indicates whether the container is ready to serve requests. If the check fails, the container
will not be restarted, but the Pod's IP address will be removed from the Service.

# Pod State 

Valid statuses: 
- Pending: pod has been accepted but is not running 
    - container image is still downloading
    - the pod cannot be scheduled bacuse of resource constrains
- Succeeded: all containers within this pod have been terminated successfully and will not be restarted
- Failed: all containers within this pod have been Terminated, and at least one container returned a failture code
- Unknown

# Secrets 
Secrets provides a way in Kubernetes to distribute credentials, keys, psswords or "secret" data to the pods   

Cases:  
- As env variables
- As file:  
  - uses volumes which have files 
  - dotenv files
- External image to pull secrets (from a private img repository)  

  