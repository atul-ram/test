# Networking

## Pod Network

- Cluster-wide network used for pod-to-pod communication managed by a CNI (Container Network Interface) plugin.

## Service Network

- Cluster-wide range of Virtual IPs managed by kube-proxy for service discovery.

### Container Network Interface

- Pod networking within Kubernetes is plumbed via the Container Network Interface (CNI).
- Functions as an interface between the container runtime and a network implementation plugin.

## Fundamentals

### Container to Container

- Containers within a pod exist within the same network namespace and share an IP.
- Enables intrapod communication over localhost.  

### Pod to Pod

- Allocated cluster unique IP for the duration of its life cycle.
- Pods themselves are fundamentally ephemeral. 

### Pod to Service
managed by **kube-proxy** and given a persistent cluster unique IP
exists beyond a Pod’s lifecycle.

### External to Service

- Handled by kube-proxy. 
- Works in cooperation with a cloud provider or other external entity (load balancer). 


