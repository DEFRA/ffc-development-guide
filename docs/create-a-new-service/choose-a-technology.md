# Choose a technology

## Development language and Framework

The primary technology choice for new FCP services is Node.js with JavaScript deployed to CDP.

Delivery capability in the programme has been optimised for this technology stack.

For scenarios where Node.js is not appropriate, Defra's secondary framework and language is .NET and C#.

> To deviate from Node.js and JavaScript, you must have an evidenced exception approved at SDA.

For full details of Defra's development framework standards, see the [Defra Software Development Standards](https://github.com/DEFRA/software-development-standards/blob/master/standards/development_language_standards.md).

### Node.js

For web based services, [Hapi.js](https://hapi.dev/) as the web framework.  Other web frameworks such as [Express](https://expressjs.com/) are not permitted.

Code must be written in JavaScript linted with [neostandard](https://github.com/neostandard/neostandard).

### .NET

For scenarios where Node.js is not appropriate, .NET can be used with approval of SDA.

CDP and the FCP Platform have been developed to support .NET, however there are not as many supporting tools and libraries given the lower adoption of the technology.

## Compute

CDP provides an abstraction over [AWS Fargate](https://aws.amazon.com/fargate/), this is the primary compute technology for FCP services.

For services still hosted on FCP Platform, [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-gb/services/kubernetes-service/) is the preferred compute technology.

Kubernetes deployments must be managed using [Helm](https://helm.sh/).

## Databases

[MongoDB](https://www.mongodb.com/) is the primary database technology supported by CDP.

CDP also supports [PostgreSQL](https://www.postgresql.org/) for specific uses cases, however teams should use MongoDB in the first instance. 

[Azure Database for PostgreSQL](https://azure.microsoft.com/en-gb/services/postgresql/) is the only database that is fully supported by the FCP Platform.

### Code first migrations

When working with PostgreSQL, [Liquibase](https://www.liquibase.org/) is the approved tool for managing database schema changes.  Both CDP and FCP Platform support Liquibase.

Whilst technologies such as [Sequelize](https://sequelize.org/) and [Entity Framework](https://docs.microsoft.com/en-us/ef/) can be used, they should not be responsible for managing database schema changes.

## Messaging

[SNS](https://aws.amazon.com/sns/) and [SQS](https://aws.amazon.com/sqs/) are the only supported messaging technologies when deploying to CDP.

[Azure Service Bus](https://azure.microsoft.com/en-gb/services/service-bus/) is the only supported messaging technology when deploying to the FCP Platform.

## Caching

[Redis](https://redis.io/) is the approved caching technology.

## Storage

[S3](https://aws.amazon.com/s3/) is the approved storage technology when deploying to CDP.

[Azure Blob Storage](https://azure.microsoft.com/en-gb/services/storage/blobs/), [Azure File Storage](https://azure.microsoft.com/en-gb/services/storage/files/) and [Azure Table Storage](https://azure.microsoft.com/en-gb/services/storage/tables/) are the supported storage technologies when deploying to the FCP Platform.

Blob Storage should be favoured over File Storage where possible due to it's superior local development experience.

## Containers

[Docker](https://www.docker.com/) is the approved container technology.
