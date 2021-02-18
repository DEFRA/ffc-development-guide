# Release pipelines

## Sandpit
Sandpit release pipelines are run from Jenkins and a deployment to the sandpit environment is automatically triggered by the CI pipeline if building from a main branch.

### Configure sandpit release pipeline

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

## Development, Test, PreProduction and Production release pipelines
Release pipelines to higher environments are run from Azure DevOps using a common microservice pipeline.

A deployment to the development and test environments environment are automatically triggered following a build from a main branch.  These pipelines can also be run manually by delivery teams.

Deployments to PreProd require a ticket raised with CCoE.

Deployments to Production require an RFC raised in myIT to authorise CCoE to deploy.

### Preparations for higher environments
Teams should ensure they have completed the following before deploying to higher environments.

- Update Azure Application Configuration is updated with all required Helm chart configurations
- Share any new databases created with the Platform team.  The release pipelines are currently dependent on knowing which services have databases in advance.  This is expected to change in the near future.
- Ensure all managed identities are created
- Ensure all Service Bus queues are created
