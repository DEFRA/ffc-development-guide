## Unit

Unit tests should be used to: 

- target functionality that is not already covered through contract or integration testing
- add extra assurance or visibility to specific functionality, for example an application that runs a series of calculations to determine a monetary value, may benefit from individual unit tests for each calculation.
- code should be written so that where unit tests are required, mocking is not required if possible or is minimal.
- unit tests should mock immediate dependencies only.
