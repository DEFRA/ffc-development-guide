# Developer workflow
Developers across the FFC programme follow the below core workflow.

Individual delivery teams are free to add their own processes on top of these core values to best fit their context.

## Process for change
This applies equally to a new feature or bug fix.

1. developer creates a feature branch from main branch
2. developer creates an empty commit with the title of the change and a link to Jira ticket
3. developer opens draft PR for change
4. developer changes semantic version of application
5. developer follows TDD and ensures behaviour is covered by automated tests
7. as developer commits, CI builds and deploys to isolated PR environment
8. CI runs tests to assure code quality
9. when ready, code is reviewed using PR deployment and CI outputs to support
10. if review passes code is merged to main branch
11. CI automatically tears down PR deployment
12. CI deploys to showcase environment with sibling services
