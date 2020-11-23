# Testing
To support developer agility and consistent interpretation of the FFC Test Stragegy, a common test structure has been agreed for usage across FFC microservices.

## Static code
SonarCloud will provide static code analysis on all repositories regardless of technology choice.  

In addition to SonarCloud, any JavaScript code is also linted using [StandardJs](https://standardjs.com/).  This linting is performed of a test script in package.json which is also run in CI pipeline.

There is no linting used for C# in the programme at present.

## Unit
Unit tests should be used to: 

target functionality that is not already covered through contract or integration testing

add extra assurance or visibility to specific functionality, for example an application that runs a series of calculations to determine a monetary value, may benefit from individual unit tests for each calculation.

Code should be written so that where unit tests are required, mocking is not required if possible or is minimal.

Unit tests should mock immediate dependencies only.

## Integration
Integration tests verify the interactions of several components, rather than a single unit. They are classified further into narrow and local integration tests. 

### Narrow integration
Narrow integration tests should be considered as tests that are bigger than unit tests, but smaller than integration involving other services or modules.

For example where a unit test would traditionally mock a web server, a narrow integration test may start the web server albeit without exposing any ports externally.

Narrow integration tests should mock immediate dependencies only.

### Local integration
Local integration tests should cover functionality that integrates with dependencies that are local to the service, for example message queues and databases.

Rather than mocking the database or queue, local integration tests could use a database or message queue container.  This reduces the need for mocks, increasing developer agility, reducing test complexity and a higher level of confidence in tests.

Where integration with third party services are needed, not covered by contract tests, then tests should be make use use of Docker containers and stubs to reduce the need for mocking.

## Contract
Contract tests should cover all provider-consumer relationships with other services that are owned by FFC.  Where one side of the contract is not owned by FFC then more traditional provider-driven or even broad integration tests with test instances of those services may be preferred.  

Contract tests should mock the immediate dependencies of the contract similar to unit tests. 

Service architectures should be written in such a way that reduce the need for these higher risk contracts.  For example introducing an FFC managed API gateway between third party services and FFC, would allow all FFC services to contract test against the API gateway abstraction.  More complicated end to end testing is then restricted to only the API gateway.

## Acceptance
Acceptance tests should cover key journeys through the application flow via the user interface. These are  defined in a Gherkin syntax (Given, When, Then) and use cucumber and webdriver.io to mimic a scenario from the user perspective. They’re executed against the microservices integrated and running as the complete service with the exception of external services (outside of the platform) which are stubbed.

## Repository structure
### Node.js
- app - all application code
- test
  - unit - unit tests
  - integration - integration tests
    - narrow - narrow integration tests
    - local - local integration tests
  - contract - contract tests
  - acceptance - acceptance tests (for frontend applications)

The integration tests are separated in the folder structure to reflect their different dependencies. 

This enables narrow integration tests that have no external dependencies to be easily run in isolation from the local integration tests.

#### Standards
- test files should be suffixed with test.js 
- test subfolders should be as flat as possible and not mirror application code structure
- tests should be named so it is clear which module they relate to.  For example a module caclulations/gross.js could have a test file called, calcuation-gross.test.js

### .NET Core
- App - application csproj
- App.Tests - application tests csproj
  - Integration
  - Unit
  - Contract
  - Acceptance (for frontend applications)

#### Standards
- test subfolders should be as flat as possible and not mirror application code structure
- tests should be named so it is clear which module they relate to.  For example a module Caclulations/Gross.cs could have a test file called, CalcuationGrossTest.cs
- all test types should be in one project

## Running tests
- all tests should be executed in CI
- a `docker-compose.test.yaml` must exist in the repository to support running tests in CI.
- for local development, it may be beneficial to create a script/ Docker Compose combination to aid test environment setup and to take advantage of file watching.
