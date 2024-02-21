# Create a source code repository

As per Defra and GDS standards, source code should be open source and hosted on GitHub.  Other tools such as Azure DevOps, Bitbucket and GitLab are not permitted for application development.

Scenarios where open source is not appropriate are rare and must be approved by a Principal Developer.

All source code is hosted in the [DEFRA GitHub Organisation](https://github.com/DEFRA)

Repositories should contain a single microservice/application.  Monorepos are not supported by CI pipeline.

## Naming conventions

Repositories should be named in a consistent manner to aid discoverability and to ensure that they are easily identifiable as part of FCP.

Below is the agreed naming convention for repositories.

`ffc-<bounded context>-<service>`

Where bounded context is an identifier to a set of related microservices and services is an identifier for the specific service.

For example, `ffc-pay-enrichment` would belong to FCP, be part of the Payment ecosystem and perform an enrichment function.

Following this naming convention helps understand ownership of repositories, avoid collisions and is essential for some CI/CD pipelines to function correctly.

## Creating a new repository

When creating a new repository intended for a Node.js application, the repository can be based on the [ffc-template-node](https://github.com/DEFRA/ffc-template-node) template repository.  

The template includes everything needed to get a minimal Node.js application up and running with CI/CD pipelines and follows all FCP structure and naming conventions.  Once created follow the instructions in the `README.md` file to rename the assets contained within.

> For .NET there is no template repository, however, this [demo repository](https://github.com/DEFRA/ffc-demo-payment-service-core) can be used for guidance.

Due to the GitHub permission model, only certain users can create new repositories.  If you are unable to create a new repository, please contact a Principal Developer.

## Repository setup

### Update access

All repositories should be setup to allow access to the FCP team.  This is done by adding the [`ffc`](https://github.com/orgs/DEFRA/teams/ffc) team with `Write` access to the repository.

> For scenarios where it may not be appropriate to add the FCP team, note that the [`ffcplatform`](https://github.com/orgs/DEFRA/people/ffcplatform) user account should be added with `Write` access to allow the CI pipeline to function correctly.

### Branch policies

Teams can extend branch policies to fit their needs, however, the following are the minimum requirements for the `main` branch on all repositories.

- protected by a branch policy
- requires at least one pull request review before merging
- stale reviews are automatically dismissed
- repositories only allow Squash merging
- must require signed commits

> For prototype and spike repositories, the branch policy can be relaxed to allow direct commits to the `main` branch.

### Secret scanning

All developers are required to [install Detect Secrets](../local-development-setup/install-detect-secrets.md).

When setting up a new repository, it needs to be configured to work with the client-side `detect-secrets` tool:

> Note that if the repository was created from the [ffc-template-node](https://github.com/DEFRA/ffc-template-node) template repository, this configuration will already be in place.

1. Add the configuration file called `.pre-commit-config.yaml` to the repository root directory with the following contents:

```yaml
repos:
- repo: https://github.com/Yelp/detect-secrets
  rev: v0.14.3
  hooks:
  - id: detect-secrets
    args: ['--baseline', '.secrets.baseline']
```

2. Create and add the baseline file. To exclude files from the `detect-secrets` scan, run with `--exclude-files <regex>`. Examples:

```bash
detect-secrets scan > .secrets.baseline
```

```bash
detect-secrets scan --exclude-files package-lock.json > .secrets.baseline
```

### Licence

The Open Government Licence (OGL) Version 3 should be added to the repository.  Example content is below.

```
The Open Government Licence (OGL) Version 3

Copyright (c) 2024 Defra

This source code is licensed under the Open Government Licence v3.0. To view this
licence, visit www.nationalarchives.gov.uk/doc/open-government-licence/version/3
or write to the Information Policy Team, The National Archives, Kew, Richmond,
Surrey, TW9 4DU.
```

### `Jenkinsfile`

In order to use the FFC CI pipeline, a `Jenkinsfile` should be added to the repository.  This file references the version of the shared pipeline to use and sets the parameters for the build.

Details on the content of the `Jenkinsfile` can be found in the [FCP CI pipeline documentation](https://github.com/DEFRA/ffc-jenkins-pipeline-library)

### Webhooks

In order to support the CI/CD pipeline, the following webhooks should be added to the repository.

1. navigate to `Settings -> Webhooks -> Add webhook`
1. set `Payload URL` to be `https://jenkins-ffc-api.azure.defra.cloud/github-webhook/`
1. set `Content type` to be `application/json`
1. set `Secret` to be the webhook secret value.  This can be retrieved from Azure Key Vault `github-webhook-secret` value in the FCP Shared Services Azure subscription.
1. set the `Pull requests` and `Pushes` events to trigger the webhook
1. select `Add webhook`

## SonarCloud

### Configure SonarCloud

> This step should be performed before running a build or you may end up with duplicate projects.

1. navigate to the Defra organisation within [SonarCloud](https://sonarcloud.io/organizations/defra/projects?sort=analysis_date)
1. select `Analyze new project`
1. select repository to analyse
1. select `Set Up`
1. select `Administration -> Analysis Method` and disable `SonarCloud Automatic Analysis`
1. select `Administration -> Update key` and ensure key matches the name of the repository, removing the `DEFRA_` prefix, for example, `ffc-pay-enrichment`

### Synk

A [Snyk](https://app.snyk.io/) scan is run as part of the CI pipeline. By default, a project will be set to private. In order to access unlimited [free tests](https://snyk.io/plans/) the project needs to be marked as open source. To do so, follow the [instructions](https://support.snyk.io/hc/en-us/articles/360000910597).


