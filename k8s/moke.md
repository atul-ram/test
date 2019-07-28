
# Mock

## Deploy a pod named nginx-pod using the nginx:alpine image.

 kubectl run --generator=run-pod/v1 nginx-pod --image=nginx:alpine

## Deploy a messaging pod using the redis:alpine image with the labels set to tier=msg.

kubectl run --generator=run-pod/v1 messaging --image=redis:alpine -l tier=msg

## Create a service messaging-service to expose the messaging application within the cluster on port 6379.

kubectl expose pod messaging --port=6379 --name messaging-service

## Create a static pod named static-busybox that uses the busybox image and the command sleep 1000

Create a pod definition file in the manifests folder. Use command 

```bash 
kubectl run --restart=Never --image=busybox static-busybox --dry-run -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
```

## Create a POD in the finance namespace named temp-bus with the image redis:alpine.

Use the command 

```bash
#Run the command
kubectl run temp-bus --image=redis:alpine --namespace=finance --restart=Never
```

## Expose the hr-web-app as service hr-web-app-service application on port 30082 on the nodes on the cluster

```bash
#Run the command
kubectl expose deployment hr-web-app --type=NodePort --port=8080 --name=hr-web-app-service --dry-run -o yaml > hr-web-app-service.yaml to generate a service definition file. Then edit the nodeport in it and create a service.
```

kubectl get nodes -o json | jq .items[].status.nodeInfo

kubectl get nodes -o jsonpath=".items[].status.nodeInfo"