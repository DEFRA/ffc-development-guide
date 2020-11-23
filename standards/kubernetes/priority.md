# Pod priority
Kubernetes Pods can have priority levels. Priority indicates the importance of a pod relative to other pods. If a pod cannot be scheduled, the scheduler tries to preempt (evict) lower priority pods to make scheduling of the pending pod possible.

In the event of over utilisation of a cluster, Kubernetes will start to kill lower priorty pods first to maintain stability.

## Priority levels available to FFC pods
There are three defined pod priority levels available within an FFC cluster.  A deployment must specify one of these levels.

### High (1000) 
Reserved primarily for customer facing or critical workload pods.

### Default (600)
Default option suitable for most pods.

### Low (200)
For pods where downtime is more tolerable.

The `priorityClass` definitions that relate to these levels can be viewed in the [ffc-kubernetes-configuration](https://github.com/DEFRA/ffc-kubernetes-configuration/tree/master/priority-classes) repository.

## Resource profile impact
In the event a cluster has to make a choice between killing one of two services sharing the same priority level, the [resource profile](resource-usage.md) configuration will influence which is killed.

## Further reading
More information is available in [Confluence](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/)
