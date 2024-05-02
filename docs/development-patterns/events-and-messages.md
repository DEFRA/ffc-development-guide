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
