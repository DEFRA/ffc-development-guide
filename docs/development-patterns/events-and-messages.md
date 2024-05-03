# Events and messages

Where possible, service to service communication will be asynchronous using Azure Service Bus.  

Depending on the pattern being applied the term `event` or `message` may be more appropriate.  For simplicity, for the remainder of this page, the term `event` will be used to refer to both these definitions.

## Use Azure Service Bus

Services should use Azure Service Bus as an event broker.  This is because the FCP CI pipeline is relatively mature in terms of dynamically provisioning Azure Service Bus infrastructure to support the development cycle and the team have strong experience with it's use.

## Avoid tight coupling

Services should be as loosely coupled as possible.  This reduces the dependency a change in one service could have on related services.

Therefore, events should be published to `Topics` as opposed to `Queues` where possible.

## Format

When sending an event to Azure Service Bus, it will be decorated by default broker properties.  For full details on the specification, see the [Microsoft documentation](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messages-payloads)

### Specification

[CloudEvents.io](https://cloudevents.io/) is a specification for describing event data in a common way and should be considered if it is appropriate for the use case.

### Mandatory custom properties

All FCP events from any service must also declare the following custom properties when publishing an event.

- `type` - the type of event, this should be prefixed with reverse DNS and describe the object and event occurring on that object in the format:
`<reverse DNS>.ffc.<object>.<event>`. 
  For example, `uk.gov.ffc.demo.claim.validated`
- `source` - the service the event originated from, eg `ffc-demo-web`
- `subject (optional)` - the subject which will give consumers context, for example when the body cannot be consistently read or contains blob data

These additional properties are based on values included in the [CloudEvents](https://cloudevents.io/) specification that are not included in the Azure Service Bus envelope.  Refer to the Cloud Events specification for further examples of their usage.

## Authentication

A [Managed Identity](identity.md) should used to authenticate microservices with Service Bus when it is deployed to Azure.  However, for local development outside of Kubernetes, a connection string can be used.

An [`ffc-messaging`](https://www.npmjs.com/package/ffc-messaging) npm package has been created to simplify this setup for teams.

## Local development

Azure Service Bus [does not yet have an emulator](https://github.com/Azure/azure-service-bus/issues/223), so local development will require a connection to the Azure Service Bus instance in the Sandpit environment.

To avoid collisions between different developers, each developer should have their own set of queues, topics and subscriptions in the Sandpit environment suffixed with their initials.

[A repository](https://github.com/DEFRA/ffc-azure-service-bus-scripts) has been created to support rapid creation and deletion of these resources.

## Naming conventions

The naming convention for queues, topics and subscriptions should be as follows:

- all entities should be prefixed with `ffc-<service>` where `<service>` is the name of the service, eg `ffc-demo`
- topics and queues should be named to denote the entity they are related to, eg `ffc-demo-payment` or `ffc-demo-claim`
- topics and queues should be suffixed with the environment they are deployed to, eg `ffc-demo-payment-dev` or `ffc-demo-payment-prd`
- subscriptions should match the name of the consuming microservice, eg `ffc-demo-payment-web`

## Pull Requests provisioning

Queues, topics and subscriptions can be automatically provisioned for each microservice PR by the Jenkins CI pipeline to ensure encapsulation of infrastructure.

Create a `provision.azure.yaml` file in the root directory of your microservice, and populate with the Service Bus infrastructure that need provisioning:

```yaml
resources:
  queues:
    - name: <queue_identifier>
  topics:
    - name: <topic_identifier>
```

where the `<queue_identifier>` and/or `<topic_identifier>` relates to the part of the queue or topic name described above. For example for the queue `ffc-demo-payment-dev`, `<queue_identifier>` would be replaced with `payment`.

For every topic specified in `provision.azure.yaml`, the CI pipeline will also create a subscription, so subscriptions **do not** need to be explictly spcecified.

Add a `name` entry in the `provision.azure.yaml` for each required queue and topic/subscription.

## Standards

- services should guarantee at least once delivery of messages, using the "outbox pattern" where appropriate
- consuming services should be idempotent and safely handle duplicate messages
- teams should ensure a suitable delivery count, retry mechanism, message lock and dead letter queue policy is in place for each entity
- teams should use session queues for request/response message patterns
