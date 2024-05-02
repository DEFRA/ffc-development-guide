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

> Note: For services using ADP, ADPs has it's own Helm chart library for both [application](https://github.com/DEFRA/adp-helm-library) and [infrastructure](https://github.com/DEFRA/adp-aso-helm-library).

## Helm chart repository

- Helm charts are published by CI to a Helm chart repository within [Azure Container Registry](https://azure.microsoft.com/en-us/products/container-registry/)

- Helm charts are only published on a main branch build

- Helm charts are automatically versioned by CI pipeline to be consistent with the application version

- Helm charts for in flight Pull Requests are not published
