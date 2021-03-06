# Shared assets
To better enable developer agility, the FFC Platform team have created several shared assets to reduce the effort needed to deliver a microservice ecosystem within FFC.

Teams are expected to use these shared assets.

## Development guide
The development guide is a library of FFC standards and guides useful to support developers within the FFC programme.

### GitHub
- [Development guide](https://github.com/DEFRA/ffc-development-guide)

## Docker parent images
Docker parent images have been created for Node.js and .NET Core.  These parent images prevent duplication in each microservice by abstracting common layers away from each microservice.

### DockerHub
- [Node.js](https://hub.docker.com/r/defradigital/node)
- [Node.js development](https://hub.docker.com/r/defradigital/node-development)
- [.NET Core](https://hub.docker.com/r/defradigital/dotnetcore)
- [.NET Core development](https://hub.docker.com/r/defradigital/dotnetcore-development)

### GitHub
- [Node.js](https://github.com/DEFRA/defra-docker-node)
- [.NET Core](https://github.com/DEFRA/defra-docker-dotnetcore)

## Jenkins library
The Jenkins shared library abstracts common CI stages away from each microservice repository and ensures that all adequate assurance steps are completed.  The library supports the addition of custom steps at key points in the pipeline.

### GitHub
- [Jenkins library](https://github.com/DEFRA/ffc-jenkins-pipeline-library)

## Helm chart library
The Helm chart library keeps microservice Helm charts dry by abstracting common resource definitions away from each microservice repository.  The packaged chart is hosted in a GitHub repository.

### GitHub
- [Helm chart library](https://github.com/DEFRA/ffc-helm-library)
- [Helm chart repository](https://github.com/DEFRA/ffc-helm-repository)

## Microservice template
A GitHub template repository has been created to allow teams to quickly get started with new microservice.  The template includes all common setup steps such as Docker, Helm charts and Jenkins.

### GitHub
- [Node.js](https://github.com/DEFRA/ffc-template-node)

## .NET Core SonarCloud scanning
Analysing containerised .NET Core microservices with SonarCloud in CI is challenging.  To simplify this process a Docker image has been created to abstract this complexity away from both microservices and the Jenkins library.

### DockerHub
- [.NET Core SonarCloud Analysis](https://hub.docker.com/r/defradigital/ffc-dotnet-core-sonar)

### GitHub
- [.NET Core SonarCloud Analysis](https://github.com/DEFRA/ffc-dotnet-core-sonar)


## Kubernetes configuration
To support creation of new Kubernetes clusters and workstream namespaces, a repostory containing setup defininitions has been created.

### GitHub
- [Kubernetes configuration](https://github.com/DEFRA/ffc-kubernetes-configuration)

## Pact broker
The Pact Broker is a repository for all Pact contracts across all environments for the programme.  It is used as a tool to aide contract testing in both build and deployment phases.

### GitHub
- [Pact broker](https://github.com/DEFRA/ffc-pact-broker)

## Jenkins agent
The Jenkins agent is a Docker image to support a Jenkins instance running in a Kubernetes cluster

### GitHub
- [Jenkins agent](https://github.com/DEFRA/ffc-jenkins-agent)

## Secret scanning
All FFC repositories are regularly scanned for committed secrets.  A utility repository has been created in support of that.

### GitHub
- [Git secret scanning](https://github.com/DEFRA/ffc-git-secret-scanning)
- [Secret scanning](https://github.com/DEFRA/ffc-secret-scanning)

## Demo service
A demo service has been created to demonstrate FFC's standards and patterns in operation.  The demo service allows developers to see an example of how these patterns work in a "real" implementation.

See [Demo service](demo-service.md) for further details.

## Microservice reference library
A repository of all microservices created by the programme.

### Confluence
[Reference library](https://eaflood.atlassian.net/wiki/spaces/FPS/pages/2315288783/Microservice+Reference+Library)

## npm publishing
Simplify publishing packages to npm, including switching between pre-release and full releases.

### GitHub
- [npm publish](https://github.com/DEFRA/ffc-npm-publish)

### DockerHub
- [npm publish](https://hub.docker.com/r/defradigital/ffc-npm-publish)

## Kafka admin client
Simplify viewing and deleting Kafka consumer groups

### GitHub
- [Kafka admin client](https://github.com/DEFRA/ffc-kafka-admin)

## Messaging npm package
npm package to simplify Azure Service Bus sending and consuming in line with FFC standards

### GitHub
- [ffc-messaging](https://github.com/DEFRA/ffc-messaging)

### npm
- [ffc-messaging](https://www.npmjs.com/package/ffc-messaging)

## Events npm package
npm package to simplify Azure Event Hubs sending and consuming in line with FFC standards

### GitHub
- [ffc-events](https://github.com/DEFRA/ffc-events)

### npm
- [ffc-events](https://www.npmjs.com/package/ffc-events)
