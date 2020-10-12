# Managed Identity

Main branch deployments of microservices to the Kubernetes require a managed identity to provide access to Postgres databases and ServiceBus message queues.

## Create

* Request the creation of an Azure Managed Identity through your usual Cloud Services support channel
* Name of Managed Identity should follow the naming convention: `ffc-<cloud-environment>-<workstream>-<service>-role` e.g. `ffc-snd-demo-web-role`

## Permissions

* Request desired permissions to be granted to your managed identity through your usual Cloud Services support channel
* See [ServiceBus queue configuration](servicebus-queues.md) and [Postgres database configuration](postgres-database.md) for identifying which permissions are required

## Helm Chart

Add Azure Identity and Azure Identity Bindings Kubernetes templates from the [ffc-helm-library](https://github.com/DEFRA/ffc-helm-library) to your microservice helm chart, [following the guidence](https://github.com/DEFRA/ffc-helm-library#azure-identity-template)

## Add Managed Identity values to App Config

Create an entry for the `dev/azureIdentity.clientID` and `dev/azureIdentity.resourceID` keys with the Client ID and Resource ID values for your Manage Identity (found via the Azure Portal), using the label that corresponds to your microservice repository name (e.g. `ffc-demo-web`)
