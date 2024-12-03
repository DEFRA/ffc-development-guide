# Continuous Integration and Continuous Delivery (CI/CD)

For the FCP Platform, [Jenkins](https://jenkins-ffc.azure.defra.cloud/) is used for CI pipelines, whilst [Azure DevOps](https://dev.azure.com/defragovuk/DEFRA-FFC) is used for CD.

Originally the FCP Platform was primarily hosted on [AWS](https://aws.amazon.com/) where Jenkins was prescribed as the CI/CD tool.  However, as the platform has migrated to [Azure](https://azure.microsoft.com/), Azure DevOps was prescribed as the CD tool.  Jenkins is still used for CI pipelines as it is a well established tool within the programme and redevelopment of the pipelines in Azure DevOps would be a significant undertaking.

## Continuous Integration (CI)

FCP adopt a "shift left" approach to security and quality, meaning that security and quality checks are performed as early as possible in the development lifecycle.  The CI pipeline is designed around this principle and looks to provide fast feedback to developers and testers prior to merging code to the main branch.

Feature branch development allows for small changes to be made and tested in isolation.  This is supported by the CI pipeline which will run a subset of tests and checks on a feature branch build.  The pipeline will also provision a dedicated environment for the feature branch build to be deployed to, allowing for a more comprehensive set of tests to be run.

In order to avoid duplication across all microservices, a [common Jenkins pipeline](https://github.com/DEFRA/ffc-jenkins-pipeline-library) has been created.

> Note that a feature branch must have an open Pull Request in order for the CI pipeline to run.

### Supported technologies

The CI pipeline supports the following technologies:

- Containerised Node.js applications targetting Azure Kubernetes Service (AKS)
- Containerised .NET applications targetting Azure Kubernetes Service (AKS)
- Helm charts
- Azure Functions
- Docker images
- npm packages

### Pipeline stages

## Build steps

The steps for a feature or main branch build are more or less the same.  The most significant difference is there is no feature branch deployment for a main build.  Instead additional build assets are created ready to be promoted to higher environments.

Below shows the pipeline steps for building a Node.js application targetting AKS.  The steps for other technologies are similar.  Anywhere `CUSTOM INJECTION` is shown custom steps can be injected.

Some steps are optional depending on the content of the repository.

1. Validate semantic version of package
1. Lint Helm chart
1. npm Audit vulnerability check
1. Snyk vulnerability check
1. `CUSTOM INJECTION`
1. Build test Docker image
1. Provision dynamic infrastructure for feature branch and to support integration tests
1. `CUSTOM INJECTION`
1. Run containerised tests (linting, unit, integration, contract)  
1. `CUSTOM INJECTION`
1. Publish contracts to Pact Broker
1. SonarCloud static code analysis
1. Build production Docker image
1. Publish image to to Azure Container Registry
1. Deploy image to dynamic AKS namespace (feature branch build only)
1. AXE accessibility tests (optional)
1. BrowserStack containerised UI and compatibility tests (optional)
1. Zap security tests (optional)
1. JMeter performance tests (optional)
1. Publish Helm chart to AZure Container Registry (main branch build only)
1. `CUSTOM INJECTION`
1. Create GitHub release (main branch build only)
1. Clean up build specific dynamic infrastructure
1. `CUSTOM INJECTION`

## Clean up steps

Once a feature branch is deleted, the clean up process will automatically run to remove any dynamic infrastructure.

## Continuous Delivery (CD)

Once a feature branch has been merged into the main branch, the CI pipeline will automatically run to package the assets and deploy the application to the Sandpit environment.  Assuming the Sandpit deployment was successful, Jenkins will then trigger Azure DevOps to deploy the application to the higher environments.

ADO will deploy the application to the following environments once the previous environment has been successfully deployed to:

- Development (automatic)
- Test (requires approval from anyone in the team)
- PreProduction (requires approval from anyone in the team)
- Production (requires approval from CCoE and an Request for Change (RFC))

> Note that some of the more mature teams in FCP have approval to run their own changes to Production using a standard change.
