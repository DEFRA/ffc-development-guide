# Secrets
Where possible, secrets should not be stored within an application or Kubernetes pod.  Instead, clusters should use [AAD Pod Identity](https://github.com/Azure/aad-pod-identity) as per Microsoft recommendation.

When secrets in a pod are unavoidable, for example when a third party API key is needed, they should be injected into the pod via a `Secret` Kubernetes resource by the CI pipeline.  The secret itself should be stored in [Azure Key Vault](https://azure.microsoft.com/en-gb/products/key-vault/) with a reference in [Azure App Configuration](https://azure.microsoft.com/en-gb/services/app-configuration/).
