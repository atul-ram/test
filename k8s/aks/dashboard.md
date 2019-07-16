# secure dashboard stuff

https://www.youtube.com/watch?v=od8TnIvuADg

https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/operator-best-practices-identity.md
https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/azure-ad-rbac.md

kubectl create sa my-dashboard-sa

kubectl create clusterrolebinding my-dashboard-sa --clusterrole=cluster-admin --serviceaccount=default:my-dashboard-sa

kubectl get secrets

kubectl get secret my-dashboard-sa-token-xdcs -o yaml

