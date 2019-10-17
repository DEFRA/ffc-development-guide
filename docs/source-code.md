# Source code
The default position for FFC is that code should be open source and hosted within GitHub.  

If there is a need for a private repository, for example when source controlling infrastructure as code, that should be controlled within FFC's hosted GitLab.  

## Branching strategies
FFC aims to be a true DevOps programme with capability to deliver value often and rapidly.

To support this, there are two branching strategy options that should be used with a source control repository to deliver services.

### Feature branch development
With feature branch development, all feature development should take place in a dedicated branch instead of the master branch. 

This allows multiple developers to work on a feature whilst protecting the master branch. 

![Feature branch development](/docs/images/feature-branch-development.png)

#### Pros
- more control over Master​
- increased opportunity for collaboration through pull requests​
- broken builds can automatically be kept out of Master​
- supports remote working and a experience variation​
- features can be tested in isolated PR environment before approval​

#### Cons
- feature branches can be long lived
- prone to merge conflicts

#### Notes
- well refined user stories can reduce feature branch scope
- good collaboration on how to implement a feature can reduce code review time

### Trunk based development
