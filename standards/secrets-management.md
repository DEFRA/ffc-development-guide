# Secrets Management
## Client-side git secret detection

All developers are required to [install detect-secrets](../guides/developer-laptop-setup/install-detect-secrets.md)
when working on open-source FFC git repositories in GitHub.

## Server-side git secret detection

Jenkins will scan for potential secrets in all open-source FFC git repositories in GitHub.

Any secrets detected will be reported to the `#secret-detection` Slack channel in the `ffc-notifications` workspace.
