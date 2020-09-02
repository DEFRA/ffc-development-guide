# Resource usage
Predictable demands are important to help Kubernetes find the right place for a pod within a cluster. When it comes to resources such as a CPU and memory, understanding the resource needs of a pod will help Kubernetes allocate the pod to the most appropriate node and generally improve the stability of the cluster.

# FFC resource usage standards
- all pods should declare both a `request` and `limit` value for CPU and memory
- production clusters should not contain any `best-effort` pods
- pods with consistent usage patterns should be run as`guaranteed` pods (i.e. equal `request` and `limit values`)
- pods with spiky usage patterns can be run as `burstable` but effort should be taken to understand why performance is not consistent and whether the service is doing too much

# Resource quotas
FFC clusters will limit available resources within a namespace using a `resourceQuota`. This quota can be viewed in the [ffc-kubernetes-configuration](https://github.com/DEFRA/ffc-kubernetes-configuration/tree/master/resource-quotas) repository.

Services which require an exception to this limit should raise a request with the Platform Services team.

# Resource profiling
Performance testing a pod is the only way to understand it's resource utilisation pattern and needs. Performance testing should take place on all pods to accurately understand their usage before they can be deployed to production.

# Further reading
More information is available in [Confluence](https://eaflood.atlassian.net/wiki/spaces/FPS/pages/1616576613/Pod+resource+usage)
