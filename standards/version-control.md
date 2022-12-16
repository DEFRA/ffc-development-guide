# Version Control

These standards are based on Defra Digital's [version control standards](https://github.com/DEFRA/software-development-standards/blob/master/standards/version_control_standards.md), but are extended to cover not only the application version, but version control of Docker images and Helm charts.

## Semantic Versioning

We use semantic versioning to manage code, images and charts.

Given a version number MAJOR.MINOR.PATCH, increment the:
- `MAJOR` version when you make incompatible API changes,
- `MINOR` version when you add functionality in a backwards compatible manner, and
- `PATCH` version when you make backwards compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions to the `MAJOR.MINOR.PATCH` format.

Full details can be found at [semver.org](https://semver.org).

## Application

- for a Node.js microservice, the version is updated in the `package.json` file's `version` property.  

- for a .NET microservice, the version is updated in the `.csproj` file in a `<Version>` tag.

## Image

- prior to pushing to a container registry, all Docker images must be tagged by CI. The tag must match the application version.

- if the image is created as part of a pull request workflow, then the PR number is instead used as the image tag.

## Helm chart

- Helm chart versions are updated in the `Chart.yaml` file's `version` property.

- as Helm charts are saved in the same repository as the application they manage, the version numbers are updated in sync with the application by CI.

- Helm charts for PR deployments are not be pushed to a Helm repository.

**Note** when using the [FFC Jenkins library](https://github.com/DEFRA/ffc-jenkins-pipeline-library), Helm chart versioning is handled automatically, so no action is needed

## Releases

- releases are packaged in the source code repository using the version number of the application by CI

## Databases

- databases use Liquibase changesets for deployment and rollback.  

- all changesets are versioned independently of the application.

- there must be an initial `0.0.0` version before any changeset, to enable full rollback of a database
