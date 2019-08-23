https://cdn2.hubspot.net/hubfs/1665891/Assets/Kubernetes%20Security%20-%20Operating%20Kubernetes%20Clusters%20and%20Applications%20Safely.pdf?t=1538587424944&_hsenc=p2ANqtz-_7jbqtRATdJAm7eFxtd5u4nVLYuIlrF67z5qbslZ10-I63-RZ4ogqu9iuuEMsx7fskPYWww2jDpVjOvrLs
a
https://kubernetes-security.info/#authorization

# Security

## kubeconfig

kubectl config --kubeconfig=/root/my-kube-config use-context research

## Roles

kubectl create role developer --verb=get,list,watch --resource=pods --resource=Configmaps --dry-run -o yaml

## Rolebinding

kubectl create rolebinding  developer-binding --role=developer --user=cluster-admin --dry-run  -o yaml

kubectl auth can-i create deployments --
kubectl auth can-i delete nodes

## Cluster Role & Cluster Role Binding

## Security Context
```bash
kubectl create secret docker-registry regcred --docker-server="" --docker-username="" --docker-password="" --docker-email="" 

apiVersion: v1kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: podname
  name: podname
spec:  
securityContext:
  runasUser: 1000
containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: podname
    resources: {}
imagePullsecrets:
-name: regcred 
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
