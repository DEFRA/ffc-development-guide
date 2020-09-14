# Developer workflow
Developers across the FFC programme follow the below core workflow.

Individual delivery teams are free to add their own processes on top of these core values to best fit their context.

## Process for change
This applies equally to a new feature or bug fix.

1. developer creates a feature branch from main branch
2. developer creates an empty commit with the title of the change and a link to Jira ticket
3. developer opens draft PR for change
4. developer changes semantic version of application
5. developer write code in line with team standards, following TDD where appropriate
6. as developer commits, CI builds and deploys to isolated PR environment
7. CI runs tests to assure code quality
8. when ready, code is reviewed using PR deployment and CI outputs to support
9.  if review passes code is merged to main branch
10. CI automatically tears down PR deployment
11. CI deploys to showcase environment with sibling services

![Development workflow](developer-workflow.jpg "Development workflow")
