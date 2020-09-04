# Interact with cluster
`kubectl` is used to interact with the cluster.  In order to use `kubectl` with an FFC cluster, a `kubeconfig` file for the cluster is required.

A cluster can only be accessed when connected to VPN.

### Acquiring a Kubeconfig file for a cluster
To acquire a Kubeconfig, a subscription Id is needed along with the name of the cluster and the resource group in which it resides.  This information can be acquired via the Azure Portal or from CSC.

`az account set --subscription SUBSCRIPTION_ID`

`az aks get-credentials --resource-group RESOURCE_GROUP --name CLUSTER --file WHERE_TO_SAVE_KUBECONFIG`

**Note** if the file parameter is not passed, the Kubeconfig will be merged with the users default configuration file stored at `~/.kube/config`.

### Productivity
Developers may find it more productive to use tools such as [k9s](https://github.com/derailed/k9s) or [kubectl-aliases](https://github.com/ahmetb/kubectl-aliases) to avoid needing to regularly type long terminal commands to interact with the cluster.
