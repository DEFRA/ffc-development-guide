# Repository conventions

For teams using the FCP Platform, repositories should be setup with the following conventions to ensure consistency and to ensure the CI pipeline functions correctly.

## Structure

The following structure should be adhered to for all repositories.  Note that not all contents will be applicable depending on the nature of the service.

- `<root>`
  - `changelog`
    - Liquibase changesets
  - `docs`
    - Documentation such as [AsyncAPI](https://www.asyncapi.com/) and [OpenAPI](https://swagger.io/specification/) specifications
  - `helm/<repository name>`
    - Helm chart for the service
  - `scripts`
    - Scripts to support local development and testing
  - `.dockerignore`
  - `.gitignore`
  - `.snyk`
  - `Dockerfile`
  - `Jenkinsfile`
  - `LICENCE`
  - `README`
  - `docker-compose.debug.yaml`
  - `docker-compose.link.yaml`
  - `docker-compose.migrate.yaml`
  - `docker-compose.override.yaml`
  - `docker-compose.test.debug.yaml`
  - `docker-compose.test.watch.yaml`
  - `docker-compose.test.yaml`
  - `docker-compose.yaml`
  - `provision.azure.yaml`
  - `sonar-project.properties`

### Node.js

In addition to the above, Node.js services should include the following content.

- `<root>`
  - `app`
    - All application code
  - `test`
    - `unit`
      - Unit tests
    - `integration`
      - `narrow`
        - Narrow integration tests
      - `local`
        - Local integration tests
    - `contract`
      - Contract tests
    - `acceptance`
      - UI and compatibility tests (for frontend applications)

### .NET

In addition to the above, .NET services should include the following content.

- `<root>`
  - `<Project Name>`
    - All application code
  - `<Project Name>.Tests`
    - Application tests
