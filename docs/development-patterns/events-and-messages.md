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

Azure Service Bus [does not yet have an official emulator](https://github.com/Azure/azure-service-bus/issues/223), however one is expected to be released by the end of 2024.

In the absence of an official emulator, teams have two options.

### Use the Azure Service Bus instance in the Sandpit environment

Local development will require a connection to the Azure Service Bus instance in the Sandpit environment.

To avoid collisions between different developers, each developer should have their own set of queues, topics and subscriptions in the Sandpit environment suffixed with their initials.

[A repository](https://github.com/DEFRA/ffc-azure-service-bus-scripts) has been created to support rapid creation and deletion of these resources.

### Use LocalSandbox emulator

An unofficial emulator called [LocalSandbox](https://github.com/mxsdev/LocalSandbox) is available.  This can be used to run a local instance of Azure Service Bus with the majority of the features available in the cloud.

#### Setup

Add a definition for `local-sandbox` to your `docker-compose.yaml` file.

```yaml
services:
  local-sandbox:
    image: localsandbox/localsandbox
    container_name: local-sandbox
```

#### Creating queues, topics and subscriptions

By default, LocalSandBox will create a queue called `default`.  

To create additional topics and subscriptions, you can use `docker-exec` to run the `az` CLI command within the LocalSandbox container.

> Note: LocalSandbox alias the `az` command to `azl`

```bash
docker exec local-sandbox \
  azl servicebus queue create \
  --name test-queue \
  --namespace-name default \
  --resource-group default

docker exec local-sandbox \
  azl servicebus topic create \
  --name test-topic \
  --namespace-name default \
  --resource-group default

docker exec local-sandbox \
  azl servicebus topic subscription create \
  --name test-subscription \
  --namespace-name default \
  --resource-group default \
  --topic-name test-topic
```

If using Docker Compose version 1.30.0 or later, you can use the [`post_start`](https://docs.docker.com/compose/how-tos/lifecycle/#post-start-hooks) lifecycle event to run these commands automatically when the container starts.

```yaml
services:
  local-sandbox:
    image: localsandbox/localsandbox
    container_name: local-sandbox
    post_start:
      command: >
        azl servicebus queue create --name test-queue --namespace-name default --resource-group default
        azl servicebus topic create --name test-topic --namespace-name default --resource-group default
        azl servicebus topic subscription create --name test-subscription --namespace-name default --resource-group default
```

#### Connecting to LocalSandbox

LocalSandbox creates a Service Bus namespace named `default.default.default.localhost`.

Using the `@azure/service-bus` SDK, the following example shows how to connect.

```javascript
import { ServiceBusClient } from '@azure/service-bus'

const connectionString = 'Endpoint=sb://default.default.default.localhost;SharedAccessKeyName=anything;SharedAccessKey=anything;UseDevelopmentEmulator=true'
const client = new ServiceBusClient(connectionString)
```

> Note: The `UseDevelopmentEmulator=true` flag is required to connect to LocalSandbox.  `SharedAccessKeyName` and `SharedAccessKey` can be any value, but must be present.

Once connected, the SDK can be used as normal as if connected to Azure Service Bus.

#### Using across different services

As FCP follows a multi-repository approach, it is likely that services created with different Compose files will need to communicate with each other through a single LocalSandbox instance.  Equally, these services may need to run in isolation.

One way to achieve this is by declaring the LocalSandbox instance in both Compose files and using the `label` property to identify if an instance is already running.

The Compose files must also declare a common network to allow the services to communicate.

##### Service A

```yaml
services:
  ffc-service-a:
    image: ffc-service-a
    networks:
      - ffc-service
    depends_on:
      - local-sandbox

  local-sandbox:
    image: localsandbox/localsandbox
    container_name: local-sandbox
    labels:
      com.docker.compose.service.role: ffc-service-local-sandbox
    networks:
      - ffc-service

networks:
  ffc-service:
    driver: bridge
    name: ffc-service
```

##### Service B

```yaml
services:
  ffc-service-b:
    image: ffc-service-b
    networks:
      - ffc-service
    depends_on:
      - local-sandbox

  local-sandbox:
    image: localsandbox/localsandbox
    container_name: local-sandbox
    labels:
      com.docker.compose.service.role: ffc-service-local-sandbox
    networks:
      - ffc-service

networks:
  ffc-service:
    driver: bridge
    name: ffc-service
```

Each repository can include a `start` script to run `docker compose up` programmatically deciding whether to start the LocalSandbox container based on the presence of the `com.docker.compose.service.role` label.

```bash
if [ -n "$(docker container ls --filter label=com.docker.compose.service.role=ffc-service-local-sandbox --format={{.ID}})" ]; then
  echo "LocalSandbox container already exists, skipping creation"
  args="--scale local-sandbox=0"
fi

docker compose up $@ # $@ passes any arguments to the script
```

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
