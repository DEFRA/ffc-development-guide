# Source code
The default position for FFC is that code should be open source and hosted within GitHub.  

If there is a need for a private repository, for example when source controlling infrastructure as code, that should be controlled within FFC's hosted GitLab.  

## Branching strategies
FFC aims to be a true DevOps programme with capability to deliver value often and rapidly.

To support this, there are two branching strategy options, `Feature branch development` and `Trunk based development`.  Feature branch development should be preferred as it has significantly lower risk and promotes more collaboration.  However, there may be some scenarios where a trunk based development approach can deliver more value.

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
With trunk based development, developers commit code directly to the master branch.

This allows for a more rapid CI/CD approach, but needs to be well supported with robust working practices.  

![Trunk based development](/docs/images/trunk-based-development.png)

#### Pros
- no long running feature branches
- changes can be implemented quickly
- less admin from branch creation and pull requests
- less merge conflicts

#### Cons
- depends on developers running builds locally
- no way to automatically prevent broken builds into master branch
- difficult to support when there is experience variation or remote working

#### Notes
- feature flags and branching by abstraction can support long running changes
- pair programming can reduce risk through real time code review
