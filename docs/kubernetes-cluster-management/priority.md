# Overview
Pods can have priority. Priority indicates the importance of a Pod relative to other Pods. If a Pod cannot be scheduled, the scheduler tries to preempt (evict) lower priority Pods to make scheduling of the pending Pod possible.

# Priority Levels
High priority: (1000)

Medium priority(default): (600)This will be the default option so priority changes are only necessary if default is not the correct priority level.

Low priority: (200) These services will be the first to be shut down if a higher priority pod is required.

* For more information click [here](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/)