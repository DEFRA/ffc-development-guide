# Configuration
Configuration for an application running in a pod should be passed to the pod via a `ConfigMap` Kubernetes resource.

The configuration values should be stored in [Azure App Configuration](https://azure.microsoft.com/en-gb/services/app-configuration/) and injected via delivery pipeline as documented in the [Configuration Management](../configuration-management.md) page.

Sensitive values should be passed to the pod via a [`Secret`](secrets.md) Kubernetes resource.
