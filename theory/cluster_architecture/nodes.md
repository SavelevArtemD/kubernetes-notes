# Nodes 

Kubernetes runs your workload by placing containers into Pods to run on Nodes. 
A node may be a virtual or physical machine, depending on the cluster. 
Each node is managed by the control plane and contains the services necessary to run Pods.  

## Management   

Ways to add Nodes to the API Server:    
- The kubelet on a node self-registers to the control plane 
- Manually add a Node object    

### Self-registration of Nodes  

When the kubelet flag --register-node is true (the default), 
the kubelet will attempt to register itself with the API server.    

For self-registration, the kubelet is started with the following options:

    --kubeconfig - Path to credentials to authenticate itself to the API server.

    --cloud-provider - How to talk to a cloud provider to read metadata about itself.

    --register-node - Automatically register with the API server.

    --register-with-taints - Register the node with the given list of taints (comma separated <key>=<value>:<effect>).

    No-op if register-node is false.

    --node-ip - IP address of the node.

    --node-labels - Labels to add when registering the node in the cluster (see label restrictions enforced by the NodeRestriction admission plugin).

    --node-status-update-frequency - Specifies how often kubelet posts node status to master.

### Manual Node administration  

You can create and modify Node objects using kubectl.   
When you want to create Node objects manually, set the kubelet flag --register-node=false.  


## Node status  

- Addresses     
    - HostName
    - ExternalIP    
    - InternalIP
- Conditions    
    - Ready:  if the node is healthy and ready to accept pods   
    - DiskPressure: if pressure exists on the disk size--that is, if the disk capacity is low    
    - MemoryPressure: if pressure exists on the node memory--that is, if the node memory is low
    - PIDPressure: if pressure exists on the processes—that is, if there are too many processes on the node
    - NetworkUnavailable: if the network for the node is not correctly configured
- Capacity and Allocatable  
    Describes the resources available on the node: CPU, memory and the maximum number of pods that can be scheduled onto the node.
- Info  
    
### Node controller     
The node controller is a Kubernetes control plane component that manages various aspects of nodes.

The node controller has multiple roles in a node's life. The first is assigning a CIDR block to the node when it is registered  
The second is keeping the node controller's internal list of nodes up to date with the cloud provider's list of available machines.
The third is monitoring the nodes' health.

### Heartbeats  
There are two forms of heartbeats:  
- NodeStatus: he kubelet updates the NodeStatus either when there is change in status
- Lease object: Each Node has an associated Lease object in the kube-node-lease namespace. Lease is a lightweight resource, 
  which improves the performance of the node heartbeats as the cluster scales. 

