# Deploy a service

## Environment control

### Non production

All team members are free to deploy to non-production environments at any time without the need for approvals external to the team.

### Production

Teams are encouraged to deploy to production as early and as often as possible to reduce risk and gain regular feedback from users.

However, there are additional controls around production deployments.

Team members are responsible for deploying to production as long as the following criteria are met.

- An approved change request raised in ServiceNow
- An agreed release time with Release Management Team
- A CCoE resource available to support the release (if required)
- A run book provided to Release Management Team in advance

#### Change requests

Change requests are raised in ServiceNow and take eight working days for approval.  Therefore, it is important to raise change requests as soon as possible.

Once a service has had three successive successful releases to production, a standard change proposal can be submitted.  If approved, teams can deploy to production within the following rules:

- standard change raised in ServiceNow at implement stage specifying microservice and version to be released
- notify Release Management before release
- confirm to Release Management if CCoE resource is required
- provide run book to Release Management
- releases Monday to Friday within working hours only

Teams are encouraged to achieve standard change status as soon as possible to enable more frequent releases to production.

## Workflow

Teams across the FCP typically follow the below process.

Individual delivery teams are free to add their own processes on top of these core values to best fit their context.

## Process for change

> This applies equally to a new feature or bug fix.

1. developer creates a feature branch from main branch
1. developer creates an empty commit with the title of the change and a link to Jira ticket
1. developer opens draft PR for change
1. developer write code in line with team standards, following TDD where appropriate
1. as developer commits, CI builds and tests code
1. CI runs tests to assure code quality
1. when ready, code is reviewed using PR deployment and CI outputs to support
1. when review passes code is merged to main branch
1. CI packages, versions and prepares for deployment to higher environments
1. deployment to non-production environments
1. testing in the non-production environments
1. developer raises change request within ServiceNow to deploy to Production environment
1. Release Management notified of deployment time and resource need
1. developer deploys to Production environment
1. change request is closed once deployment is verified
