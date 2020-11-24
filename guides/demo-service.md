# Demo service

The FFC Demo service is a mock digital service to both prove Platform capability and act as an examplar service.  This allows delivery teams see working examples of patterns and standards in practice and replicate them within their own services.

## Service context

The demo service is built around the premise of claiming financial aid in the event property subsides into a mine shaft.  This is an entirely fictional scenario and no Defra service providing this capabililty exists.

The demo service is made up of six microservices, five Node.js and one .NET Core.  There is also a respository that supports local development across all six microservices.

### Web service

Front end web application where user follows a claim creation journey and submits a new claim for payment. 
Claim data is cached using Redis between questions and submitted to an Azure Service Bus queue for the Claim service.

[Source code](https://github.com/DEFRA/ffc-demo-web)

### Claim service

Receives a new claim submission and persists claim data in a PostgreSQL database. 
Publishes new claim validated message to two Azure Service Bus queues for the Payment and Calculation services.

[Source code](https://github.com/DEFRA/ffc-demo-claim-service)

### Calculation service

Receives a valid claim message and calculates a monetary value based on claim data.
Publishes claim value to Azure Service Bus queue for Payment Service.

[Source code](https://github.com/DEFRA/ffc-demo-calculation-service)

### Payment service

Generates a payment schedule from valid claim message and persists in a PostgreSQL database.
Also persists associated payment value received from calcuated message.

Both a Node.js and .NET Core version of this service exist.

[Source code (Node.js)](https://github.com/DEFRA/ffc-demo-payment-service)
[Source code (.NET Core)](https://github.com/DEFRA/ffc-demo-payment-service-core)

### Payment web service

Front end web application for internal users.  Requests payment schedule data from Payment service via HTTP (Node.js only).

[Source code](https://github.com/DEFRA/ffc-demo-payment-web)

### Local development orchestration

Easy running and debugging of all demo services locally within single Docker network.

[Source code](https://github.com/DEFRA/ffc-demo-development)

## Patterns and standards

The demo service is built following FFC standards and patterns.  However, for reasons of time constraint, not every service has every pattern in place.
Below is a reference of where patterns and standards can be found.

### Directory structure
- All

### Tests
#### Unit
- Web
- Payment (Node.js)
- Payment (.NET Core)
- Calculation
- Payment web

#### Narrow integration
- Web
- Payment (Node.js)
- Calculation
- Payment web

#### Local integration
- Web - Redis, Azure Service Bus
- Claim - Azure Service Bus, PostgreSQL
- Payment (Node.js) - Azure Service Bus, PostgreSQL
- Calculation - Azure Service Bus

#### Contract - Provider Async
- Claim

#### Contract - Consumer Async
- Payment (Node.js)

#### Contract - Provider HTTP
- Payment (Node.js)

#### Contract - Consumer HTTP
- Payment web

#### Acceptance
- Web

#### Security
- Web
- Payment web

#### Accessibility
- Web

#### .NET Core Snyk setup
- Payment (.NET Core)

### Docker
- All

### Docker Compose
- All

### FFC CI Pipeline Jenkinsfile
- All

### Liquibase database migrations
- Claim
- Payment (Node.js)
- Payment (.NET Core)

### Messaging
#### Send message to queue
- Web
- Claim
- Calculation

#### Receive message from queue
- Claim
- Calculation
- Payment (Node.js)
- Payment (.NET Core)

#### Outbox pattern
- Claim

#### Dead lettering
- Calcuation

#### Completing message
- Claim
- Calculation
- Payment (Node.js)
- Payment (.NET Core)

#### Abandon message
- Claim
- Calculation
- Payment (Node.js)
- Payment (.NET Core)

### Webpack
- Web
- Payment web

### Application Insights
- All
