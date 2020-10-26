# API Standards

Teams must document the API of all microservices they develop. Both REST APIs such as HTTP and Asynchronous APIs such as messages and events. Links to the documents mentioned below should also be included in the [Microservice Reference Library](https://eaflood.atlassian.net/wiki/spaces/FPS/pages/2318533447/Demo+payment+service)

## REST

Teams need to document their API using OpenAPI 3.0 specification which can be found [here] (https://swagger.io/specification/).

This approach is supported by GDS technical standards which states "The open standards board recommends that government organisations use OpenAPI version 3 to describe RESTful APIs" for more information you can [read] (https://www.gov.uk/government/publications/recommended-open-standards-for-government/describing-restful-apis-with-openapi-3)

The OpenAPI file should be kept in a ```docs``` folder in the top level of the folder and named ```openapi.yaml``` unless there is a scenario which requires multiple versions being used at the same time in which case the file should be named ```openapi-<version>.yaml```. This should be documented in the ```API``` section of the ```Microservice Reference Library```.

An example of how to do this can be found in [ffc-demo-payment-service](https://github.com/DEFRA/ffc-demo-payment-service/tree/master/docs).


## Asynchronous

Teams need to document their API using AsyncAPI specification which can be found [here] (https://www.asyncapi.com/docs/specifications/2.0.0)

The AsyncAPI file should be kept in a ```docs``` folder in the top level of the folder and named ```asyncapi.yaml``` unless there is a scenario which requires multiple versions being used at the same time in which case the file should be named ```asyncapi-<version>.yaml```. This should be documented in the ```Events``` section of the ```Microservice Reference Library```.

An example of how to do this can be found in [ffc-demo-payment-service](https://github.com/DEFRA/ffc-demo-payment-service/tree/master/docs).