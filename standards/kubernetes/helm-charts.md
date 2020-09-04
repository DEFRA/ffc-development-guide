## Helm chart
Helm charts should be saved in a directory which matches the name of the chart. For example a chart referencing the ELM Payment Service would be stored in the `elm-payment-service` directory.

Helm chart versions should be updated in the `Chart.yaml` file's `version` property.

As Helm charts are saved in the same repository as the application they manage, the version numbers should be updated in sync with the application.

Helm charts for PR deployments should not be pushed to a Helm repository.
