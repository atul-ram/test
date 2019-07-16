
https://github.com/shanepeckham/AKS_Security/blob/master/Azure/AD_RBAC/README.md

https://github.com/kubernetes/cloud-provider-azure/blob/master/docs/cloud-provider-config.md

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
kubectl create clusterrolebinding aad-default-group-cluster-admin-binding \
        --clusterrole=cluster-admin \
        --group=<group-id>

-OR-

kubectl create clusterrolebinding aad-default-cluster-admin-binding \
        --clusterrole=cluster-admin \
        --user <user-id>
```

## Load Balancer

[Load Balancer annotations] (https://github.com/kubernetes/cloud-provider-azure/blob/master/docs/azure-loadbalancer.md#loadbalancer-annotations)


# Get openid-configuration

https://login.microsoftonline.com/tenant-id/.well-known/openid-configuration
