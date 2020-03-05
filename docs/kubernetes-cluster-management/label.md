# Overview
Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users, but do not directly imply semantics to the core system. Labels can be used to organise and to select subsets of objects. Labels can be attached to objects at creation time and subsequently added and modified at any time. Each object can have a set of key/value labels defined. Each Key must be unique for a given object.

In order to take full advantage of using labels, they should be applied on every resource object.

# Kubernetes Labels

For each cluster we should use the below labels:
    app.kubernetes.io/name
    app.kubernetes.io/instance
    app.kubernetes.io/version
    app.kubernetes.io/component
    app.kubernetes.io/part-of
    app.kubernetes.io/managed-by
 
* A description and example of these can be seen in the confluence page [here](https://eaflood.atlassian.net/wiki/spaces/FPS/pages/1618214984/Kubernetes+labels)