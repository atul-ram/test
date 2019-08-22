
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

#2

## Take a backup of the etcd cluster and save it to /tmp/etcd-backup.db

## Create a Pod called redis-storage with image: redis:alpine with a Volume of type emptyDir that lasts for the life of the Pod. Specs on the right

## Create a new pod called super-user-pod with image busybox:1.28. Allow the pod to be able to set system_time

## A pod definition file is created at /root/use-pv.yaml. Make use of this manifest file and mount the persistent volume called pv-1. Ensure the pod is running and the PV is bound.

## Create a new deployment called nginx-deploy, with image nginx:1.16 and 1 replica. Record the version. Next upgrade the deployment to version 1.17 using rolling update. Make sure that the version upgrade is recorded in the resource annotation.

## Create a new user called john. Grant him access to the cluster. John should have permission to create, list, get, update and delete pods in the development namespace . The private key exists in the location: /root/john.key and csr at /root/john.csr

Generate a certificateSigningRequest for John and get it approved. Create the correct RBAC configuration for the user.

## Create an nginx pod called nginx-resolver using image nginx, expose it internally with a service called nginx-resolver-service. Test that you are able to look up the service and pod names from within the cluster. Record results in /root/nginx.svc and /root/nginx.pod

## Create a static pod on node01 called nginx-critical with image nginx. Create this pod on node01 and make sure that it is recreated/restarted automatically in case of a failure.

Add --pod-manifest-path to kubelet service on worker or staticPodPath in the kubelet config.yaml

kubectl get nodes -o json | jq .items[].status.nodeInfo

kubectl get nodes -o jsonpath=".items[].status.nodeInfo"

kubectl get pods -o=jsonpath='{.items[0].spec.securityContext}'

kubectl get pod ubuntu-sleeper  -o json | jq '.spec.securityContext'