kubectl config use-context <context>

kubectl get nodes
kubectl get pods -o wide




Examples:
  # Print the supported API Resources
  kubectl api-resources

  # Print the supported API Resources with more information
  kubectl api-resources -o wide

  # Print the supported namespaced resources
  kubectl api-resources --namespaced=true

  # Print the supported non-namespaced resources
  kubectl api-resources --namespaced=false

  # Print the supported API Resources with specific APIGroup
  kubectl api-resources --api-group=extensions

How to get role definitions
kubectl get clusterroles cluster-admin -o yaml

kubectl get clusterroles admin -o yaml