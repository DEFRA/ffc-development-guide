# Helm chart
Helm charts allow multiple Kubenetes resource definitions to be deployed and undeployed as a single unit.

Helm version 3 is used within FFC.  

Helm 2 should never be used due to security risk introduced by Tiller in a cluster.

## Source control
- Helm charts are source controlled in the same repository as the microservice they relate to

- Helm charts are saved in a `helm` directory and named the same as the repository. For example a chart referencing the ELM Payment Service would be stored in the `./helm/elm-payment-service` directory

- Helm chart versions are automatically updated by CI in line with the application version

## Helm chart library
- to keep Helm charts DRY, the [FFC Helm Chart Library](https://github.com/DEFRA/ffc-helm-library) is used as a base for all resource definitions

- consuming Helm charts only need to define where there is variation from the base Helm chart

## Helm chart repository
- Helm charts are published to a Helm chart repository using Azure Container Registry

- Helm charts for in flight Pull Requests are not published
