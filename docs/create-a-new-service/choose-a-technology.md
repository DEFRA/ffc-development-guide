# Choose a technology

## Development language and Framework

The primary technology choice for new FCP services is Node.js with JavaScript deployed to Azure Kubernetes Service (AKS).

Delivery capability in the programme has been optimised for this technology stack.

For scenarios where Node.js is not appropriate, Defra's secondary framework and language is .NET and C#.

> To deviate from Node.js and JavaScript, you must have a strong case and have received approval from a Principal Developer.

For full details of Defra's development framework standards, see the [Defra Software Development Standards](https://github.com/DEFRA/software-development-standards/blob/master/standards/development_language_standards.md).

### Node.js

For web based services, [Hapi.js](https://hapi.dev/) as the web framework.  Other web frameworks such as [Express](https://expressjs.com/) are not permitted.

Code must be written in JavaScript linted with [StandardJs](https://standardjs.com/).  The use of TypeScript is not permitted.

### .NET

For scenarios where Node.js is not appropriate, .NET can be used with approval of a Principal Developer.

The FCP Platform has been developed to support .NET, however there are not as many supporting tools and libraries given the low adoption of the technology within the programme.

## Databases

[PostgreSQL](https://www.postgresql.org/) is the approved database technology.  It is the only database that is fully supported by the FCP Platform.

[Azure Database for PostgreSQL](https://azure.microsoft.com/en-gb/services/postgresql/) has been deployed to Azure.

### Code first migrations

[Liquibase](https://www.liquibase.org/) is the approved tool for managing database schema changes.  Both CI and CD pipelines have been setup to support Liquibase.

Whilst technologies such as [Sequelize](https://sequelize.org/) and [Entity Framework](https://docs.microsoft.com/en-us/ef/) can be used, they should not be responsible for managing database schema changes.

## Messaging

[Azure Service Bus](https://azure.microsoft.com/en-gb/services/service-bus/) is the preferred messaging technology.

## Caching

[Redis](https://redis.io/) is the approved caching technology.

[Azure Cache for Redis](https://azure.microsoft.com/en-gb/services/cache/) has been deployed to Azure.

## Storage

[Azure Blob Storage](https://azure.microsoft.com/en-gb/services/storage/blobs/), [Azure File Storage](https://azure.microsoft.com/en-gb/services/storage/files/) and [Azure Table Storage](https://azure.microsoft.com/en-gb/services/storage/tables/) are the preferred storage technologies.

Blob Storage should be favoured over File Storage where possible due to it's superior local development experience.

## Containers

[Docker](https://www.docker.com/) is the approved container technology.
