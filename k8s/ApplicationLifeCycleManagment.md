


## Create a deployment 

kubectl create -f deployment-definition.yaml

## Update a deployment

kubectl apply -f deployment-definition.yaml

kubectl describe deployment myapp-deployment

## Status

kubectl rollout status deployment/myapp-deployment

## Deployment Strategy

- Recreate
- Rolling Update

## Rollback

```bash
kubectl rollout undo deployment/my-app-deployment

```



$ kubectl rollout pause deployment/ghost
$ kubectl rollout resume deployment/ghost

kubectl rollout history deployment/ghost

kubectl scale deployment/nginx ‐‐replicas=4