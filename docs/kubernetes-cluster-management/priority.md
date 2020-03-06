# Overview
Pods can have priority. Priority indicates the importance of a pod relative to other Pods. If a pod cannot be scheduled, the scheduler tries to preempt (evict) lower priority pods to make scheduling of the pending pod possible.

# Priority Levels available to FFC pods
These are the pod priority levels available within an FFC cluster that pods are able to use.

High priority: (1000) These should be reserved primarily for customer facing pods.

Default priority: (600) This will be the default option so a pod only needs to specify a priority class if default is not the correct priority level.

Low priority: (200) These pods will be the first to be shut down if a higher priority pod is required.

# Further reading
More information is available in [Confluence](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/)

# Resoure limits
The [resource limit](resource-usage.md) of a pod will also impact pod priority for those pods with the same priority level.
