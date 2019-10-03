# Version Control
These standards are based on Defra Digital's [version control standards](https://github.com/DEFRA/software-development-standards/blob/master/standards/version_control_standards.md), but are extended to cover not only the application version, but version control of Docker images and Helm charts.

## Semantic Versioning
We use semantic versioning to manage code, images and charts.

Given a version number MAJOR.MINOR.PATCH, increment the:
- MAJOR version when you make incompatible API changes,
- MINOR version when you add functionality in a backwards compatible manner, and
- PATCH version when you make backwards compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

Full details can be found at [semver.org](https://semver.org).

## Application version
For a Node.js microservice, the version should be updated in the `package.json` file's `version` property.

## Image version
Prior to pushing to a container registry, all Docker images must be tagged. The tag must match the application version.

If the image is created as part of a pull request workflow, then the PR number should instead be used as the image tag, prior to pushing to the container registry.

For local development the tag should be added automatically when creating the image reading from the `package.json` (or equivalent) file.

Within the CI/CD pipeline there should be logic to determine whether to use to PR number depending on whether the commit is to master or a feature/topic branch.

## Helm chart version
Helm charts should be saved in a directory which matches the name of the chart. For example a chart referencing the ELM Payment Service would be stored in the `elm-payment-service` directory.

Helm chart versions should be updated in the `Chart.yaml` file's `version` property.

As Helm charts are saved in the same repository as the application they manage, the version numbers should be updated in sync with the application.

Helm charts for PR deployments should not be pushed to a Helm repository.

## Release version
Release should be packaged in a repository using the version number of the application.

## Creating a new version
The judgement call as to whether a change is a MAJOR, MINOR or PATCH can only be assessed by a person.  Therefore there needs to be a flag added to the application to indicate which type of release a change is.
