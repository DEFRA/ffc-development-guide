# Overview
Pods can have priority. Priority indicates the importance of a Pod relative to other Pods. If a Pod cannot be scheduled, the scheduler tries to preempt (evict) lower priority Pods to make scheduling of the pending Pod possible.

# Priority Levels
High priority: All customer facing services

Medium priority(default): This will be the default option so there is no changes required if 

Low priority: These things will be the first to be shut down if a higher priority pod is required

* For more information click [here](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/)