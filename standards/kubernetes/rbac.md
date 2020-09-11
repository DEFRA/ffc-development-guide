# Role Based Access Control (RBAC)
FFC clusters enable RBAC through `RoleBinding` Kubernetes resources.

Each delivery team will have it's own Rolebinding so permissions can be permitted or denied on a team level.

## Development cluster
- Platform team developers have Cluster Admin role allowing them full access to the cluster

- Delivery team developers are restricted to only namespaces they own
