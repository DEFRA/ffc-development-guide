# Secrets
Where possible, secrets should not be stored within an application or Kubernetes pod.  Instead, clusters should use [AAD Pod Identity](https://github.com/Azure/aad-pod-identity) as per Microsoft recommendation.

When secrets in a pod are unavoidable, for example when a third party API key is needed, secrets should be stored in Azure Key Vault and injected into Kubernetes `Secret` resources via CI.

**Note** The approach for unavoidable secrets is under review and it is likely that the programme will adopt a tool such as [Kubernetes Secrets Store CSI Driver](https://github.com/kubernetes-sigs/secrets-store-csi-driver).
