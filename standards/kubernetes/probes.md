# Probes
To increase the stability and predicatability of a Kubernetes cluster, services should make use of both readiness and liveness probes unless there is a significant reason not to.

Probe end points should follow the convention of `healthy` for readiness probes and `healthz` for liveness probes.
