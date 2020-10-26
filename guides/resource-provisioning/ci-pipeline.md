# FFC CI Pipeline

## Create build pipeline for a microservice

All Jenkins pipelines are created within a workstream team's Jenkins folder, for example `ffc-grants`.

All pipelines must use the [FFC CI pipeline](../standards/ci-pipeline.md).

### Configure GitHub repository

- navigate to `Settings -> Webhooks -> Add webhook`
- set `Payload URL` to be `https://jenkins-ffc-api.azure.defra.cloud/github-webhook/`
- set `Content type` to be `application/json`
- set `Secret` to be the webhook secret value.  This can be retrieved from Azure Key Vault
- set the following events to trigger the webhook
  - `Pull requests`
  - `Pushes`
- select `Add webhook`

### Configure Jenkins

- navigate to your workstream folder in Jenkins
- select `New Item`
- enter the `item name` in the format `<repository name>-build`, for example `ffc-demo-web-build`
- select `Multibranch Pipeline`
- select `Ok`
- enter `GitHub` as a Branch Source
- for credentials select the `ffcplatform` GitHub token
- enter your GitHub URL in the `HTTPS URL` field, for example `https://github.com/DEFRA/ffc-demo-web.git`
- set `Discover branches` to `All branches`
- delete `Discover pull requests from origin` and `Discover pull requests from forks`
- set `Scan Repository Triggers -> Periodically if not otherwise run` to `true` with an interval of `1 hour`
- set `Pipeline Action Triggers -> Pipeline Delete Event` set `ffc-housekeeping/cleanup-on-branch-delete`
- set `Pipeline Action Triggers -> Include Filter` to be `*`
- set `Pipeline Action Triggers -> Additional Parameter -> Parameter Name` to be `repoName` and `Parameter Value` to be the name of the repository, for example, `ffc-demo-web`

### Configure SonarCloud

**Note** this step should be performed before running the build or you may end up with duplicate projects.

- navigate to the Defra organisation within [SonarCloud](https://sonarcloud.io/organizations/defra/projects?sort=analysis_date)
- select `Analyze new project`
- select repository to analyse
- select `Set Up`
- select `Adminsitration -> Analysis Method` and disable `SonarCloud Automatic Analysis`
- select `Administration -> Quality Profiles` and set all to `Sonar way`
- select `Administration -> Quality Gate` and set to `Sonar way`
- select `Administration -> Update key` and ensure key matches the name of the repository, removing the `DEFRA_` prefix, for example, `ffc-demo-web`

### Configure repository

- add this [Jenkinsfile](../../resources/Jenkinsfile) to the repository removing either the Node.js or .NET Core line as appropriate.

For more information about usage of this Jenkinsfile, see the [FFC CI pipeline documentation](https://github.com/DEFRA/ffc-jenkins-pipeline-library).
