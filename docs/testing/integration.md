# Integration

Integration tests verify the interactions of several components, rather than a single unit. They are classified further into narrow and local integration tests. 

### Narrow integration

Narrow integration tests should be considered as tests that are bigger than unit tests, but smaller than integration involving other services or modules.

For example where a unit test would traditionally mock a web server, a narrow integration test may start the web server albeit without exposing any ports externally.

Narrow integration tests should mock immediate dependencies only.

### Local integration

Local integration tests should cover functionality that integrates with dependencies that are local to the service, for example message queues and databases.

Rather than mocking the database or queue, local integration tests could use a database or message queue container.  This reduces the need for mocks, increasing developer agility, reducing test complexity and a higher level of confidence in tests.

Where integration with third party services are needed, not covered by contract tests, then tests should be make use use of Docker containers and stubs to reduce the need for mocking.
