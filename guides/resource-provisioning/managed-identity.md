# Managed Identity

Main branch deployments of microservices to the FFC Platform Kubernetes cluster require an Azure Managed Identity to be bound to a Pod to provide access to additional Azure resources such as [Postgres databases](postgres-database.md) and [Service Bus message queues](servicebus-queues.md).

## Create a Managed Identity

Request the creation of an Azure Managed Identity through your usual Cloud Services support channel.

The name of the Managed Identity should adhere to the following naming convention: `ffc-<cloud-environment>-<workstream>-<service>-role`, e.g. `ffc-snd-demo-web-role`.

## Add Managed Identity Permissions

Permissions will need to be associated to the Managed Identity that determine which Azure resources it can access.

This step should be carried out when configuring these additional resources. See relevant sections in [ServiceBus queue configuration](servicebus-queues.md) and [Postgres database configuration](postgres-database.md).

## Update Microservice Helm Chart

In your microservice codebase, add the `AzureIdentity` and `AzureIdentityBinding` Kubernetes templates from the [ffc-helm-library](https://github.com/DEFRA/ffc-helm-library) to your microservice Helm Chart.

Create these templates in the Helm chart directory of your microserive (`helm/<REPO_NAME>/templates/`) [following the FFC Helm Library guidence](https://github.com/DEFRA/ffc-helm-library#azure-identity-template).

## Add Managed Identity values to Azure App Configuration

Azure App Configuration stores values required by the Jenkins CI pipelines.

For your newly created Managed Identity, create two entries:

* **Key**: `dev/azureIdentity.clientID`; **Value**: Client ID of your Managed Identity (found via the Azure Portal); **Label**: `<REPO_NAME>` (e.g. `ffc-demo-web`)
* **Key**: `dev/azureIdentity.resourceID`; **Value**: Resource ID of your Manage Identity (found via the Azure Portal); **Label**: `<REPO_NAME>` (e.g. `ffc-demo-web`)
