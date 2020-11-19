# Release pipeline

Release pipelines are run from Jenkins and a deployment to the development environment is automatically triggered by the CI pipeline if building from a main branch.

**Note** this approach to a release pipeline is only a temporary approach and will be replaced when our release capability using Azure DevOps is ready for use.

## Configure release pipeline

- navigate to your workstream folder in Jenkins
- select `Credentials -> (global) -> Add Credentials`
- select `Secret text` type and set both `Description` and `Id` to `<respository name>-deploy-token`, for example `ffc-demo-web-deploy-token`
- set the secret value to be a unique string, this value will be used to authenticate the CI pipeline when triggering a release
- navigate back to the workspace folder and select `New Item`
- enter the `item name` in the format `<repository name>-deploy`, for example `ffc-demo-web-deploy`
- select `Pipeline`
- select `Ok`
- set `This project is parameterised` to `true` and add the following parameters

  |Name|Default Value|Description|
  |---|---|---|
  |`chartVersion`|`1.0.0`|`Version of service to deploy`|
  |`chartName`|`<repository name>` for example, `ffc-demo-web`|`Service to deploy`|
  |`namespace`|`ffc-<service name>` for example, `ffc-grants`|`Cluster namespace to deploy to`|
  |`environment`|`dev`|`Cluster environment to deploy to`|
  |`helmChartRepoType`|`acr`|`Location of Helm charts`|

- set `Trigger builds remotely` to `true`
- enter unique deploy token created above
- add inline pipeline script with the following content

  ```
  @Library('defra-library@v-9') _

  deployToCluster environment: params.environment,
                namespace: params.namespace,
                chartName: params.chartName,
                chartVersion: params.chartVersion,
                helmChartRepoType: params.helmChartRepoType
  ```
- select `Save`
