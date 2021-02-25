## Kubernetes objects   

[Documentation](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)

Kubernetes objects are persistent entities in the Kubernetes system. 
Kubernetes uses these entities to represent the state of cluster.   

A Kubernetes object is a "record of intent"--once you create the object, 
the Kubernetes system will constantly work to ensure that object exists. 
By creating an object, you're effectively telling the Kubernetes system what 
you want your cluster's workload to look like; this is your cluster's desired state.    

### Object Spec and Status  
Almost every Kubernetes object includes two nested object fields:   
- **spec**: desired state
- **status**: current state 

### Kubernetes Object Management    
#### Imperative commands    
When using imperative commands, a user operates directly on live objects in a cluster. 
The user provides operations to the kubectl command as arguments or flags.

#### Imperative object configuration    
In imperative object configuration, the kubectl command specifies the operation (create, replace, etc.), 
optional flags and at least one file name. 
The file specified must contain a full definition of the object in YAML or JSON format. 

#### Declarative object configuration   
When using declarative object configuration, 
a user operates on object configuration files stored locally, 
however the user does not define the operations to be taken on the files. 
Create, update, and delete operations are automatically detected per-object by kubectl. 
---

## Namespaces   
**Namespace** - virtual clusters backed by the same physical cluster. 
Low-level resources, such as nodes and persistentVolumes, are not in any namespace.    

```bash
# In a namespace
kubectl api-resources --namespaced=true

# Not in a namespace
kubectl api-resources --namespaced=false

```
--- 
## Labels and Selectors
**Labels** - key/value pairs that are attached to objects.  

### Label selectors     
The API currently supports two types of selectors: equality-based and set-based.    

- **Equality-based requirement**    
    Equality- or inequality-based requirements allow filtering by label keys and values. 
    Matching objects must satisfy all of the specified label constraints, t
    hough they may have additional labels as well.
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: cuda-test
    spec:
      containers:
        - name: cuda-test
          image: "k8s.gcr.io/cuda-vector-add:v0.1"
          resources:
            limits:
              nvidia.com/gpu: 1
      nodeSelector:
        accelerator: nvidia-tesla-p100
    ```
- **Set-based requirement**     
    Set-based label requirements allow filtering keys according to a set of values. 
    Three kinds of operators are supported: in,notin and exists (only the key identifier).    
  
Examples:   
```bash
kubectl get pods -l environment=production,tier=frontend
kubectl get pods -l 'environment in (production),tier in (frontend)'
kubectl get pods -l 'environment in (production, qa)'
kubectl get pods -l 'environment,environment notin (frontend)'
```
---

## Annotations  
Using to **attach arbitrary non-identifying metadata to objects**. Clients such as tools and libraries can retrieve this metadata.      
Examples:   
- Build, release, or image information like timestamps, release IDs, git branch 
- Pointers to logging, monitoring, analytics, or audit repositories.
- User or tool/system provenance information, such as URLs of related objects from other ecosystem components.
```yaml

apiVersion: v1
kind: Pod
metadata:
  name: annotations-demo
  annotations:
    imageregistry: "https://hub.docker.com/"
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```