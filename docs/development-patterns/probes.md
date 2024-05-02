# Probes

To increase the stability and predictability of a Kubernetes cluster, services should make use of both readiness and liveness probes to help Kubernetes manage the stability of the service.

Probe end points should follow the convention of `healthy` for readiness probes and `healthz` for liveness probes.

If no probe is defined, Kubernetes will still attempt to monitor the health of a service.  However, it will only be able to do so by monitoring the status of the container.  This is not a reliable way to determine the health of a service.

## Readiness probes

Readiness probes help Kubernetes manage the availability of a service.  They are used to determine if a service is ready to receive traffic.  If a service is not ready, Kubernetes will not route traffic to it.

Helm using the readiness probe to determine if a service is successfully deployed.  If a service does not report ready in five minutes, the deployment will be considered a failure and will be automatically rolled back.

> Note: the readiness probe only prevents traffic routing through the internal cluster network, it does not for example stop a service from consuming messages via Azure Service Bus.

## Liveness probes

Liveness probes help Kubernetes manage the stability of a service.  They are used to determine if a service is still running.  If a service is not running, Kubernetes will restart it.

## Probe configuration

Probe configuration is subjective to a service and consideration should be given to the following:

- the initial delay before probes are started
- the frequency of the probe
- how many sequential failures are required before a service is considered unhealthy
- the timeout the probe should wait for a response
- what should the probe check for to determine the health of the service
