# Secrets
Where possible, secrets should not be stored within an application or Kubernetes pod.  Instead, clusters should use [AAD Pod Identity](https://github.com/Azure/aad-pod-identity) as per Microsoft recommendation.

When secrets in a pod are unavoidable, for example when a third party API key is needed, secrets should be directly injected from secure vault at deployment.
This is a more secure method of accessing the secrets than storing them in base64 encoded Kubernetes secrets and has been agreed at CTWG.

**Note** An example in the demo service is coming soon.