# Serverless functions in K8s   

Rather than using containers to start applications on kubernetes, you can also use  Functions.  
Most popular projects enabling functions are:   
- OpenFaas  
- Kubeless  
- Fission   
- OpenWhisk 

You can install and use any of the projects to let developers launch functions on your Kubernetes cluster.  

# Kubeless  

[github](https://github.com/kubeless/kubeless)  
It leverages the Kubernetes resources to provide auto-scaling, API routing, monitoring, etc. It uses Custom Resource Definitions 
to be able to create functions. 

Functions are supported:    
- HTTP functions:   
  - http functions gets executed when an http endpoint is triggeresd    
  - you write function and return the text/html
  - Using `Ingress controller`  
  - Sheduled functions
- PubSub (kafka or NATS)    
    - Triggers a functions when data is available   
-AWS Kinesis    
      - Triggers based on data in AWS Kinesis

  