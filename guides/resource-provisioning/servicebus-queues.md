# Azure Service Bus Queues, Topics and Subscriptions

This guide describes how to configure access to Azure Service Bus from microservices running within Azure Kubernetes Service (AKS).

## Create a microservice Managed Identity

If not already configured, [add an Azure Managed Identity to your microservice](managed-identity.md).

## Request creation of Service Bus queues, topics and subscriptions

### For point-to-point queues

For each queue required by your microservices, you will need:
* A Service Bus queue for each environment
* A Service Bus queue for each of the developers on the workstream in the Sandpit environment to use for local development

Queues should follow the following naming convention:

```
ffc-<workstream>-<identifier>-<purpose>
```

where `<purpose>` either denotes the environment e.g. `ffc-demo-payment-snd`, or the initials of a developer for the local development queues e.g. `ffc-demo-payment-jw`.

Request the creation of the required queues through your usual Cloud Services support channel.

### For topics and subscriptions

For each topic, you will need:
* A Service Bus topic for each environment
* A Service Bus topic for each of the developers on the workstream in the Sandpit environment to use for local development

Subscriptions are created on a topic. For every subscription, you will also need:
* A Service Bus subscription to the topic for each environment
* A Service Bus subscription to each developer topic in the Sandpit environment to use for local development

Topics and subscriptions should follow the following naming convention:

```
ffc-<workstream>-<identifier>-<purpose>
```

where `<purpose>` either denotes the environment e.g. `ffc-demo-reporting-snd`, or the initials of a developer for the local development queues e.g. `ffc-demo-reporting-jw`.

Request the creation of the required topics and subscriptions through your usual Cloud Services support channel.

## Update Managed Identity permissions

For each environment queue, topic and subscription created, read and/or write permissions should be added to the Managed Identites of the microservices that will be using it.

Request Cloud Services to add the relevant permissions to the Managed Identities.

Permissions do not need to be added to Managed Identities for the local development queues.

## PR queue provisioning

Queues, topics and subscriptions can be automatically provisioned for each microservice PR by the Jenkins CI pipleine to ensure encapsulation of infrastructure.

Create a `provision.azure.yaml` file in the root directory of your microservice, and populate with the Service Bus infrastructure that need provisioning:

```
resources:
  queues:
    - name: <queue_identifier>
  topics:
    - name: <topic_identifier>
```

where the `<queue_identifier>` and/or `<topic_identifier>` relates to the part of the queue or topic name described above. For example for the queue `ffc-demo-payment-dev`, `<queue_identifier>` would be replaced with `payment`.

For every topic specified in `provision.azure.yaml`, the CI pipeline will also create a subscription, so subscriptions **do not** need to be explictly spcecified.

Add a `name` entry in the `provision.azure.yaml` for each required queue and topic/subscription.

## Update local development environment

Configure your development environment so that the following environment variables are available for local development:

```
MESSAGE_QUEUE_HOST=<INSERT_VALUE_FROM_AZURE_PORTAL>
MESSAGE_QUEUE_PASSWORD=<INSERT_VALUE_FROM_AZURE_PORTAL>
MESSAGE_QUEUE_USER=RootManageSharedAccessKey
```

Values for `MESSAGE_QUEUE_HOST` and `MESSAGE_QUEUE_PASSWORD` will be found in the Azure Portal.

Also create a variable for each of your developer queues, topics and subscriptions:

```
<IDENTIFIER>_<QUEUE|TOPIC|SUBSCRIPTION>_ADDRESS=ffc-<workstream>-<identifier>-<purpose>
```

where `<IDENTIFIER>` is the same as the `<identifier>` part of the queue/topic/subscription name.

Options for storing these environment variables include:
* A `.env` file in the root of the microservice that uses Service bus, making sure you **Do not commit the `.env` file to the git repository (add it to the `.gitignore`)**
* Your shell `rc` file e.g. `.bashrc` or `.zshrc`

## Update Docker Compose to use Service Bus environment variables

Add the following environment variables to the microserive `environment` section of the `docker-compose.yaml`:

```
MESSAGE_QUEUE_HOST: ${MESSAGE_QUEUE_HOST:-notset}
MESSAGE_QUEUE_PASSWORD: ${MESSAGE_QUEUE_PASSWORD:-notset}
MESSAGE_QUEUE_USER: ${MESSAGE_QUEUE_USER:-notset}
```

And for every queue, topic and subscription you have created also add:

```
<IDENTIFIER>_<QUEUE|TOPIC|SUBSCRIPTION>_ADDRESS: ${<IDENTIFIER>_<QUEUE|TOPIC|SUBSCRIPTION>_ADDRESS:-notset}
```

## Update microservice Helm chart

Update the `ConfigMap` template of the Helm Chart (`helm/<REPO_NAME>/templates/config-map.yaml`) to include the environment variables for the message queue host and every queue, topic and subscription you have created, for example:

```
MESSAGE_QUEUE_HOST: {{ quote .Values.container.messageQueueHost }}
<IDENTIFIER>_<QUEUE|TOPIC|SUBSCRIPTION>_ADDRESS: {{ quote .Values.container.<identifier><Queue|Topic|Subscription>Address }}
```

Then create default values for these in the `container` section of the Helm Chart values file (`helm/<REPO_NAME>/values.yaml`):

```
container:
  messageQueueHost: dummy
  <identifier><Queue|Topic|Subscription>Address: <identifier>
```

## Add queue, topic and subscription names to Azure App Configuration

Azure App Configuration stores values required by the Jenkins CI pipelines. For each queue, topic and subscription create a key-value entry for each environment via the Azure Portal:

* **Key**: `dev/container.<identifier><Queue|Topic|Subscription>Address`; **Value**: `ffc-<workstream>-<identifier>-<environment>`

where `<workstream>` and `<identifier>` refer to those parts of the queue name described above, and `<environment>` denotes the environment e.g. `ffc-demo-payment-dev`

**Do not** add a label to these App Configuration entries.

## Add messaging code to the microservice

Update your microservice code using the relevant Azure authentication and Service Bus SDKs for your language.

Patterns for using a Service Bus in microservice code are outside of the scope of this guide, but you must use the official Azure SDKs to access Managed Identities in AKS and distributed tracing.

An example is shown below for a Node.js microservice, but please check current best practice with the FFC Platform Team.

### Node.js example

Install the Azure authentication and Service Bus SDK NPM packages:

```
npm install @azure/ms-rest-nodeauth @azure/service-bus
```

With the Managed Identity bound to your microservice in the Kubernetes cluster (following the guidence above), you can then request credentials to authenticate with Service Bus e.g.:

```
async function example() {
  const auth = require('@azure/ms-rest-nodeauth')
  const credentials = await auth.loginWithVmMSI({ resource: 'https://servicebus.azure.net' })

  const { ServiceBusClient } = require('@azure/service-bus')
  const host = <REPLACE_WITH_SERVICE_BUS_HOST_ADDRESS>
  const sbClient = ServiceBusClient.createFromAadTokenCredentials(host, credentials)

  // Use sbClient to read/write messages to Service Bus queue
}
```
