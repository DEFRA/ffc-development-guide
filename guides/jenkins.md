# Jenkins

## Using Jenkins locally

### Installation

It is recommended that Jenkins is run locally within the Blue Ocean Docker image. The Blue Ocean image is recommended by Jenkins as it comes with Blue Ocean already configured. Blue Ocean provides a more user friendly experience which may suit developers new to Jenkins.

The below command will pull the image and run the container.

```
docker run \
  -u root \
  --rm \
  -d \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v <PATH_TO_LOCAL_GIT_REPOS>:/repos:ro \
  jenkinsci/blueocean
```

`-v <PATH_TO_LOCAL_GIT_REPOS>:repos:ro`

The above line should be amended to specify the file path of your local git repositories. This will allow you to run builds from your local repository.

An example could be ` -v /c/users/ddts_220606/source/repos:\repos:ro`

This [official guide](https://jenkins.io/doc/book/installing/) includes instructions for how to setup access credentials to the Jenkins container.

### Run build from local repository

The following assumes you have an existing repository containing a Jenkinsfile.

- select `New Item` from the main menu
- enter a project name and select `Pipeline`
- within the `Pipeline` section, select `Pipeline script from SCM` as the `Definition`
- enter `Git` as the `SCM`
- enter the local repository path into the `Repository URL` using the mount configured above, for example `file:////repos/repo1`
- Set the `Script Path` to where the Jenkinsfile can be found in the repo
- save the project
- select `Build Now`

### Troubleshooting

If an exception of type `Jenkins CI Pipeline Scripts not permitted to use method groovy.lang.GroovyObject` is thrown, then complete the following steps.

- select `Manage Jenkins` from the main menu
- select `In-process Script Approval`
- select `Approve` for Groovy
