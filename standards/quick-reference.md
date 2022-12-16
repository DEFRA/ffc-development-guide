# Quick reference
This page represents a summary of the core standards teams should follow when delivering services for FFC.

These standards are intended to support a consistent interpretation of the Architecture Vision across the programme allowing as much flexibility as possible.

- follow the Architecture Vision
  - design containerised microservices around business domains and bounded contexts
  - use asynchronous communication between services via Azure Service Bus
  - orchestrate containers using Kubernetes
- follow Defra's wider software development standards
- microservices should be built primarily using Node.js unless there is an evidenced reason not to
- if a web framework is needed, use Hapi.js
- if Node.js is not appropriate, then use .NET
- use PostgreSQL for relational databases
- avoid storing secrets in the cluster where possible, instead use AAD Pod Identity
- follow the agreed test structure in each microservice repository
- use contract testing with Pact to support e2e testing
- use the FFC shared library for CI
- use the FFC Helm chart library as a base for Helm charts
- use the Defra Node.js and .NET docker base images
- follow feature branch development with PR deployments
- work in an agile manner
- keep deployments small and frequent
- do not store live data in any environment other than production
