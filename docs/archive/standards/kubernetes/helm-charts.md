# Helm chart
Helm charts allow multiple Kubernetes resource definitions to be deployed and un-deployed as a single unit.

Helm version 3 is used within FFC.  Currently FFC is limited to version `3.6` due to breaking changes introduced in version [`3.7`](https://github.com/helm/helm/releases/tag/v3.7.0).

Helm 2 should never be used due to security risk introduced by Tiller in a cluster.

## Source control
- Helm charts are source controlled in the same repository as the microservice they relate to

- Helm charts are saved in a `helm` directory and named the same as the repository. For example a chart referencing the ELM Payment Service would be stored in the `./helm/elm-payment-service` directory

- Helm chart versions are automatically updated by CI in line with the application version

## Helm chart library
- to keep Helm charts DRY, the [FFC Helm Chart Library](https://github.com/DEFRA/ffc-helm-library) is used as a base for all resource definitions

- consuming Helm charts only need to define where there is variation from the base Helm chart

- the library helps teams abstract complexity of Kubernetes resource definitions as well as providing a consistent approach to Helm chart development and container security

## Helm chart repository
- Helm charts are published by CI to a Helm chart repository within [Azure Container Registry](https://azure.microsoft.com/en-us/products/container-registry/)

- Helm charts are only published on a main branch build

- Helm charts are automatically versioned by CI pipeline to be consistent with the application version

- Helm charts for in flight Pull Requests are not published
