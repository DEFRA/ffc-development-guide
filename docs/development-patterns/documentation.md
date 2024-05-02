# Documentation

FCP services are delivered following an agile methodology based on evidenced user needs. Documentation is expected to follow this mantra and be iterated with the code as the product evolves.

Documentation should be "just enough" to cover what needs to be communicated and presented as simply and effectively as possible.

## Documentation Location

Code documentation will be stored in the same source code repository as the product it supports unless it contains sensitive information.

FCP developments will mostly be within an open source microservices ecosystem. Each microservice will have its own repository and its own documentation.

## Readme

A readme file must exist in every repo in markdown format (.md). It must include the following **if** they apply

- description of service
- prerequisites
- development tools setup
- test tools setup
- how to run in development
- how to run tests
- how to make changes to the code

### Additional Detail

Depending on the complexity of the service, the following may be included in the readme, or it may be more effective to capture in separate documentation/diagrams referenced by the readme.

- how product fits into wider architecture
- internal product architecture
- data structure
- API end points
- monitoring
- error handling
- audit
- user access
- security
- complexity worth documenting
- pipelines


## Architectural Decisions

Architectural decisions are discussed and agreed at the Architectural Design Authority.  A library of Architectural Decision records should be maintained.

## Technical Debt

Technical debt should be captured on the backlog and regularly addressed as part of the development process.

## Code

Effective software engineering can negate the need for additional documentation. Code should follow Defra's digital standards and apply appropriate recognised design patterns.

We should aim to write readable code that requires minimal comments as per Defra's digital standards. However in some scenarios effective code comments can be used to explain complexity.

Well named and considered unit tests can also act as effective documentation.

## Backlog items

Backlog items are captured in the form of backlog items in Azure DevOps.

There is no need to duplicate backlog items in additional documentation.

Items should be should be referenced in associated Pull Requests.

## Sensitive Documentation

Sensitive documentation should be stored within a private location with access restricted to those who need it.

## APIs

### REST

Teams need to document their API using OpenAPI 3.0 specification which can be found [here](https://swagger.io/specification/).

This approach is supported by GDS technical standards which states "The open standards board recommends that government organisations use OpenAPI version 3 to describe RESTful APIs" for more information you can [read](https://www.gov.uk/government/publications/recommended-open-standards-for-government/describing-restful-apis-with-openapi-3)

The OpenAPI file should be kept in a `docs` folder in the top level of the folder and named `openapi.yaml` unless there is a scenario which requires multiple versions being used at the same time in which case the file should be named `openapi-<version>.yaml`. This should be documented in the `API` section of the `Microservice Reference Library`.

An example of how to do this can be found in [ffc-demo-payment-service](https://github.com/DEFRA/ffc-demo-payment-service/tree/master/docs).


### Asynchronous

Teams need to document their API using AsyncAPI specification which can be found [here](https://www.asyncapi.com/docs/specifications/2.0.0)

The AsyncAPI file should be kept in a `docs` folder in the top level of the folder and named `asyncapi.yaml` unless there is a scenario which requires multiple versions being used at the same time in which case the file should be named `asyncapi-<version>.yaml`. This should be documented in the `Events` section of the `Microservice Reference Library`.

An example of how to do this can be found in [ffc-demo-payment-service](https://github.com/DEFRA/ffc-demo-payment-service/tree/master/docs).
