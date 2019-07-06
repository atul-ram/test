

Get all Nodes in your cluster.

a. Get by Names
nodes=$(kubectl get nodes -o jsonpath='{range.items[*].metadata}{.name} {end}')


b.Get by IPs
nodes=$(kubectl get nodes -o jsonpath='{range .items[*].status.addresses[?(@.type=="ExternalIP")]}{.address} {end}')

Copy the docker config to each node

for n in $nodes; do 
 scp ~/.docker/config.json root@$n:/var/lib/kubelet/config.json;
 done


kubectl create -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: private-image-testing
spec:
  containers:
    - name: private-image-test
      image: $YOUR_PRIVATE_IMAGE_NAME
      imagePullPolicy: Always
      command: [ "echo", "SUCCESS" ]
EOF

kubectl create secret docker-registry myregistrysecret --docker-server=DOCKER_REGISTRY_SERVER_URL --docker-username=DOCKER_USER_NAME --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL_ID




The Smallest deployable entity in kubernetes is known as a Pod.

Pods run inside nodes, and containers run inside the pods.

Controllers( Deployment, StatefulSets, DaemonSet) create pods.


A Pod may run a single container or multiple containers inside it.

Containers that run in a same pod share network and storage.

Inside a Pod, containers can address other containers with localhost.

Since containers also share storage, they can even communicate using Inter Process Communication (IPC) or Semaphores.


## Pod's Life
This is how a Pod's life goes.

Pods are created.
- Pods are assigned a Unique ID(UID).
- Scheduled to a random Node.
- They run and server requests.
- Wait for graceful termination (according to restart policy).
- If a node dies before that pods are scheduled for recreation, a new pod is created with new UID. The old pod's state is not taken into consideration.


#ref 
https://medium.com/platformer-blog/how-i-passed-the-cka-certified-kubernetes-administrator-exam-8943aa24d71d

https://docs.google.com/spreadsheets/d/10NltoF_6y3mBwUzQ4bcQLQfCE1BWSgUDcJXy-Qp2JEU/edit#gid=0

https://github.com/walidshaari/Kubernetes-Certified-Administrator
https://github.com/twajr/ckad-prep-notes#where-to-practice


# Current Progress

- [ ] __Core Concepts - 13%__
  - [ ] API Primitives
  - [ ] Create and Configure Basic Pods
- [ ] __Configuration - 18%__
  - [ ] Understand ConfigMaps
  - [ ] Understand SecurityContexts
  - [ ] Define App Resource Requirements
  - [ ] Create and Consume Secrets
  - [ ] Understand Service Accounts
- [ ] __Multi-Container Pods - 10%__
  - [ ] Design Patterns: Ambassador, Adapter, Sidecar
    - [ ] - Sidecar Pattern
    - [ ] - Init Containers
- [ ] __Pod Design - 20%__
  - [ ] Using Labels, Selectors, and Annotations
  - [ ] Understand Deployments and Rolling Updates
  - [ ] Understand Deployment Rollbacks
  - [ ] Understand Jobs and CronJobs
- [ ] - __State Persistence - 8%__
  - [ ] - Understand PVCs for Storage
- [ ] __Observability - 18%__
  - [] Liveness and Readiness Probes
  - [ ] Understand Container Logging
  - [ ] Understand Monitoring Application in Kubernetes
  - [ ] Understand Debugging in Kubernetes
- [ ] __Services and Networking - 13%__
  - [ ] Understand Services
  - [ ] Basic Network Policies

# Where to Practice
In my opinion, and all that is required to pass this test, is to just setup a gcloud account, and use a two-node GKE cluster for studying. Heck, you can even use the very nice google cloud shell and not even leave your browser.

[gcloud command line (SDK) documentation](https://cloud.google.com/sdk/)

Here are commands used to create a two-node cluster for studying. I keep these here just so I can fire up and destroy a cluster for a few hours each day for study. Notice that you can tailor the cluster version to match the k8s version for the exam.
```
gcloud config set compute/zone us-central1-a
gcloud config set compute/region us-central1
gcloud container clusters create my-cluster --cluster-version=1.11.7-gke.12 \
     --image-type=ubuntu --num-nodes=2
```
The result:
```
NAME        LOCATION       MASTER_VERSION  MASTER_IP     MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
my-cluster  us-central1-a  1.10.7-gke.6    35.232.253.6  n1-standard-1  1.11.7-gke.12  2          RUNNING

cloudshell:~$ kubectl get nodes
NAME                                        STATUS    ROLES     AGE       VERSION
gke-my-cluster-default-pool-5f731fab-9d6n   Ready     <none>    44s       1.11.7-gke.12
gke-my-cluster-default-pool-5f731fab-llrb   Ready     <none>    41s       1.11.7-gke.12
```
## Setting kubectl Credentials
If using the cloud shell, you'll sometimes need to authorize kubectl to connect to your cluster instance.
```
gcloud container clusters get-credentials my-cluster
```
## Deleting Your Cluster
No need to keep the cluster around when not studying, so:
```
gcloud container clusters delete my-cluster
```
## To Get Current GKE Kubernetes Versions
```
  gcloud container get-server-config
```
