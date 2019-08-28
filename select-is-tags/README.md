# Read ImageStream Tags

This Jenkinsfile will read in a list of `ImageStream` tags for the user to select from as part of a manual input stage.

You can test this out by creating a few tags in your project.

```
oc new-project jenkins-test
oc tag docker.io/openshift/hello-openshift:v3.7 hello-openshift:v3.7
oc tag docker.io/openshift/hello-openshift:v3.8 hello-openshift:v3.8
oc tag docker.io/openshift/hello-openshift:v3.9 hello-openshift:v3.9
oc new-build https://github.com/pittar/jenkins-pipelines --context-dir=select-is-tags --build-env IMAGE_STREAM=hello-openshift --build-env PROJECT_NAME=jenkins-test --name=pipelines
```
