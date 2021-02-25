# What is Kubernetes
Kubernetes is a portable, extensible platform for managing containerized workloads and services,
that facilitates both declarative configuration and automation.

### Kubernetes provides:
- Service discovery and load balancing
- Storage orchestration
- Automated rollouts and rollbacks
- Automatic bin packing
- Self-healing
- Secret and configuration management

### What Kubernetes is not
- Does not limit the types of applications supported
- Does not deploy source code and does not build your application
- Does not dictate logging, monitoring, or alerting solutions
- Does not provide nor mandate a configuration language/system
- Does not provide nor adopt any comprehensive machine configuration, maintenance, management, or self-healing systems
- Kubernetes is not a mere orchestration system. In fact, it eliminates the need for orchestration

# Kubernetes Components

A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications.
Every cluster has at least one worker node. The worker node(s) host the Pods that are the components of the application workload.
The control plane manages the worker nodes and the Pods in the cluster.

### Control Plane Components
The control plane's components make global decisions about the cluster (for example, scheduling),
as well as detecting and responding to cluster events
(for example, starting up a new pod when a deployment's replicas field is unsatisfied).

- kube-apiserver
    The API server is a component of the Kubernetes control plane that exposes the Kubernetes API.
    The main implementation of a Kubernetes API server is kube-apiserver.

- etcd
    Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.

- kube-scheduler
    Control plane component that watches for newly created Pods with no assigned node, and selects a node for them to run on.

- kube-controller-manager
    Logically, each controller is a separate process, but to reduce complexity,
    they are all compiled into a single binary and run in a single process.
    These controllers include:

    - Node controller: Responsible for noticing and responding when nodes go down.
    - Replication controller: Responsible for maintaining the correct number of pods for every replication controller object in the system.
    - Endpoints controller: Populates the Endpoints object (that is, joins Services & Pods).
    - Service Account & Token controllers: Create default accounts and API access tokens for new namespaces.

- cloud-controller-manager
    A Kubernetes control plane component that embeds cloud-specific control logic

### Node Components

- kubelet
    An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod.

- kube-proxy
    kube-proxy is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.