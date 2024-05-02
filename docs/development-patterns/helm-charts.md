# Helm charts

Helm charts allow multiple Kubernetes resource definitions to be deployed and un-deployed as a single unit.

## Source control

- Helm charts are source controlled in the same repository as the microservice they relate to
- Helm charts are saved in a `helm` directory and named the same as the repository. For example, `./helm/ffc-demo-web` directory
- Helm chart versions are automatically updated by CI in line with the application version

## Helm chart library

- to keep Helm charts DRY, the [FCP Helm Chart Library](https://github.com/DEFRA/ffc-helm-library) is used as a base for all resource definitions
- consuming Helm charts only need to define where there is variation from the base Helm chart
- the library helps teams abstract complexity of Kubernetes resource definitions as well as providing a consistent approach to Helm chart development and container security

> Note: For services using ADP, ADP has it's own Helm chart library for both [application](https://github.com/DEFRA/adp-helm-library) and [infrastructure](https://github.com/DEFRA/adp-aso-helm-library).

## Helm chart repository

- Helm charts are published by CI to a Helm chart repository within [Azure Container Registry](https://azure.microsoft.com/en-us/products/container-registry/)
- Helm charts are only published on a main branch build
- Helm charts are automatically versioned by CI pipeline to be consistent with the application version
- Helm charts for in flight Pull Requests are not published

## Labelling resources

Labels can be added to Kubernetes resources to categorise and query objects.

In order to take full advantage of using labels, they should be applied on every resource object within a Helm chart. i.e. all deployments, services, ingresses etc.

When using the [FCP Helm Chart Library](https://github.com/DEFRA/ffc-helm-library), these labels are automatically applied to all resource objects.

### Required labels
Each Helm chart templated resource should have the below labels. Example placeholder references to the `values.yaml` file are provided.

```go
metadata:
  labels:
    app: {{ quote .Values.namespace }}
    app.kubernetes.io/name: {{ quote .Values.name }}
    app.kubernetes.io/instance: {{ .Release.Name }}
    app.kubernetes.io/version: {{ quote .Values.labels.version }}
    app.kubernetes.io/component: {{ quote .Values.labels.component }}
    app.kubernetes.io/part-of: {{ quote .Values.namespace }}
    app.kubernetes.io/managed-by: {{ .Release.Service }}
    helm.sh/chart: {{ .Chart.Name }}-{{ .Chart.Version | replace "+" "_" }}
    environment: {{ quote .Values.environment }}
```

> Note: `Deployment` resource objects should have two sets of labels, one for the actual deployment and another for the pod template the deployment manages.

## Selectors

Services selectors should be matched by app and name. Selectors should be consistent once set, otherwise updates to Helm charts will be rejected.
```go
selector:
  app: {{ quote .Values.name }}
  app.kubernetes.io/name: {{ quote .Values.name }}
```

## Resources

Predictable demands are important to help Kubernetes find the right place for a pod within a cluster. When it comes to resources such as a CPU and memory, understanding the resource needs of a pod will help Kubernetes allocate the pod to the most appropriate Node and generally improve the stability of the cluster.

### Declaring a profile

- all pods declare both a `request` and `limit` value for CPU and memory
- pods with consistent usage patterns are run as`guaranteed` ie equal `request` and `limit` values
- pods with spiky usage patterns can be run as `burstable` ie `request` lower than `limit` values
- pods without `limit` values or `request` values are run as `best-effort`.  These are not used in FCP as they can cause instability in the cluster


### Resource quotas

Clusters will limit available resources within a service's namespace using a [`ResourceQuota`](https://kubernetes.io/docs/concepts/policy/resource-quotas/).

Teams should frequently review the resource usage of their pods and adjust the `ResourceQuota` accordingly.

> The resource quota should cover all pods running at their limit values at full level of replicas.

The quota is deployed by the FCP Platform's [`platform-aks-configuration`](https://dev.azure.com/defragovuk/DEFRA-FFC/_build?definitionId=2776) pipeline.

### Resource profiling

Performance testing an end to end service is the only way to fully understand required resource needs.  Teams should ensure a suitable performance test strategy is in place.

## Priority classes

FCP Platform has three [priority classes](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/). 

Priority indicates the importance of a pod relative to other pods. If a pod cannot be scheduled, the scheduler tries to pre-empt (evict) lower priority pods to make scheduling of the pending pod possible.

In the event of over utilisation of a cluster, Kubernetes will start to kill lower priority pods first to maintain stability.

### `high` (1000) 
Reserved primarily for customer facing or critical workload pods.

### `default` (600)
Default option suitable for most pods.

### `low` (200)
For pods where downtime is more tolerable.

When using the [FCP Helm Chart Library](https://github.com/DEFRA/ffc-helm-library), the `Default` priority class is automatically applied to all pods.  This value can be overwritten by adding the below to the `values.yaml` file.

```go
deployment:
  priorityClassName: high
```

### Resource profile impact
In the event a cluster has to make a choice between killing one of two services sharing the same priority level, the resource profile configuration will influence which is killed in the order below.
  1. Best effort
  2. Burstable
  3. Guaranteed
