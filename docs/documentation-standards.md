# Future Farming and Countryside Programme Software Documentation Strategy

## Overview
This document details how software should be documented within the Future Farming and Countryside Programme (FFC).  

FFC products are delivered following an agile methodology based on evidenced user needs.  Documentation is expected to follow this mantra and be iterated with the code as the product evolves.  

Documentation should be "just enough" to cover what needs to be communicated and presented as simply and effectively as possible.  

## Documentation Location
Documentation will be stored in the same source code repository as the product it supports.

FFC developments will mostly be that of an open source microservices ecosystem. Each microservice will have its own repository and its own documentation.

## Readme
A readme file must exist in every repo in markdown format (.md).  It must include the following **if** they apply:
- Description of product
- Prerequisites
- Development tools setup
- Test tools setup
- How to run in development
- How to run tests
- How to make changes to the code

## Additional Detail
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
  
## Architecture Documentation
There may be documentation owned by FFC architecture that developers need to contribute to.

For example, a Defra wide API endpoint catalogue or a Cloud Service Centre namespace library.  These documents should be completed in line with the standards of the owner.

## Architectural Decisions
Architectural decisions are discussed and agreed at FFC Technical Design Authority (TDA) chaired by the Chief Architect.  Agreed outputs are captured as part of the TDA by Architecture.

## Technical Debt
A current view of technical debt is maintained by TDA.  Developers are expected to contribute to forming this vision.

## Code
Effective software engineering can negate the need for additional documentation.  Code should follow Defra's digital standards and apply appropriate recognised design patterns.

We should aim to write readable code that requires minimal comments as per Defra's digital standards.  However in some scenarios effective code comments can be used to explain complexity.

Well named and considered unit tests can also act as effective documentation.

## User Stories
User stories should be captured and tracked in an appropriate tool such as Jira or Azure DevOps.

User stories should have testable acceptance criteria and be supported by evidenced user needs.

There is no need to duplicate user stories in additional documentation.  

User stories should be referenced in associated Pull Requests.

## Sensitive Documentation
Sensitive documentation should be stored within a private repository within Azure DevOps.

## Developer Guide
The aim of the Developer Guide is to allow a new developer to FFC to understand the software landscape of FFC and how they can work within it.  It should include the following:

- Technologies and languages used
- Setting up a development laptop
- Digital standards
- Delivery standards
- Backlog location
- Estimation Strategy
- Docker standards
- Developing locally with Docker and Kubernetes
- Kubernetes cluster management
- Pull Request workflow
- High level architecture
- Source code catalogue, location and naming convention
- Pipeline location, requirements and naming convention
- Cloud environment overview and access procedure
- Slack and other channels
- Organisation Chart
- Developer and Test Community of Practice details
- Complexity worth documenting
- Developer checklist for common changes
- Documentation standards

Several of these items may include links to other documentation so there is a single source of the truth.
