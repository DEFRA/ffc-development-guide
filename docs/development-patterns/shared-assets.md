# Shared assets

To better enable developer agility, the FCP provide several shared assets to reduce the effort needed to deliver a microservice ecosystem within.

Teams are expected to use these shared assets.

## Development guide

The development guide is a library of FCP standards and guides useful to support developers within the FCP programme.

- [Development guide](https://defra.github.io/ffc-development-guide)

## Docker parent images

Docker parent images have been created for Node.js and .NET.  These parent images prevent duplication in each microservice by abstracting common layers away from each microservice.

### DockerHub

- [Node.js](https://hub.docker.com/r/defradigital/node)
- [Node.js development](https://hub.docker.com/r/defradigital/node-development)
- [.NET](https://hub.docker.com/r/defradigital/dotnetcore)
- [.NET development](https://hub.docker.com/r/defradigital/dotnetcore-development)

### GitHub

- [Node.js](https://github.com/DEFRA/defra-docker-node)
- [.NET](https://github.com/DEFRA/defra-docker-dotnetcore)

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

## .NET SonarCloud scanning

Analysing containerised .NET microservices with SonarCloud in CI is challenging.  To simplify this process a Docker image has been created to abstract this complexity away from both microservices and the Jenkins library.

### DockerHub

- [.NET SonarCloud Analysis](https://hub.docker.com/r/defradigital/ffc-dotnet-core-sonar)

### GitHub

- [.NET SonarCloud Analysis](https://github.com/DEFRA/ffc-dotnet-core-sonar)

## Secret scanning

All FCP repositories are regularly scanned for committed secrets.  A utility repository has been created in support of that.

### GitHub

- [Git secret scanning](https://github.com/DEFRA/ffc-git-secret-scanning)
- [Secret scanning](https://github.com/DEFRA/ffc-secret-scanning)

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

npm package to simplify Azure Service Bus sending and consuming in line with FCP standards

### GitHub

- [ffc-messaging](https://github.com/DEFRA/ffc-messaging)

### npm

- [ffc-messaging](https://www.npmjs.com/package/ffc-messaging)

## Events npm package

npm package to simplify Azure Event Hubs sending and consuming in line with FCP standards

### GitHub

- [ffc-events](https://github.com/DEFRA/ffc-events)

### npm

- [ffc-events](https://www.npmjs.com/package/ffc-events)
