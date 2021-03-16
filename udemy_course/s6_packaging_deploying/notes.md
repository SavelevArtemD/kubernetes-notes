# Helm

It's package manager for Kubernetes? maintened by CNCF.     
Helm using a packaging format called charts:    
- Chart is a collection of files that describe a set of Kubernetes resources    
- A singe chart can deploy an app, a piece of software, or a database   
- it can have dependencies  
- chart use templates that are typically developed by a package maintainer  
- they will generate yaml files that k8s understands    
= templates like dynamic yaml files, which can contain logic and variables  
  
You can setup helm repository on S3
