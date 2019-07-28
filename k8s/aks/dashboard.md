# secure dashboard stuff

https://www.youtube.com/watch?v=od8TnIvuADg

https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/operator-best-practices-identity.md
https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/azure-ad-rbac.md

http://blog.cowger.us/2018/07/03/a-read-only-kubernetes-dashboard.html

kubectl create sa my-dashboard-sa

kubectl create clusterrolebinding my-dashboard-sa --clusterrole=cluster-admin --serviceaccount=default:my-dashboard-sa

kubectl get secrets

kubectl get secret my-dashboard-sa-token-xdcs -o yaml



## responsibility of a namespace admin
https://docs.bitnami.com/kubernetes/how-to/configure-rbac-in-your-kubernetes-cluster/



## https://github.com/Azure/AKS/issues/517

According to k8s dashboard documentation, these are the minimal privileges needed:
https://github.com/kubernetes/dashboard/wiki/Access-control#default-dashboard-privileges

- create permission for secrets in kube-system namespace required to create kubernetes-dashboard-key-holder secret.
- get, update and delete permissions for secrets named kubernetes-dashboard-key-holder and kubernetes-dashboard-certs in kube-system namespace.
- get and update permissions for config map named kubernetes-dashboard-settings in kube-system namespace.
proxy permission to heapster service in kube-system namespace required to allow getting metrics from heapster.

