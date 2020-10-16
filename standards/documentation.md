# Documentation

FFC products are delivered following an agile methodology based on evidenced user needs. Documentation is expected to follow this mantra and be iterated with the code as the product evolves.

Documentation should be "just enough" to cover what needs to be communicated and presented as simply and effectively as possible.

## Documentation Location
Documentation will be stored in the same source code repository as the product it supports.

FFC developments will mostly be within an open source microservices ecosystem. Each microservice will have its own repository and its own documentation.

## Readme
A readme file must exist in every repo in markdown format (.md). It must include the following **if** they apply:
- Description of product
- Prerequisites
- Development tools setup
- Test tools setup
- How to run in development
- How to run tests
- How to make changes to the code

### Additional Detail
Depending on the complexity of the product, the following may be included in the readme, or it may be more effective to capture in separate documentation/diagrams referenced by the readme.

- How product fits into wider architecture
- Internal product architecture
- Data structure
- API end points
- Monitoring
- Error handling
- Audit
- User access
- Security
- Complexity worth documenting
- Pipelines

## Microservice Reference Library
This is a reference library of all microservices created and maintained by FFC.  The purpose of which is so we have an easy access resource to understand what capability we have developed, but also to promote reuse where appropriate.

The registry is updated initially by the Solution Architect when designing the service.  It is then updated and maintained by the developer as the service iterates through it's lifecycle.

The registry is in [Confluence](https://eaflood.atlassian.net/wiki/spaces/FPS/pages/2315288783/Microservice+Reference+Library)

### Library Content
- Name of service
- Owner of service
- Link to Git repository
- Link to Jira backlog
- Summary of service's purpose and capability
- Architectural diagrams following C4 model, see [c4model.com](https://c4model.com/) for more information
- Technology stack
- Dependencies
- API specification, can be link to OpenAPI specification file
- Events, can be link, can be link to AsyncAPI specification file
- Functions
- Version history, can be link to GitHub
- Usage, ie which services use this microservice
- Useful references

## Architecture Documentation
There may be documentation owned by FFC architecture that developers need to contribute to.

For example, a Defra wide API endpoint catalogue or a Cloud Service Centre namespace library. These documents should be completed in line with the standards of the owner.

## Architectural Decisions
Architectural decisions are discussed and agreed at FFC Technical Design Authority (TDA) chaired by the Chief Architect. Agreed outputs are captured as part of the TDA by Architecture.

## Technical Debt
A current view of technical debt is maintained by TDA. Developers are expected to contribute to forming this vision.

## Code
Effective software engineering can negate the need for additional documentation. Code should follow Defra's digital standards and apply appropriate recognised design patterns.

We should aim to write readable code that requires minimal comments as per Defra's digital standards. However in some scenarios effective code comments can be used to explain complexity.

Well named and considered unit tests can also act as effective documentation.

## Issues
Work items are captured in the form Jira issues.

There is no need to duplicate issues in additional documentation.

Issues should be referenced in associated Pull Requests.

## Sensitive Documentation
Sensitive documentation should be stored within a private repository within Azure DevOps or Confluence depending on the audience.
