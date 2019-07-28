https://docs.google.com/presentation/d/1zrfVlE5r61ZNQrmXKx5gJmBcXnoa_WerHEnTxu5SMco/edit#slide=id.g309fc81a05_0_197

https://ramitsurana.gitbooks.io/awesome-kubernetes/content/#cloud-providers
https://medium.com/faun/how-to-pass-certified-kubernetes-administrator-cka-exam-on-first-attempt-36c0ceb4c9e
# Control Plane Components

- kube-apiserver
- etcd
- kube-controller-manager
- kube-scheduler

## kube-apiserver

- Provides a forward facing REST interface into the control plane & datastore.
- All clients and other applications interact with kubernetes **strictly** through the API server.
- Acts as a gatekeeper to the cluster by handling authentication & authorization, request validation, mutation, and admission control in addition to being the front-end to the backing datastore.

## etcd

- etcd acts as a cluster datastore.
- Purpose in relation is to provide a stong, consistent and highly available key-value store for persisting cluster state.
- Stores objects and config information.

## kube-controller-manager

- Serves as the primary daemon that manages all core component control loops.
- Monitors the cluster state via the apiserver and steers the cluster towards the desired state.

## kube-scheduler

- Verbose policy-rich engine that evaluates workload requirements and attempts to place it on a matching resource.
- Default scheduler uses bin packing.
- Workload Requirements can include: general hardware requirements, affinity/anti-affinity, labels, and other various custom resource requirements.

# Node Components

- kubelet
- kube-proxy
- Container Runtime Engine

## kubelet

- Acts as the node agent responsible for managing the lifecycle of every pod on its host.

- the single host daemon required for a being a part of a kubernetes cluster
- can read pod manifests from several different locations
- workers: poll kube-apiserver looking for what they should run
- masters: run the master services as static manifests found locally on the host

## kube-proxy

- Manages the network rules on each node.
  - creates the rules on the host to map and expose services.
  - uses a combination of ipvs and iptables to manage networking/loadbalancing 
- Performs connection forwarding or loadbalancing for Kubernetes cluster services.
- Available proxy Modes.
  - Usespace
  - iptables
  - ipvs

## Container Runtime Engine

# Optional Services

## cloud-controller-manager

- Daemon that provides cloud-provider specific knowledge and integration capability into the core control loop of Kubernetes.
- The controllers include Node, Route, Service, and add an additional controller to handle things such as *PersistentVolume* Labels.

## Cluster DNS

- Provides Cluster Wide DNS for Kubernetes Services.
  - kube-dns (default pre 1.11) kube-dns is a wrapper around dnsmasq with another container in the pod that manages its config
  - CoreDNS (current default) CoreDNS is a single golang  binary with a slew of plugins and much better native kubernetes support

## Kube Dashboard

A limited, general purpose web front end for the Kubernetes Cluster.

## Heapster / Metrics API Server

Provides metrics for use with other Kubernetes Components.

- Heapster (deprecated, removed in Dec)
- Metrics API (current)







## Pod

- The Smallest deployable entity in kubernetes is known as a Pod.
- Pods run inside nodes, and containers run inside the pods.
- Controllers( Deployment, StatefulSets, DaemonSet) create pods.
- Inside a Pod, containers can address other containers with localhost.Since containers also share storage, they can even communicate using Inter Process Communication (IPC) or Semaphores.
- Pods are **one or more containers** that share volumes, a network namespace, and are part of a **single context**


## Pod's Life
This is how a Pod's life goes.

Pods are created.

- Pods are assigned a Unique ID(UID).
- Scheduled to a random Node.
- They run and server requests.
- Wait for graceful termination (according to restart policy).
- If a node dies before that pods are scheduled for recreation, a new pod is created with new UID. The old pod's state is not taken into consideration.

### From CLI
kubectl create -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: private-image-testing
  labels:
    app: demo
    env: test
spec:
  containers:
    - name: private-image-test
      image: $YOUR_PRIVATE_IMAGE_NAME
      imagePullPolicy: Always
      command: [ "echo", "SUCCESS" ]
EOF


## Services

- **Unified method of accessing** the exposed workloads of Pods.
- Durable resource
  - static cluster IP
  - static namespaced DNS name
- **unofficially** Service is an internal load balancer to your pod(s).
- Services are persistent objects used to reference ephermeral resources.


## Cluster Roles

four cluster roles, admin, cluster-admin, edit, and view available by default in this setup

## Namespace

Namespaces provide a scope for names.Names of resources need to be unique within a namespace, but not across namespaces.

Three pre-defined namespaces

- default: if not specified
- kube-system: for internal kubernetes objects
- kube-public: auto redable by all users

Dont use namespaces for versioning ,Just use labels instead

## Labels

- key/value pairs attached to objects
- metadata
- No semantics for kubernetes
- loose coupling via selectors
- same label cant repeat the key

## Volumes

Need for volumes

- Permanence: Storage that lasts longer than lifetime of a pod
- Shared state: Multiple containers in a pod need to share state/files
- volumes: Address both these needs

Important types of volumes

- configMap
  - configMap volume mounts data from ConfigMap object
  - ConfigMap object define key-value pairs
  - usecases:
    - Providing config information for apps running inside pods
    - Specifiying config information for control plane(controllers)
- emptyDir
  - created when pod is creted on node
  - shared space/state across containers in same pod.
  - usecases: Scratch space,checkpointing .
  - when container crashes data remains,when pod removes/crashes,data lost
- gitRepo
  - mount an empty dir and clone a git repo into it.
- secret
  - pass sensitive information to pods
  - secret is creaed and stored in control plane
  - mount that secret as a volume so that it is available inside pod
- hostPath
  - mount file/directory from node filesystem into pod
  - use cases: Access docker internals,running cAdvisor
  - block devices or sockets on host

Volumes(in general) : Lifetime of abstraction = lifetime of pod,this is longer than lifetimeof any container inside pod.

Persistent Volumes: lifetime of abatraction independent of pod lifetime.


## DemonSets

- Usecase includes Monitoring Solution, Logs viewer, kube-proxy & Networking

since v1.12 DemonSet uses NodeAffinity & default schedule

