# Deploy a service

## Environment control

### Sandpit

All team members are free to deploy to sandpit as required.

### Development

All team members are free to deploy to development as required.

### Test

All team members are free to deploy to test as required.

### Pre-Production

All team members are free to deploy to pre-production as long as deployment to all previous environments has been successful.

### Production

Before a service can be released to production, the following must be in place:

- Architecture design approved at Architectural Design Authority (ADA)
- Service design approved through GRIP
- External IT Health Check (ITHC) passed

For most teams, CCoE are responsible for deploying to production.  However, some more mature teams who have a proven track history of successful releases may be granted permission to deploy to production themselves.

A production release must be supported by:

- An approved change request raised in myIT (ServiceNow)
- An agreed release time with Release Management Team
- A CCoE resource available to support the release
- A run book prepared for the release
- A pending CD pipeline deployment to production

#### Change requests

Change requests are raised in myIT (ServiceNow) and take 10 working days for approval.  Therefore, it is important to raise change requests as soon as possible.

Once a service has had three successive successful releases to production, a standard change proposal can be submitted.  If approved, teams can deploy to production within the following rules:

- standard change raised in myIT at implement stage specifying microservice and version to be released
- notify Release Management by 16:30 the day before release at the latest
- confirm to Release Management if CCoE resource is required
- provide run book to Release Management
- no more than five releases to production per team per day
- releases Monday to Friday within working hours only

Teams are encouraged to achieve standard change status as soon as possible to enable more frequent releases to production.

## Workflow

Teams across the FCP typically follow the below process.

Individual delivery teams are free to add their own processes on top of these core values to best fit their context.

## Process for change

> This applies equally to a new feature or bug fix.

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
11. CI packages, versions and prepares for deployment to higher environments
12. pipeline automatically deploys to Sandpit and Development environments sequentially
13. developer approves deployment to Test environment
14. developer approves deployment to PreProduction environment
17. developer raises change request within myIT to deploy to Production environment
18. developer prepares run book for Production deployment
19. Release Management notified of deployment time and resource need
20. CCoE approves deployment to Production environment
