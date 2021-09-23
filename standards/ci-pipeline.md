# FFC CI pipeline
The [FFC CI pipeline](https://github.com/DEFRA/ffc-jenkins-pipeline-library) is a shared Jenkins library.  The purpose of which is to abstract the complexity of building, testing and deploying applications away from consuming microservices.

All FFC microservices will use the FFC CI pipeline.

## Supported technologies
- Node.js
- .NET Core

## Pipeline trigger
The pipeline will trigger automatically on any commit to a branch with an open Pull Request (PR) or the main branch.

## Build steps
The steps for a PR or main build are more or less the same.  The most significant difference is there is no deployment to a dedicated PR Kubernetes namespace with a main build.  Instead additional build assets are created.

The pipeline supports context specific flexibility by allowing custom steps to be injected into various points.  Anywhere `CUSTOM INJECTION` is shown below can support any number of additional steps to run.

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
- run accessibility tests
  - Pa11y
  - AXE
- run acceptance tests
  - Selenium
  - Cucumber
  - BrowserStack
- run security tests 
  - OWASP Zap
- run performance tests
  - JMeter
- publish Helm chart (main build only)
- CUSTOM INJECTION
- create GitHub release (main build only)
- destroy dedicated infrastructure for build
- CUSTOM INJECTION

## Cleanup steps
On closure or merge of a PR, the cleanup pipeline will run.  This will destroy any related dynamically created infrastructure or PR deployments.
