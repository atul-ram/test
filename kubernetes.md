

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