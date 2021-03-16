# Flux  

FLux automates the deployment of containers to K8s. it can sync your version control (git) and your K8s cluster.
You can put manifest files within your git repository. Flux will monitor this repo and make shure that manifests are deployed.
Flux also can automatically upgrede your containers to the latest version available.    

With flux, you declaratively describer the entire desired state of your system in git.  

Flux overview:  
![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s8_gitops/pictures/flux_pipeline.png)        
[source](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/)



