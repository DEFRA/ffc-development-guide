# Demo service

The FFC Demo service is a mock digital service to both prove Platform capability and act as an examplar service.  This allows delivery teams see working examples of patterns and standards in practice and replicate them within their own services.

## Service context

The demo service is built around the premise of claiming financial aid in the event property subsides into a mine shaft.  This is an entirely fictional scenario and no Defra service providing this capabililty exists.

The demo service is made up of six microservices, five Node.js and one .NET Core.  There is also a respository that supports local development across all six microservices.

### Web service

[Source code](https://github.com/DEFRA/ffc-demo-web)

### Claim service

[Source code](https://github.com/DEFRA/ffc-demo-claim-service)

### Calculation service

[Source code](https://github.com/DEFRA/ffc-demo-calculation-service)

### Payment service

[Source code (Node.js)](https://github.com/DEFRA/ffc-demo-payment-service)
[Source code (.NET Core)](https://github.com/DEFRA/ffc-demo-payment-service-core)

### Payment web service

[Source code](https://github.com/DEFRA/ffc-demo-payment-web)

### Local development orchestration

[Source code](https://github.com/DEFRA/ffc-demo-development)
