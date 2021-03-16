# Skaffold  

Skaffold is an open source project from Google. 
it's a cli for CD of apps that can run on Kubernetes.   
Skaffold will handle:   
- building, for example with docker build   
- pushing to docker hub or others repo  
- Deploying, to k8s kluster     

Skaffold can monitor your application changes while you are developing it. When change happens it can execute workflow to deploy app immediatelly to the k8s Kluster.
Also you can integrate it to CI\CD pipeline.    

You can do the build with local docker installation, or with alternatives like in-cluster build with Kaniko or even a remote build on Google Cloud Build    
Also possible **Bazel, build packs or custom builds**   
Deploying can be done with kubectl or helm.     

Skaffold pipeline:  

![alt text](https://github.com/SavelevArtemD/kubernetes-notes/blob/master/udemy_course/s7_CD/pictures/skaffold_pipeline.png)    
[source](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/)
