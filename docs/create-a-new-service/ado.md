# Setup an Azure DevOps CD pipeline

Common Azure DevOps (ADO) pipeline definitions [have been created](https://dev.azure.com/defragovuk/DEFRA-FFC/_build) and can be reused to deploy services to the various environments.

These pipelines are responsible for not only deploying applications, but for provisioning and configuring the Azure environments the service run in.

## Pipeline categories

Pipelines are split into different capabilities which must be run sequentially into an environment when changes are made.

### [`platform-core`](https://dev.azure.com/defragovuk/DEFRA-FFC/_build?definitionId=2751)

Deploy the core platform services such as PaaS services and networking.  This pipeline rarely needs to be run and should only be done via the Platform team.

### [`platform-aks-configuration`](https://dev.azure.com/defragovuk/DEFRA-FFC/_build?definitionId=2776)

Deploy configuration for the AKS.  This pipeline should be run when a new namespace is added to support a new bounded context.

### [`platform-services`](https://dev.azure.com/defragovuk/DEFRA-FFC/_build?definitionId=2752)

Deploy updates to the environment for a specific resource types including:

- Managed Identities
- Azure Service Bus
- Azure Storage

To select the service definition, when running the pipeline select the `Service` and `Resource Type`.

The pipeline is also capable deploying all databases and helm charts for a bounded context, but should not be used for that purpose.  Instead the Helm and database pipelines should be used as they allow deploying individual services by semantic version.

### [`platform-services-database`](https://dev.azure.com/defragovuk/DEFRA-FFC/_build?definitionId=3011)

Deploy a database migration for a specific application version.  This pipeline should be run when a new database migration is required selecting `service`, `databaseRepo` and `version`.

### [`platform-services-helm`](https://dev.azure.com/defragovuk/DEFRA-FFC/_build?definitionId=2974)

Deploy a helm chart for a specific application version.  This pipeline should be run whenever an update to a service running in AKS is required selecting `service`, `helmChart` and `chartVersion`.

## Creating a new service definition

The above pipelines are dependent on teams maintaining an [ADO Git repository](https://dev.azure.com/defragovuk/DEFRA-FFC/_git/DEFRA-FFC-PLATFORM) with their service definitions and configuration.

This repository will need to be cloned locally and updated through a pull request.

### Create bounded context folder

Similar to Jenkins, each service should group it's service definitions in a folder matching the name of the bounded context under the `services` folder. 

For example, `ffc-demo`.

### Create definitions

Definitions should be created sub-categorised by the type of resource they are deploying.

Guidance for this activity can be found in the [Platform services documentation](https://dev.azure.com/defragovuk/DEFRA-FFC/_wiki/wikis/DEFRA-FFC.wiki/5171/DEFRA-FFC-Project-Wiki-Homepage).
