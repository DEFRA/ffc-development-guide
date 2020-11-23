# Creating a workstream namespace
Each workstream delivery team in FFC will have their own dedicated namespace in each cluster.

This allows logical separation between services as well as the enabling simpler implementation of monitoring, RBAC and stability mechanisms.

## Requirements
The following are the required outcomes of a workstream namespace.#
- namespace follows the naming convention `ffc-WORKSTREAM` eg. `ffc-elm`
- `ResourceQuota` resource to limit resource usage
- `RoleBinding` resource to restrict interaction to delivery team

## Process
Full instructions are included in the [FFC Kubernetes Configuration](https://github.com/DEFRA/ffc-kubernetes-configuration) repository.
