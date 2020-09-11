# Secrets Management
## Setting up a new FFC git repository
The [standards for creating FFC git repositories](../standards/source-code.md) contains details for configuring new repos to
work with the client-side `detect-secrets` tool.

## Dealing with false positives
`detect-secrets` often identifies false positives (something it thinks is a secret, but is not), which will stop
the developer from committing their changes. We have two strategies for dealing with false positives.

1. For one-time false positives, they can be overridden by commiting with the `--no-verify` flag. This will commit the
change, but any future commits with changes to the file containing the false positive will result in it being detected again.

2. False positives can be permanently ignored by adding them to the secrets baseline. Run the following command and
commit the updated `.secrets.baseline` file:

```
detect-secrets scan --update .secrets.baseline
```

## Excluding files and directories
The `pre-commit` configuration file detailed in the [guide for creating FFC git repositories](../standards/source-code.md)
contains a regex for exlcuding files from the `detect-secrets` scan. This can be updated to exlcude additional
files or directories. Update with caution.
