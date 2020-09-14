# FFC CI pipeline
The [FFC CI pipeline](https://github.com/DEFRA/ffc-jenkins-pipeline-library) is a shared Jenkins library.  The purpose of which is to abstract the complexity of building, testing and deploying applications away from consuming microservices.

All FFC microservices will use the FFC CI pipeline.

## Supported technologies
- Node.js
- .NET Core

## Pipeline trigger
The pipeline will trigger automatically on any commit to a branch with an open Pull Request (PR) or the master branch.

## Build steps
The steps for a PR or master build are more or less the same.  The most significant difference is there is no deployment to a dedicated PR Kubernetes namespace with a master build.  Instead additional build assets are created.

The pipeline supports context specific flexiblity by allowing custom steps to be injected into various points.  Anywhere `CUSTOM INJECTION` is shown below can support any number of additional steps to run.

- validate semantic version of package
- lint Helm chart
- package vulnerability scanning
  - npm Audit (Node.js only)
  - Snyk
- CUSTOM INJECTION
- build test image
- provision dynamic infrastructure for PR deployment and build
  - Azure Service Bus
  - Azure PostgreSQL
- CUSTOM INJECTION
- run tests
  - lint
  - unit
  - narrow integration
  - local integration (uses dynamic build infrastructure)
  - contract
- CUSTOM INJECTION
- publish contract
  - Pact Broker
- static code analysis
  - SonarCloud
- build image
- push to container registry
- deploy to PR namespace (PR build only)
- run acceptance tests (PR build only)
  - Selenium
  - Cucumber
- run security tests 
  - OWASP Zap
- run performance tests
  - JMeter
- publish Helm chart (master build only)
- CUSTOM INJECTION
- create GitHub release (master build only)
- destroy dedicated infrastructure for build
- CUSTOM INJECTION

## Cleanup steps
On closure or merge of a PR, the cleanup pipeline will run.  This will destroy any related dynamically created infrastructure or PR deployments.

## Upcoming steps
The FFC CI pipeline is rapidly iterating.  Upcoming features include:
- accessiblity tests
- compatibility tests
