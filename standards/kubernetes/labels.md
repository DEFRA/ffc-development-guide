# Labels
Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users, but do not directly imply semantics to the core system. 

Labels can be used to organise and to select subsets of objects. Labels can be attached to objects at creation time and subsequently added and modified at any time. Each object can have a set of key/value labels defined. Each Key must be unique for a given object.

In order to take full advantage of using labels, they should be applied on every resource object within a Helm chart. i.e. all deployments, services, ingresses etc.

## Required labels
Each Helm chart templated resource should have the below labels. Example placeholders are provided for values.

```
metadata:
  labels:
    app: {{ quote .Values.namespace }}
    app.kubernetes.io/name: {{ quote .Values.name }}
    app.kubernetes.io/instance: {{ .Release.Name }}
    app.kubernetes.io/version: {{ quote .Values.labels.version }}
    app.kubernetes.io/component: {{ quote .Values.labels.component }}
    app.kubernetes.io/part-of: {{ quote .Values.namespace }}
    app.kubernetes.io/managed-by: {{ .Release.Service }}
    helm.sh/chart: {{ .Chart.Name }}-{{ .Chart.Version | replace "+" "_" }}
    environment: {{ quote .Values.environment }}
```

**Note** `Deployment` resource objects should have two sets of labels, one for the actual deployment and another for the pod template the deployment manages.

## Selectors
Services selectors should be matched by app and name. Selectors should be consistent otherwise updates to Helm charts will be rejected.
```
selector:
  app: {{ quote .Values.name }}
  app.kubernetes.io/name: {{ quote .Values.name }}
```

## Further reading
More information is available in [Confluence](https://eaflood.atlassian.net/wiki/spaces/FPS/pages/1618214984/Kubernetes+labels)
