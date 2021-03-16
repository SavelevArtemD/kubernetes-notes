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

# AWS EKS   

AWS EKS - fully managed by AWS K8s cluster. You pay a fee for every cluster you spin up (to pay for the master nodes)
and then you pay per EC2 worker that you attach to your cluster.

Some EKS features:  
- AWS created its own VPC CNI for EKS 
- AWS can even manage you workers to ensure updates are applied to your workers 
- Cluster authentication is using IAM, so you don't need to setup your own users within the cluster, use IAM  
- Service Accounts can be tied IAM roles to use IAM roles on a pod-level  
- Integrates with many other AWS services(Like Cloud Watch for logging)   
- CLI eksctl available 

### IRSA  

EKS supports IAM Roles for Service Accounts (IRSA)  
With this feature you can specify IAM policies at a pod level (one pod have access to s3 backet, others not)  
With IAM Roles for Service Accounts, it lets you hand out permissions on a more granular level  
IAM Rples for Service Accounts uses the IAM OpenID Connect provider (OIDC) that EKS exposes 
To link a IAM Role with a Service Acoount, you need to add an annotation to the Service Account 
The EKS Pod Identity Webhook will then automatically inject environment variables into the pod that have this ServiceAccount assigned 
These environment variables will be picked up by the AWS SDK during authentication  

