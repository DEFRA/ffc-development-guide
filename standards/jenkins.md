# Jenkins
Jenkins is used for CI pipelines across all FFC teams.

Where possible, build steps are containerised to reduce plugin and Jenkins configuration dependencies.

## Shared library
Node.js and .NET Core microservices use the [FFC CI pipeline](ci-pipeline.md) for managing builds.  This pipeline abstracts complexity such as infrastructure provisioning, building and testing away from consuming microservices, significantly reducing the effort needed to build and deploy services in line with other FFC standards.

## Jenkinsfiles
Jenkinsfiles use the Groovy syntax, adhering to the official [Apache Groovy style guide](https://groovy-lang.org/style-guide.html)

Microservice Jenkinsfiles are source controlled in the same repository as the microservice itself.

Secret values and sensitive configuration are not included in Jenkinsfiles, instead use either a parameter or environment variable.

## Credentials
Jenkins credentials should only be used for Jenkins pipeline specific activities.  They should not hold credentials relating to microservice configuration or deployment.  These credentials should be sourced from Azure Key Vault or for files, a private Azure Repository.

## Workspace folders
All delivery teams will have their own workspace folders to logically separate their pipelines and credentials. 
The naming convention of these folders will be `ffc-<service>`.  For example, `ffc-grants`.
