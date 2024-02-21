# Git secrets

When working in the open it is important to ensure that no secrets are committed to a public repository. 

All developers setup [`detect-secrets`](../local-development-setup//install-detect-secrets.md) to scan their code for secrets before committing.

## Dealing with false positives
`detect-secrets` often identifies false positives (something it thinks is a secret, but is not), which will stop
the developer from committing their changes. We have two strategies for dealing with false positives.

1. For one-time false positives, they can be overridden by committing with the `--no-verify` flag. This will commit the
change, but any future commits with changes to the file containing the false positive will result in it being detected again.

2. False positives can be permanently ignored by adding them to the secrets baseline. Run the following command and
commit the updated `.secrets.baseline` file:

```bash
detect-secrets scan --update .secrets.baseline
```
