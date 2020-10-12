# Azure Service Bus Queues

## Create Managed Identity for your microservice

If not alreay configured add [Managed Identity](managed-identity.md) to your microservice

## Create Queues

* Request the creation of Azure Service Bus queues through your usual Cloud Services support channel
* For each queue that you need, create one for the main branch deploy and one for each of the developers on the workstream to use for local development
* Name of queues should follow the naming convention:
`ffc-<workstream>-<service>-<purpose>` where `<purpose` either denotes the main branch deploy queue e.g. `ffc-demo-payment-dev`, or the developer queue for local development using the developer's initials `ffc-demo-payment-jw`

## Permissions

Request CSC to add the relevant read and/or write permissions to the microservice Managed Identity for the queues you have created.

## PR Queue Provisioning

Queues can be automatically provisioned for each PR by the Jenkins CI pipleine to ensure encapsulation of infrastructure. To enable this create `provision.azure.yaml` in the root directory of your microservice, and populate:

```
resources:
  queues:
    - name: <service>
```

using the `<service>` part of the queue name. For example for the queue `ffc-demo-payment-dev`, `<service>` would be replaced with `payment`.

## Update local development environment

Add the following environment variables to the `.env` file in the root of your microservice.

```
MESSAGE_QUEUE_HOST=<INSERT_VALUE_FROM_AZURE_PORTAL>
MESSAGE_QUEUE_PASSWORD=<INSERT_VALUE_FROM_AZURE_PORTAL>
MESSAGE_QUEUE_SUFFIX=<DEVELOPER_INITIALS>
MESSAGE_QUEUE_USER=RootManageSharedAccessKey
```

**Do not commit the `.env` file to the git repository so make sure to add it to the `.gitignore`**

The `<DEVELOPER_INITIALS>` should match those used in the name of the Service Bus queues cretd for local devlopment.

## Update Docker Compose to use Service Bus environment variables

Add the following environment variables to the `environment` section of the `docker-compose.yaml`:

```
MESSAGE_QUEUE_HOST: ${MESSAGE_QUEUE_HOST:-notset}
MESSAGE_QUEUE_PASSWORD: ${MESSAGE_QUEUE_PASSWORD:-notset}
MESSAGE_QUEUE_USER: ${MESSAGE_QUEUE_USER:-notset}
```

And for every queue you have created also add:

```
<SERVICE>_QUEUE_ADDRESS: ffc-<workstream>-<service>-${MESSAGE_QUEUE_SUFFIX}
```

where `workstream` and `<service>` refer to those parts of the queue name

## Helm Chart

Update the ConfigMap template of the Helm Chart (`helm/<REPO_NAME>/templates/config-map.yaml`) to include the environment variables for the message queue host and every queue you have:

```
MESSAGE_QUEUE_HOST: {{ quote .Values.container.messageQueueHost }}
<SERVICE>_QUEUE_ADDRESS: {{ quote .Values.container.<service>QueueAddress }}
```

Then create default values for these in the `container` section of the Helm Chart values file (`helm/<REPO_NAME>/values.yaml`):

```
container:
  messageQueueHost: dummy
  <service>QueueAddress: <service>
```

## Add Queue Names to App Config

For each message queue create two entries relating to the main branch and PR queue names in App Config using the Azure Portal:

1. Key: `dev/container.<service>QueueAddress`, value: `ffc-<workstream>-<service>-dev`
2. Key: `dev/pr/container.<service>QueueAddress`, value: `ffc-<workstream>-<service>-pr`

Do not add a label to the App Config entries.

## Add messaging code to the microserive

Install Azure Service Bus and Authentication NPM packages: `npm install @azure/ms-rest-nodeauth @azure/service-bus`.

With the Managed Identity bound to your microservice in the Kubernetes cluster, you can then use the Service Bus message queues e.g.:

```
const auth = require('@azure/ms-rest-nodeauth')
const credentials = await auth.loginWithVmMSI({ resource: 'https://servicebus.azure.net' })

const { ServiceBusClient } = require('@azure/service-bus')
const host = <REPLACE_WITH_SERVICE_BUS_HOST_ADDRESS>
const sbClient = ServiceBusClient.createFromAadTokenCredentials(host, credentials)
```

Patterns for using Service Bus are outside of the scope of this guide, but please check current best practice with the FFC Platform Team.
