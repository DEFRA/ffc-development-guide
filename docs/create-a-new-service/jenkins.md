# Setup a Jenkins CI pipeline

Assuming the repository has been [configured correctly](github.md), including setup of [SonarCloud](https://sonarcloud.io/) and [Snyk](https://app.snyk.io/), the following steps should be followed to setup a Jenkins CI pipeline.

## Create bounded context folder

Each service should group it's CI pipelines in a Jenkins folder matching the name of the bounded context.  This is to ensure that the Jenkins UI remains manageable as the number of services grows.

For example, a repository named `ffc-pay-enrichment` would be placed in a folder named `ffc-pay`.

If a bounded context folder does not exist, create one by:

1. navigating to the [Jenkins home page](https://jenkins-ffc.azure.defra.cloud/)
1. select `New Item`
1. enter the `item name` in the format `ffc-<bounded context>`, for example `ffc-pay`
1. select `Folder`
1. select `Ok`

## Create a build pipeline

1. navigate to your bounded context folder in Jenkins
1. select `New Item`
1. enter the `item name` in the format `<repository name>-build`, for example `ffc-demo-web-build`
1. select `Multibranch Pipeline`
1. select `Ok`
1. enter `GitHub` as a Branch Source
1. for credentials select the `github-token` with the `ffcplatform` user
1. enter your GitHub URL in the `HTTPS URL` field, for example `https://github.com/DEFRA/ffc-demo-web.git`
1. set `Discover branches` to `All branches`
1. delete `Discover pull requests from origin` and `Discover pull requests from forks`
1. set `Scan Multibranch Pipeline Triggers -> Periodically if not otherwise run` to `true` with an interval of `1 hour`
1. set `Pipeline Action Triggers -> Pipeline Delete Event` set `ffc-housekeeping/cleanup-on-branch-delete`
1. set `Pipeline Action Triggers -> Include Filter` to be `*`
1. set `Pipeline Action Triggers -> Additional Parameter -> Parameter Name` to be `repoName` and `Parameter Value` to be the name of the repository, for example, `ffc-demo-web`

> Definitions can be copied from existing pipelines.  To save time, when creating a new pipeline select `Copy from` and enter the name of an existing pipeline.  Remember to update the `repoName` and `GitHub URL` fields.

## Create a deployment pipeline

Although Azure DevOps is used for deployments, Jenkins is used to trigger the deployment pipeline following the successful run of a main branch build.  

The deployment to the Sandpit environment is also orchestrated by Jenkins.

1. navigate to your bounded context folder in Jenkins
1. select `New Item`
1. enter the `item name` in the format `<repository name>-deploy`, for example `ffc-demo-web-deploy`
1. select `Pipeline`
1. select `Ok`
1. select `This project is parameterized`
1. add the following parameters:
   - `environment` with a default value of `snd`
   - `namespace` with a default value of `<bounded context>`
   - `chartName` with a default value of `<repository name>`
   - `chartVersion` with a default value of `1.0.0`
   - `helmChartRepoType` with a default value of `acr`
1. select `Trigger builds remotely (e.g., from scripts)`
1. enter `Authentication token` which can be found in any existing deployment pipeline
1. enter the below script in the `Pipeline` section with a `Definition` of `Pipeline script`

```groovy
@Library('defra-library@v-9') _

deployToCluster environment: params.environment, 
                namespace: params.namespace, 
                chartName: params.chartName, 
                chartVersion: params.chartVersion,
                helmChartRepoType: params.helmChartRepoType
```

## Test the pipeline

Commits to both feature branches and main branches should trigger the build pipeline.  Successful builds of the main branch should trigger the deployment pipeline.

Following deployment the new service should be available in the Sandpit AKS cluster in a namespace matching the bounded context.  For example, `ffc-demo-web` would be deployed to the `ffc-demo` namespace.

The specific steps that run during the pipeline are based on the content of the repository assuming it follows the [standard structure](conventions).

> Builds can also be triggered manually by selecting `Build Now` from the pipeline page.
