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

## Git secret scanning
All FFC repositories are regularly scanned for committed secrets.  A utility repository has been created in support of that.

### GitHub
- [Git secret scanning](https://github.com/DEFRA/ffc-git-secret-scanning)

## Demo service
A demo service has been created to demonstrate FFC's standards and patterns in operation.  The demo service allows developers to see an example of how these patterns work in a "real" implementation.

### GitHub
- [Web service](https://github.com/DEFRA/ffc-demo-web)
- [Claim service](https://github.com/DEFRA/ffc-demo-claim-service)
- [Calculation service](https://github.com/DEFRA/ffc-demo-calculation-service)
- [Payment service (Node.js)](https://github.com/DEFRA/ffc-demo-payment-service)
- [Payment service (.NET Core)](https://github.com/DEFRA/ffc-demo-payment-service-core)
- [Payment web service](https://github.com/DEFRA/ffc-demo-payment-web)
- [Local development support](https://github.com/DEFRA/ffc-demo-development)
