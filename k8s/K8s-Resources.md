https://github.com/krzko/awesome-cka
https://github.com/twajr/ckad-prep-notes


[Kubernetes The Hard Way ](https://github.com/mmumshad/kubernetes-the-hard-way)

https://www.youtube.com/watch?v=WFGPArjYMbQ

https://interactive.linuxacademy.com/diagrams/ThePodofMinerva.html

# How to shutdown and restart kubernetes cluster

take a backup of the K8s in case things go south when you try to bring the cluster online (I will use Heptio-Velero for that.)

- on the master node stop the following services:
  - kupe-apiserver
  - kube-scheduler
  - kube-controllers
- on the nodes stop the following services:
  - kubelet
  - kube-proxy

# Etcd backup 



