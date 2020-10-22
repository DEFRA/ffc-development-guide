# FFC CI Pipeline

## Create build pipeline for a microservice

All Jenkins pipelines are created within a workstream team's Jenkins folder, for example `ffc-grants`.

All pipelines must use the [FFC CI pipeline](../standards/ci-pipeline.md).

- generate an SSH key pair
- in GitHub, navigate to your repository's `Settings -> Deploy Keys -> Add deploy key`
- add the public key as a new deployment key
- navigate to your workstream folder in Jenkins
- select `Credentials -> (global) -> Add Credentials`
- select `Secret text` type and set both `Description` and `Id` to `<respository name>-deploy-token`, for example `ffc-demo-web-deploy-token`
- set the secret value to be the private key
- select `New Item`
- enter the `item name` in the format `<repository name>-build`, for example `ffc-demo-web-build`
- select `Multibranch Pipeline`
- select `Ok`
- enter `GitHub` as a Branch Source
- for credentials select the `ffcplatform` GitHub token
- enter your GitHub URL in the `HTTPS URL` field, for example `https://github.com/DEFRA/ffc-demo-web.git`
- set `Pipeline Action Triggers -> Pipeline Delete Event` set `ffc-housekeeping/cleanup-on-branch-delete` 
- set `Pipeline Action Triggers -> Include Filter` to be `*`
- set `Pipeline Action Triggers -> Additional Parameter -> Parameter Name` to be `repoName` and `Parameter Value` to be the name of the repository, for example, `ffc-demo-web`
