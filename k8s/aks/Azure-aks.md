## kubenetes the hard way on azure

https://github.com/ivanfioravanti/kubernetes-the-hard-way-on-azure

## Latest AKS change log

https://github.com/Azure/AKS/blob/master/CHANGELOG.md

- [Preview - Limit egress traffic for cluster nodes and control access to required ports and services in Azure Kubernetes Service](https://docs.microsoft.com/en-us/azure/aks/limit-egress-traffic#required-ports-and-addresses-for-aks-clusters)

## AKS team Dashboard

https://github.com/Azure/AKS/projects/1


https://github.com/shanepeckham/AKS_Security/blob/master/Azure/AD_RBAC/README.md

https://github.com/kubernetes/cloud-provider-azure/blob/master/docs/cloud-provider-config.md
https://cloudmanagement.navisite.com/azure-kubernetes-service-aks-network-design/

https://docs.microsoft.com/en-us/azure/governance/policy/concepts/rego-for-aks

## set of improvment points, since last visit

https://github.com/Azure/kubernetes-keyvault-flexvol
https://docs.microsoft.com/en-us/azure/aks/view-master-logs TBD

# Common commands

```sh
az aks list -o table
```

- az aks delete

```sh
az aks delete --name myAKSCluster --resource-group MyResourceGroup
```

- az aks create

```sh
sh -x latest-aad-aks.sh
```

- steps to configure

```sh

az aks get-credentials --name myAKSCluster --resource-group MyResourceGroup --overwrite-existing

az aks get-credentials --name myAKSCluster --resource-group MyResourceGroup --admin --overwrite-existing

az aks browse --name myAKSCluster --resource-group MyResourceGroup

kubectl create clusterrolebinding aad-default-group-cluster-admin-binding --clusterrole=cluster-admin --group=ae1c4a4d-985c-4bd6-b003-1e8bc8d1be28

```

# Limit access to Cluster Admin

## Cluster Admin

```sh
# Get the resource ID of your AKS cluster
AKS_CLUSTER=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query id -o tsv)

# Get the account credentials for the logged in user
# ACCOUNT_UPN=$(az account show --query user.name -o tsv)
GROUP_NAME="ClusterAdmin"
GROUP_ID=$(az ad group show --group $GROUP_NAME --query objectId -o tsv)

# Assign the 'Cluster Admin' role to the user
az role assignment create \
    --assignee $GROUP_ID \
    --scope $AKS_CLUSTER \
    --role "Azure Kubernetes Service Cluster Admin Role"

```

## Cluster User Role

```sh
# Get the resource ID of your AKS cluster
AKS_CLUSTER=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query id -o tsv)

# Get the account credentials for the logged in user
# ACCOUNT_UPN=$(az account show --query user.name -o tsv)
GROUP_NAME="ClusterUser"
GROUP_ID=$(az ad group show --group $GROUP_NAME --query objectId -o tsv)

# Assign the 'Cluster Admin' role to the user
az role assignment create \
    --assignee $GROUP_ID \
    --scope $AKS_CLUSTER \
    --role "Azure Kubernetes Service Cluster User Role"

```

- Enable access to dashboard--without error

```sh

kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

## Enabling AAD groups

You can also optionally add groups into your admin role

```sh

# Get the account credentials for the logged in user
# ACCOUNT_UPN=$(az account show --query user.name -o tsv)
GROUP_NAME="ClusterAdmin"
GROUP_ID=$(az ad group show --group $GROUP_NAME --query objectId -o tsv)

kubectl create clusterrolebinding aad-default-group-cluster-admin-binding \
        --clusterrole=cluster-admin \
        --group=$GROUP_ID

-OR-

kubectl create clusterrolebinding aad-default-cluster-admin-binding \
        --clusterrole=cluster-admin \
        --user <user-id>
```

## Load Balancer

[Load Balancer annotations] (https://github.com/kubernetes/cloud-provider-azure/blob/master/docs/azure-loadbalancer.md#loadbalancer-annotations)


# Get openid-configuration

https://login.microsoftonline.com/tenant-id/.well-known/openid-configuration


# we can create standard load balancer (preview) 

https://docs.microsoft.com/en-us/azure/aks/load-balancer-standard


# List DaemonSets 

```bash
kubectl get DaemonSet --all-namespaces
NAMESPACE     NAME                       DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
kube-system   azure-cni-networkmonitor   3         3         3       3            3           beta.kubernetes.io/os=linux   54m
kube-system   azure-ip-masq-agent        3         3         3       3            3           beta.kubernetes.io/os=linux   54m
kube-system   kube-proxy                 3         3         3       3            3           beta.kubernetes.io/os=linux   54m
kube-system   omsagent                   3         3         3       3            3           beta.kubernetes.io/os=linux   54m
```
