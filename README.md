# travisci-docker-artifactory

[![Build Status](https://travis-ci.org/Shahbrothers/travisci-docker-artifactory.svg?branch=master)](https://travis-ci.org/Shahbrothers/travisci-docker-artifactory)

`To make this integration work you will need to have running Artifactory-Enterprise/Artifactory-pro/Artifactory SAAS.`

#### Command to test npm package:

* Set npm command line client to work with [Artifactory Npm Registry](https://www.jfrog.com/confluence/display/RTF/Npm+Registry).
    create `.npmrc` file and paste following content to it:
    
```
registry = https://$ARTIFACTORY_URL/api/npm/$ARTIFACTORY_NPM_REPO_NAME/
_auth = $ARTIFACTORY_USER:$ARTIFACTORY_PASSWORD
email = youremail@email.com
always-auth = true
```

* Install dependencies: `npm install`
* Start node Server: `npm start` 
* Test node package: `npm test` 
* Access Application on: [http://localhost:3000](http://localhost:3000)

#### Command to build docker image and push it to Artifactory:

*   Build docker image: ```docker build -t $ARTIFACTORY_DOCKER_REPOSITORY/node-version .```
*   Run docker container: ```docker run -d -p 3000:3000 $ARTIFACTORY_DOCKER_REPOSITORY/node-version```
*   Login to Artifactory docker registry: ```docker login -u ARTIFACTORY_USER -p $ARTIFACTORY_PASSWORD $ARTIFACTORY_DOCKER_REPOSITORY```
*   Push docker image: ```docker push $ARTIFACTORY_DOCKER_REPOSITORY/node-version```

### Steps to build docker images using Circle CI and push it to Artifactory.

##### Step 1:

copy `.travis.yml` to your project

##### Step 2:

Enable your project in [travis-ci](https://travis-ci.org/) .

##### Step 3:

add Environment Variables `ARTIFACTORY_URL`, `ARTIFACTORY_USER`, `ARTIFACTORY_DOCKER_REPOSITORY`, `ARTIFACTORY_PASSWORD`, `DOCEKR_STAGE_REPO` and `DOCEKR_PROD_REPO` in Environment Variables settings of Bitbucket Pipeline.
In this example `$ARTIFACTORY_DOCKER_REPOSITORY=jfrogtraining-docker-dev.jfrog.io`

```
ARTIFACTORY_URL ->  Artifactory URL 
e.g  ARTIFACTORY_URL -> https://mycompany.jforg.io/mycompany

ARTIFACTORY_USER -> Artifactory User which has permission to deploy artifacts.
e.g ARTIFACTORY_USER -> admin

ARTIFACTORY_PASSWORD -> Password for Artifactory User.
e.g ARTIFACTORY_PASSWORD -> password

ARTIFACTORY_DOCKER_REPOSITORY -> Artifactory docker registry to download and push Artifacts.
e.g ARTIFACTORY_DOCKER_REPOSITORY -> docker 

DOCEKR_STAGE_REPO -> Artifactory docker registry to push Artifacts.
e.g DOCEKR_STAGE_REPO -> docker-stage-local

DOCEKR_PROD_REPO -> Artifactory docker registry to push Artifacts.
e.g DOCEKR_PROD_REPO -> docker-prod-local 
```
![screenshot](img/Screen_Shot2.png)

##### Step 4:

You should be able to see published Docker image in Artifactory.
![screenshot](img/Screen_Shot3.png)

## Note: `This solution only supports Artifactory with valid ssl as Travis CI does not support insecure registry`
