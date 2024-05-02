# Probes

Kubernetes has two types of probes, readiness and liveness.

Kubernetes uses readiness probes to know when a container is ready to start accepting traffic.

Kubernetes uses liveness probes to know when to restart a container.

The [FCP Helm chart library](https://github.com/DEFRA/ffc-helm-library) includes templates for both readiness and liveness probes.

## Configuring probes

Probes can be configured in the Helm chart on a `Deployment` resource, under the container node.

The above is a simple example of an Http readiness and liveness probes.

```yaml
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

In this example, the cluster will wait for `10` seconds after the pod is deployed.  It will then poll both the liveness and readiness endpoints on port `3000` every `10` seconds.  

If it receives three successive status codes other than `200` for the readiness probe it will stop routing traffic to that pod.

> Note: the readiness probe only prevents traffic routing through the internal cluster network, it does not for example stop a service from consuming messages via Azure Service Bus.

If it receives three successive status codes other than `200` for the liveness probe it will assume the pod is unresponsive and kill it.

> A liveness probe works in conjunction with the `restartPolicy` value. In order to restart the `restartPolicy` must be set to `Always` or `OnFailure`.  The FCP Helm chart default is `Always`.

## Values

`path`: the URL route the liveness probe should sent a response to.

`port`: the port on which the service is exposing

`initialDelaySeconds`: how long before the first probe should be sent. This should be safely longer than it takes the pod to start up, otherwise the pod could be stuck in a reboot loop

`periodSeconds`: how often the liveness probe should check the pod is responsive. Recommendation is between 10 and 20 seconds

`failureThreshold`: how many probe failures before the pod is automatically restarted

`timeoutSeconds`: how long to wait for a response before considering the probe failed

As well as Http probes, there are also command and TCP based probes, full details can be found in the [documentation](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)

## Role in deployments

During deployment from Helm, if a pod does not report healthy within `5` minutes, the deployment will be rolled back automatically and the deployment pipeline will fail.
