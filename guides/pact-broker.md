# Pact Broker
 
The Pact Broker is an application for sharing of consumer driven contracts and verification results. It is optimised for use with "pacts" (contracts created by the Pact framework), but can be used for any type of contract that can be serialized to JSON. https://github.com/pact-foundation/pact_broker
 
To use the Pact-Broker locally, this repository contains a Dockerized version of the Pact Broker:
https://github.com/pact-foundation/pact-broker-docker
 
## Pact Broker CLI
To install the Pact Broker CLI follow the instructions in the Developer set up guide
 
## Webhooks
 
Documentation:
https://docs.pact.io/pact_broker/webhooks
 
Through the Pact-Broker CLI:
 
Open a command window and navigate to the bin folder of the pact-cli and run the following command replacing the varibles for your deployment. For more information: https://docs.pact.io/pact_broker/client_cli/readme
 
```
pact-broker create-webhook "https://listeners.jenkins-sb.savagebeast.com/job/listeners-acceptance/job/graphql/job/DEVTOOLS-610-test-pact-broker-webhooks/buildWithParameters?os_authType=basic&environment=shared&graphqlHost=shared.graphql.docker.savagebeast.com" --request=POST --broker-base-url=http://localhost:9292 --description='Test 3 Webhook CLI' --contract-content-changed --consumer="Example App" --provider="Example API" --user=username:password --header="accept:application/json"
```
 
Through the Pact-Broker UI:
 
1. Under the column called "Webhook status" click on "Create"
2. Under the heading "Links" Locate "pb:create" in the column called "NON-GET" Click on the "**!**" symbol. 
3. A new window will open to allow the use of HTTP verbs POST, DELETE. 
 
To create the webhook use POST with following body:
 
### Jenkins Example
 
```
{
  "description": "Trigger ffc-demo-claim-service Acceptance Test Job on contract_content_changed event from ffc-demo-payment-service",
  "events": [{
    "name": "contract_content_changed"
  }],
  "request": {
    "method": "POST",
    "url": "",
    "headers": {
      "authorization": "Basic "
    }
  }
}
 
```
 
### Slack Example
 
```
{
  "description": "Trigger to send Slack message on provider-verification-failed event from ffc-demo-payment-service",
  "events": [
    {
      "name": "provider_verification_failed"
    }
  ],
  "request": {
    "method": "POST",
    "url": "",
    "headers": {
      "Content-Type": "application/json"
    },
    "body": {
      "channel": "#generalbuildfailures",
      "username": "webhookbot",
      "text": "Pact verification failed for ${pactbroker.consumerName}/${pactbroker.providerName}: ${pactbroker.pactUrl}",
      "icon_emoji": ":("
    }
  }
}
```
 
To delete a Webhook use the DELETE Verb with an empty body.
 
To test the webhook:
 
1. Select the webhook using "GET"
2. Under the heading "Links" Locate "pb:execute"
 
## Note
 
As of writing this guide, there is an issue with the default url used within the UI for executing the API calls. 
 
The current default url uses the internal IP address. This causes the following error: 
 
`violates the following Content Security Policy directive: "default-src 'self'". Note that 'connect-src' was not explicitly set, so 'default-src' is used as a fallback.`
 
to resolve the issue, the IP address needs replacing with the DNS domain name.
