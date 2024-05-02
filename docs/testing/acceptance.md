# Acceptance

Acceptance tests should cover key journeys through the application flow via the user interface. These are  defined in a Gherkin syntax (Given, When, Then) and use cucumber and webdriver.io to mimic a scenario from the user perspective. They’re executed against the microservices integrated and running as the complete service with the exception of external services (outside of the platform) which are stubbed.
