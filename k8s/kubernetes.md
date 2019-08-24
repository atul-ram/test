TLS bootstrapping a node 


https://labs.play-with-k8s.com/

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



#ref 
https://medium.com/platformer-blog/how-i-passed-the-cka-certified-kubernetes-administrator-exam-8943aa24d71d

https://docs.google.com/spreadsheets/d/10NltoF_6y3mBwUzQ4bcQLQfCE1BWSgUDcJXy-Qp2JEU/edit#gid=0

https://github.com/walidshaari/Kubernetes-Certified-Administrator
https://github.com/twajr/ckad-prep-notes#where-to-practice

https://github.com/kelseyhightower/kubernetes-the-hard-way


[Self assesment](https://docs.google.com/spreadsheets/d/1kyPaDFQyHt8lm-rFEm2Xz89oqM3HI0A2wvzvvbGYB2c/edit#gid=0)

# Where to Practice

[play with k8s](https://labs.play-with-k8s.com/#)

or
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


[What does “production ready” really mean for a Kubernetes cluster?](https://speakerdeck.com/luxas/what-does-production-ready-really-mean-for-a-kubernetes-cluster-umea-may-2019) by Lucas Käldström (May 7, 2019)

# Production ready Cluster
  
  1. The cluster is resonably secure.
  2. The cluster components are highly available enough for user's needs
  3. All elements in the cluster are declaratively cotrolled.
  4. Changes to the cluster state can be safely applied (upgrades/rollbacks)
  5. The cluster passes as many end-to-end tests as possible.


# Master to Cluster

## apiserver to kubelet

   The connections from the apiserver to the kubelet are used for:

- Fetching logs for pods.
- Attaching (through kubectl) to running pods.
- Providing the kubelet’s port-forwarding functionality.

## apiserver to nodes, pods, and services

Kubernetes supports SSH tunnels to protect the Master -> Cluster communication paths.

