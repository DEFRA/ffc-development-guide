# Overview

Kubernetes has two types of probes, readiness probes and liveness probes.

Kubernetes uses readiness probes to know when a container is ready to start accepting traffic.

Kubernetes uses liveness probes to know when to restart a container.

# Configuring probes

Probes can be configured in the Helm chart on either a Pod or Deployment yaml file under the container node.

```
readinessProbe:
  path: /healthy
  port: 3000
  initialDelaySeconds: 10
  periodSeconds: 10
  failureThreshold: 3

livenessProbe:
  path: /healthz
  port: 3000
  initialDelaySeconds: 10
  periodSeconds: 10
  failureThreshold: 3
```

The above is an example of simple Http readiness and liveness probes.

Kubernetes will periodically call the /healthy and /healthz routes.

If it receives a status code other than 200 for the readiness probe it will stop routing traffic to that pod.

If it receives a status code other than 200 for the liveness probe it will assume the pod is unresponsive and restart it.

**Note** that a liveness probe works in conjunction with the restartPolicy value. In order to restart the restartPolicy must be set to Always or OnFailure.

## Values

**path**: the URL route the liveness probe should sent a response to.

**port**: the port on which the service is exposing

**initialDelaySeconds**: how long before the first probe should be sent. This should be safely longer than it takes the pod to start up, otherwise the pod could be stuck in a reboot loop

**periodSeconds**: how often the liveness probe should check the pod is responsive. Recommendation is between 10 and 20 seconds

**failureThreshod**: how many probe failures before the pod is automatically restarted

As well as http probes, there are also command and TCP based probes, full details can be found in the documentation https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/

# Standards

To increase the stability of a Kubernetes cluster, services should make use of both readiness and liveness probes unless there is a significant reason not to.

Probe end points should follow the convention of `healthy` for readiness probes and `healthz` for liveness probes.
