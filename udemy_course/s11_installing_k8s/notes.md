# Kubeadm       

It's toolkit by Kubernetes to create a cluster. It works on any deb/rpm compatible Linux OS, for example Ubuntu, Debian, Redhat or Centos.  

Kubeadm supports bootstrap tokens - tokens that can be used to create a cluster, or to join nodes later on. Supports upgrading/downgrading clusters.
It does not install a networking solution. You have to install a CNI yourself.  

Requirements:   
- 2 GB RAM  
- 2 CPUs for the master node    
- network connectivity between the nodes    
    - Can be a private network (internal IP addresses)  
    - or public routable internet IP addresses (in this case its best to use a firewall to only allow access within the cluster and to the users)
- minimal 2 nodes (for master and for pods)     

[Installation instruction](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)