# Manual Deployment Pipeline

This Jenkinsfile pipeline will deploy any DryDock application specified by `${APPLICATION_PREFIX}`. 

The user will be presented with a drop-down list of all available `ImageStream` tags in this project. Once the user selects a tag, it will be deployed. 

## Parameters
The pipeline expects 2 parameters:
  * `APPLICATION_PREFIX`: Lower-case abbreviated application name
  * `DB_ENV_NAME`: DB environment definition from `config.yml` in your `flying-squirrel-migrator` Docker image

## Environmental Dependencies
* **ImageStreams**: The pipeline expects to find 2 `ImageStreams` in the current OpenShift project:
  * `${APPLICATION_PREFIX}-web`
  * `${APPLICATION_PREFIX}-flying-squirrel-migrator`

  Both `ImageStreams` must contain **THE SAME** tags.

* **Web Application Objects**, created from `drydock-manual-deployment-pipeline.yml`
  * Delete all existing web application objects: 
    * `$ oc delete all,configmap --selector app=dfip` 
  
  * Create new web application objects: 
    * `$ oc process -f drydock-manual-deployment-template.yml -p APPLICATION_PREFIX=dfip | oc create -f -`

# Create and run the pipeline
```
oc new-build https://github.com/vace117/jenkins-pipelines \
    --strategy=pipeline \
    --name=application-deployment-pipeline \
    --context-dir=drydock-manual-deployment-pipeline \
    --build-env APPLICATION_PREFIX=dfip \
    --build-env DB_ENV_NAME=gen12dvu
```

This pipeline works together with the `drydock-manual-deployment-pipeline.yml`