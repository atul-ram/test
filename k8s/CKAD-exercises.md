http://kubernetesbyexample.com/

https://github.com/dgkanatsios/CKAD-exercises

https://kubectl.docs.kubernetes.io/



## Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

```sh
kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml
```

## Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

```sh
kubectl create deployment --image=nginx nginx --dry-run -o yaml
```

## Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)

```sh
kubectl run --generator=deployment/v1beta1 nginx --image=nginx --dry-run --replicas=4 -o yaml
```

# Service

## Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

```sh
kubectl expose pod redis --port=6379 --name redis-service --dry-run -o yaml
```

## Save it to a file - (If you need to modify or add some other details)

```sh
kubectl run --generator=deployment/v1beta1 nginx --image=nginx --dry-run --replicas=4 -o yaml > nginx-deployment.yaml
```