https://kubectl.docs.kubernetes.io/

KUBE_EDITOR="vi"
kubectl config use-context <context>




kubectl --context=k8s14-euw-prod -n kube-system get pods | grep azure-ip-masq-agent


kubectl get nodes
kubectl get pods -o wide

kubectl run nginx --image=nginx --restart=Never --port=80 --expose
# observe that a pod as well as a service are created

kubectl create service clusterip foobar --tcp=80:80 -o json --dry-run


# Examples:
  ## Print the supported API Resources
  kubectl api-resources

  ## Print the supported API Resources with more information
  kubectl api-resources -o wide

  ## Print the supported namespaced resources
  kubectl api-resources --namespaced=true

  ## Print the supported non-namespaced resources
  kubectl api-resources --namespaced=false

  ## Print the supported API Resources with specific APIGroup
  kubectl api-resources --api-group=extensions

How to get role definitions
kubectl get clusterroles cluster-admin -o yaml

kubectl get clusterroles admin -o yaml

## checking, can the service account dummy list pods?

kubectl auth can-i list pods --as=system:serviceaccount:test:dummy --namespace=test

## checking, can the service account dummy create service

kubectl auth can-i create services --as=system:serviceaccount:test:dummy --namespace=test

## get Pods in verbose  

```bash
kubectl -v=9 get pods
```
### Get this pod's YAML without cluster specific information


```bash
kubectl get po nginx -o yaml --export
```

### Get pod logs

```bash
kubectl logs nginx
```

### If pod crashed and restarted, get logs about the previous instance

```bash
kubectl logs nginx -p
```

## Quota

### Get the YAML for a new ResourceQuota called 'myrq' without creating it

```bash
kubectl create quota myrq --hard=cpu=1,memory=1G,pods=2 --dry-run -o yaml
```

# Using curl to discover API resources
