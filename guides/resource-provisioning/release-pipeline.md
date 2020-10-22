# Release pipeline

Release pipelines are run from Jenkins and a deployment to the development environment is automatically triggered by the CI pipeline if building from a main branch.

**Note** this approach to a release pipeline is only a temporary approach and will be replaced when our release capability using Azure DevOps is ready for use.

## Configure release pipeline

- navigate to your workstream folder in Jenkins
- select `Credentials -> (global) -> Add Credentials`
- select `Secret text` type and set both `Description` and `Id` to `<respository name>-deploy-token`, for example `ffc-demo-web-deploy-token`
- set the secret value to be a unique string
- navigate back to the workspace folder and select `New Item`
- enter the `item name` in the format `<repository name>-deploy`, for example `ffc-demo-web-deploy`
- select `Pipeline`
- select `Ok`
- set `This project is parameterised` to `true` and add the following parameters
  - `Name`: `chartVersion`
  - `Default Value`: `1.0.0`
  - `Description`: `Version of service to deploy`  
  - `Name`: `chartName`
  - `Default Value`: `<repository name>` for example, `ffc-demo-web`
  - `Description`: `Service to deploy`
  - `Name`: `namespace`
  - `Default Value`: `ffc-<service name>` for example, `ffc-grants`
  - `Description`: `Cluster namespace to deploy to`
  - `Name`: `environment`
  - `Default Value`: `dev`
  - `Description`: `Cluster environment to deploy to`
  - `Name`: `namespace`
  - `Default Value`: `ffc-<service name>` for example, `ffc-grants`
  - `Description`: `Cluster namespace to deploy to`
  - `Name`: `helmChartRepoType`
  - `Default Value`: `acr`
  - `Description`: `Location of Helm charts`
- set `Trigger builds remotely` to `true`
- enter unique deploy token created above
- add inline pipeline script with the following content
  ```
  @Library('defra-library@v-8') _

  deployToCluster environment: params.environment, 
                namespace: params.namespace, 
                chartName: params.chartName, 
                chartVersion: params.chartVersion,
                helmChartRepoType: params.helmChartRepoType
  ```
- select `Save`
