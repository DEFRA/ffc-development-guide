# Setup an Azure DevOps CD pipeline

Common Azure DevOps (ADO) pipeline definitions [have been created](https://dev.azure.com/defragovuk/DEFRA-FFC/_build) and can be reused to deploy services to the various environments.

These pipelines are responsible for not only deploying applications, but for provisioning and configuring the Azure environments the service run in.

## Pipeline categories

Pipelines are split into different capabilities which must be run sequentially into an environment when changes are made.

- [`platform-core`](https://dev.azure.com/defragovuk/DEFRA-FFC/_build?definitionId=2751) - 

## Creating a new service definition

ADO pipelines are dependent on teams updating an [ADO Git repository](https://dev.azure.com/defragovuk/DEFRA-FFC/_git/DEFRA-FFC-PLATFORM) with their service definitions.

This repository will need to be cloned locally and updated through a pull request.

### Create bounded context folder

Similar to Jenkins, each service should group it's service definitions in a folder matching the name of the bounded context under the `services` folder. 

For example, a repository named `ffc-pay-enrichment` would be defined in an `ffc-pay` folder.

### Create environment definitions



#### Create Helm definitions
