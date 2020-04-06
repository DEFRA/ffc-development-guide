# CI/CD pipelines
The capabilities of FFC are underpinned by DevOps CI/CD pipelines providing cloud agnostic tooling with build and release automation through Jenkins.

Jenkins standards and guides can be found [here](jenkins.md).

## Pipeline design
The below diagrams show the current design for a CI/CD pipeline that embraces a DevOps culture.

**Note that these designs are currently in draft state and will be further refined through engagement with GIO, CSC, Security and emerging needs of the FFC digital services**

![CI/CD pipeline](/docs/images/cicd.png)

Each pipeline specified could be made up of multiple Jenkinsfiles.

## Pull Request pipelines
Following creation of and on update to a Pull request, the PR pipeline should run automatically.  There will actually be two PR pipelines, the first will run on every push to remote, whilst the second will run prior to any merge to master.  A tag will be added to the branch to allow Jenkins to understand which pipeline to run.

### Pipeline steps (1)
- build the service
- run unit tests
- run containerised integration tests
- run code quality tests
- tag container image
- push image to container registry
- deploy to non-production PR environment
- report build status

### Pipeline steps (2)
- build the service
- run unit tests
- run containerised integration tests
- run code quality tests
- tag container image
- push image to container registry
- deploy to non-production PR environment
- run UI tests
- run security tests
- report build status

A failed PR build must prevent the PR from completing.

## Build pipeline
Following merge to the master branch, the build pipeline should run automatically.  The purpose of this pipeline is to provide assurance that the service is production ready.

### Pipeline steps
- build the service
- set version
- validate container image hash value
- delete PR image
- run unit tests
- run containerised integration tests
- run code quality tests
- run acceptance tests
- run security tests
- run performance tests
- run accessibility tests
- tag container image
- push image to container registry
- package Helm chart
- push Helm chart to Helm repository
- deploy to non-production environment
- run UI tests
- run compatibility tests
- create release note
- run service hooks
- report build status

## Release pipeline
The release pipeline is used to deploy a service to production.

Depending on the service, the trigger for a release pipeline could be:
- automatically triggered by a completed build
- triggered by a service hook
- manually triggered

### Pipeline steps
- deploy to production environment
- add production flag to image in container registry
- add production flag to Helm chart in Helm repository
- archive release code
- run service hooks
- report release status

## Ongoing pipeline
This pipeline will run on a regular basis to provide ongoing assurance regarding the quality and integrity of production services.

### Pipeline steps
- run performance tests
- run security tests
