# Events and messages

Where possible, service to service communication will be asynchronous using Azure Service Bus.  

Depending on the pattern being applied the term `event` or `message` may be more appropriate.  For simplicity, for the remainder of this page, the term `event` will be used to refer to both these definitions.

## Avoid tight coupling

Services should be as loosely coupled as possible.  This reduces the dependency a change in one service could have on related services.

Therefore, events should be published to `Topics` as opposed to `Queues` where possible.

## Format

When sending an event to Azure Service Bus, it will be decorated by default broker properties.  For full details on the specification, see the [Microsoft documentation](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messages-payloads)

### Specification

[CloudEvents.io](https://cloudevents.io/) is a specification for describing event data in a common way and should be considered if it is appropriate for the use case.

## Local development with Azure Service Bus

Until recently, Azure Service Bus [did not have an official emulator](https://github.com/Azure/azure-service-bus/issues/223), however [one has now been released](https://mcr.microsoft.com/en-us/artifact/mar/azure-messaging/servicebus-emulator/about)

Teams now have several options for local development.

### Use the Microsoft Azure Service Bus Emulator

The [official emulator](https://github.com/Azure/azure-service-bus-emulator-installer) can be used to run a local instance of Azure Service Bus.

#### Setup

Add a definition for both the emulator and SQL Server to your `docker-compose.yaml` file.

```yaml
services:
  servicebus-emulator:
    container_name: servicebus-emulator
    image: mcr.microsoft.com/azure-messaging/servicebus-emulator
    volumes:
      - "./config.json:/ServiceBus_Emulator/ConfigFiles/Config.json"
    ports:
      - "5672:5672"
    environment:
      SQL_SERVER: sqledge
      MSSQL_SA_PASSWORD: Some-really-strong-password!123
      ACCEPT_EULA: "Y"
    depends_on:
      - sqledge

  sqledge:
    container_name: sqledge
    image: "mcr.microsoft.com/azure-sql-edge"
    environment:
      ACCEPT_EULA: "Y"
      MSSQL_SA_PASSWORD: Some-really-strong-password!123
```

#### Creating queues, topics and subscriptions

A `config.json` file can be used to pre-configure the emulator with queues, topics and subscriptions during startup.

```json
{
  "UserConfig": {
    "Namespaces": [
      {
        "Name": "sbemulatorns",
        "Queues": [
          {
            "Name": "test-queue",
            "Properties": {
              "DeadLetteringOnMessageExpiration": false,
              "DefaultMessageTimeToLive": "PT1H",
              "DuplicateDetectionHistoryTimeWindow": "PT20S",
              "ForwardDeadLetteredMessagesTo": "",
              "ForwardTo": "",
              "LockDuration": "PT1M",
              "MaxDeliveryCount": 10,
              "RequiresDuplicateDetection": false,
              "RequiresSession": false
            }
          }
        ],
        "Topics": [
          {
            "Name": "test-topic",
            "Properties": {
              "DefaultMessageTimeToLive": "PT1H",
              "DuplicateDetectionHistoryTimeWindow": "PT20S",
              "RequiresDuplicateDetection": false
            },
            "Subscriptions": [
              {
                "Name": "test-subscription",
                "Properties": {
                  "DeadLetteringOnMessageExpiration": false,
                  "DefaultMessageTimeToLive": "PT1H",
                  "LockDuration": "PT1M",
                  "MaxDeliveryCount": 10,
                  "ForwardDeadLetteredMessagesTo": "",
                  "ForwardTo": "",
                  "RequiresSession": false
                }
              }
            ]
          }
        ]
      }
    ],
    "Logging": {
      "Type": "File"
    }
  }
}
```

#### Connecting to the emulator

Using the `@azure/service-bus` SDK, the following example shows how to connect.

```javascript
import { ServiceBusClient } from '@azure/service-bus'

const connectionString = 'Endpoint=sb://localhost;SharedAccessKeyName=anything;SharedAccessKey=anything;UseDevelopmentEmulator=true'
const client = new ServiceBusClient(connectionString)
```

> Note: The `UseDevelopmentEmulator=true` flag is required to connect to LocalSandbox.  `SharedAccessKeyName` and `SharedAccessKey` can be any value, but must be present.

Once connected, the SDK can be used as normal as if connected to Azure Service Bus.

#### Using across different services

As FCP follows a multi-repository approach, it is likely that services created with different Compose files will need to communicate with each other through a single emulator instance.  Equally, these services may need to run in isolation.

One way to achieve this is by declaring the emulator instance in both Compose files and using the `label` property to identify if an instance is already running.

The Compose files must also declare a common network to allow the services to communicate.

##### Service A

```yaml
services:
  ffc-service-a:
    image: ffc-service-a
    networks:
      - ffc-service
    depends_on:
      - servicebus-emulator

  servicebus-emulator:
    container_name: servicebus-emulator
    image: mcr.microsoft.com/azure-messaging/servicebus-emulator
    environment:
      SQL_SERVER: sqledge
      MSSQL_SA_PASSWORD: Some-really-strong-password!123
      ACCEPT_EULA: "Y"
    depends_on:
      - sqledge
    labels:
      com.docker.compose.service.role: ffc-service-servicebus-emulator
    networks:
      - ffc-service

  sqledge:
    container_name: sqledge
    image: "mcr.microsoft.com/azure-sql-edge"
    environment:
      ACCEPT_EULA: "Y"
      MSSQL_SA_PASSWORD: Some-really-strong-password!123
    labels:
      com.docker.compose.service.role: ffc-service-servicebus-emulator-sqledge
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
      - servicebus-emulator

  servicebus-emulator:
    container_name: servicebus-emulator
    image: mcr.microsoft.com/azure-messaging/servicebus-emulator
    environment:
      SQL_SERVER: sqledge
      MSSQL_SA_PASSWORD: Some-really-strong-password!123
      ACCEPT_EULA: "Y"
    depends_on:
      - sqledge
    labels:
      com.docker.compose.service.role: ffc-service-servicebus-emulator
    networks:
      - ffc-service

  sqledge:
    container_name: sqledge
    image: "mcr.microsoft.com/azure-sql-edge"
    environment:
      ACCEPT_EULA: "Y"
      MSSQL_SA_PASSWORD: Some-really-strong-password!123
    labels:
      com.docker.compose.service.role: ffc-service-servicebus-emulator-sqledge
    networks:
      - ffc-service

networks:
  ffc-service:
    driver: bridge
    name: ffc-service
```

Each repository can include a `start` script to run `docker compose up` programmatically deciding whether to start the emulator container based on the presence of the `com.docker.compose.service.role` label.

```bash
if [ -n "$(docker container ls --filter label=com.docker.compose.service.role=ffc-service-servicebus-emulator --format={{.ID}})" ]; then
  echo "Service Bus container already exists, skipping creation"
  args="--scale servicebus-emulator=0 --scale sqledge=0"
fi

docker compose up $@ # $@ passes any arguments to the script
```

An example repository using the emulator can be found [here](https://github.com/johnwatson484/service-bus-emulator-poc)

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

Similar to the Microsoft Azure Service Bus Emulator, a single emulator can be used across different services.

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

An example repository using LocalSandbox can be found [here](https://github.com/johnwatson484/message-broker-client)

### Use the Azure Service Bus instance in the Sandpit environment

Local development may use a connection to the Azure Service Bus instance in the Sandpit environment.

To avoid collisions between different developers, each developer should have their own set of queues, topics and subscriptions in the Sandpit environment suffixed with their initials.

[A repository](https://github.com/DEFRA/ffc-azure-service-bus-scripts) has been created to support faster creation and deletion of these resources at scale.

Once created, developers can set a local environment variable with their initials.

```bash
export MESSAGE_SUFFIX=-jw
```

Then reference this in the Docker Compose file of the relevant service.

```yaml
services:
  ffc-service:
    environment:
      MESSAGE_TEST_QUEUE: test-queue${MESSAGE_SUFFIX}
      MESSAGE_TEST_TOPIC: test-topic${MESSAGE_SUFFIX}
      MESSAGE_TEST_SUBSCRIPTION: test-subscription${MESSAGE_SUFFIX}
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

For every topic specified in `provision.azure.yaml`, the CI pipeline will also create a subscription, so subscriptions **do not** need to be explicitly specified.

Add a `name` entry in the `provision.azure.yaml` for each required queue and topic/subscription.

## Standards

- services should guarantee at least once delivery of messages, using the "outbox pattern" where appropriate
- consuming services should be idempotent and safely handle duplicate messages
- teams should ensure a suitable delivery count, retry mechanism, message lock and dead letter queue policy is in place for each entity
- teams should use session queues for request/response message patterns
