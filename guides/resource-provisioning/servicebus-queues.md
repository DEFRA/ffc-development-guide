# Azure Service Bus Queues

This guide describes how to configure access to Azure Service Bus from microservices running within Azure Kubernetes Service (AKS).

## Create a microservice Managed Identity

If not already configured, [add an Azure Managed Identity to your microservice](managed-identity.md).

## Request creation of Service Bus queues

For each queue required by your microservices, you will need:
* A Service Bus queue for each environment
* A Service Bus queue for each of the developers on the workstream in the Sandpit environment to use for local development

Queues should follow the following naming convention:

```
ffc-<workstream>-<identifier>-<purpose>
```

where `<purpose>` either denotes the environment e.g. `ffc-demo-payment-dev`, or the initials of a developer for the local development queues e.g. `ffc-demo-payment-jw`.

Request the creation of the required queues through your usual Cloud Services support channel.

## Update Managed Identity permissions

For each main branch queue created, read and/or write permissions should be added to the Managed Identites of the microservices that will be using the queues.

Request Cloud Services to add the relevant permissions to the Managed Identities.

Permissions do not need to be added to Managed Identities for the local development queues.

## PR queue provisioning

Queues can be automatically provisioned for each microservice PR by the Jenkins CI pipleine to ensure encapsulation of infrastructure.

Create a `provision.azure.yaml` file in the root directory of your microservice, and populate with the queues that need provisioning:

```
resources:
  queues:
    - name: <identifier>
```

where the `<identifier>` relates to the part of the queue name described above. For example for the queue `ffc-demo-payment-dev`, `<identifier>` would be replaced with `payment`. Add a `name` entry for each required queue.

## Update local development environment

Configure you development environment so that the following environment variables are available for local development:

```
MESSAGE_QUEUE_HOST=<INSERT_VALUE_FROM_AZURE_PORTAL>
MESSAGE_QUEUE_PASSWORD=<INSERT_VALUE_FROM_AZURE_PORTAL>
MESSAGE_QUEUE_SUFFIX=<DEVELOPER_INITIALS>
MESSAGE_QUEUE_USER=RootManageSharedAccessKey
```

Values for `MESSAGE_QUEUE_HOST` and `MESSAGE_QUEUE_PASSWORD` will be found in the Azure Portal, and the `<DEVELOPER_INITIALS>` should match those used in the name of the Service Bus queues created for local devlopment.

Options for storing these environment variables include:
* A `.env` file in the root of the microservice that uses Service bus, making sure you **Do not commit the `.env` file to the git repository (add it to the `.gitignore`)**
* Your shell `rc` file e.g. `.bashrc` or `.zshrc`

## Update Docker Compose to use Service Bus environment variables

Add the following environment variables to the `environment` section of the `docker-compose.yaml`:

```
MESSAGE_QUEUE_HOST: ${MESSAGE_QUEUE_HOST:-notset}
MESSAGE_QUEUE_PASSWORD: ${MESSAGE_QUEUE_PASSWORD:-notset}
MESSAGE_QUEUE_USER: ${MESSAGE_QUEUE_USER:-notset}
```

And for every queue you have created also add:

```
<IDENTIFIER>_QUEUE_ADDRESS: ffc-<workstream>-<identifier>-${MESSAGE_QUEUE_SUFFIX}
```

where `<workstream>` and `<identifier>` refer to those parts of the queue name described above.

## Update microservice Helm chart

Update the `ConfigMap` template of the Helm Chart (`helm/<REPO_NAME>/templates/config-map.yaml`) to include the environment variables for the message queue host and every queue you have created:

```
MESSAGE_QUEUE_HOST: {{ quote .Values.container.messageQueueHost }}
<IDENTIFIER>_QUEUE_ADDRESS: {{ quote .Values.container.<identifier>QueueAddress }}
```

Then create default values for these in the `container` section of the Helm Chart values file (`helm/<REPO_NAME>/values.yaml`):

```
container:
  messageQueueHost: dummy
  <identifier>QueueAddress: <identifier>
```

## Add queue names to Azure App Configuration

Azure App Configuration stores values required by the Jenkins CI pipelines. For each message queue create a key-value entry for each environment via the Azure Portal:

* **Key**: `dev/container.<identifier>QueueAddress`; **Value**: `ffc-<workstream>-<identifier>-<environment>`

where `<workstream>` and `<identifier>` refer to those parts of the queue name described above, and `<environment>` denotes the environment e.g. `ffc-demo-payment-dev`

**Do not** add a label to these App Configuration entries.

## Add messaging code to the microservice

Update your microservice code using the relevant Azure authentication and Service Bus SDKs for your language.

Patterns for using a Service Bus in microservice code are outside of the scope of this guide, you must use the official Azure SDKs to access Managed Identities in AKS and distributed tracing.

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
