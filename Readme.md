# Conventions for services

We want to meet a certain standard when developing and deploying services.

## Nodejs

Don't use `babel-node` in production. Use `yarn` instead of `npm`.

* We use nodejs LTS version 10.
* Put code into a `src` directory
* `yarn build` should build into a `lib` folder
* `yarn lint` should lint all files
* `yarn test` should execute all tests
* `yarn test:unit` can execute all unit tests
* `yarn test:rest-api` can execute all rest api tests
* package.json `main` should point to `lib/index.js`
* Private packages should have `private:true` in their package.json
* `yarn start` should start the server by using the built code (production use)
* `yarn start:dev` can be used to start the server from `src` with `babel-node`
* `yarn start:watch` can be used to start the server from `src` with a filewatcher (such as `babel-watch`)

## Requirements

In order to deploy anything to Kubernetes, it needs to be packaged in a docker image.
This image should be build automatically and pushed after successful tests by the CI system.

To deploy to Kubernetes, the service repository should contain its Helm chart.
This chart must be referenced as a `HelmRelease` resource in the [rplan-gitops](https://github.com/actano/rplan-gitops) respository.

A Kubernetes `Service` resource must exist to expose the pods to the cluster.
It should contain [Ambassador mapping annotation](https://www.getambassador.io/reference/mappings/).

## Infrastructure conventions

* Write a `Dockerfile`
* Write a `Jenkinsfile-k8s` (see [jenkins-k8s](https://github.com/actano/jenkins-k8s))
* Artifact: Docker image, image tag should be `<branch>-<GIT_SHA>`
* Helm chart in service repository
* HelmRelease in [gitops repository](https://github.com/actano/rplan-gitops)
    * For automated releases, use
        ```
        flux.weave.works/tag.chart-image: "glob:<branchname>-*"
        ```
* Secret templates in GitOps repository

## Application specific conventions

* Log in the bunyan JSON format. For node, you should use ([`@rplan/logger`](https://github.com/actano/rplan-logger))
* Shut the server down gracefully by handling SIGTERM and SIGINT
* Use [actano eslint config](https://github.com/actano/javascript)
* Expose `/health` endpoint which checks external dependencies (e.g. working database connection)
* Expose `/metrics` endpoint (for node, use [`prom-client`](https://www.npmjs.com/package/prom-client))

## Versioning of the REST-API
In order to support breaking changes, we will add versioning to our REST-API. 
The details still have to be specified here.

### General idea
- A service will provide multiple versions of its REST-API. 
- The requested API-version will be part of the URL.
- If a service needs to make breaking changes to the REST-API, a new  API version should be provided.
- Older/deprecated versions should still be supported for some time in order to allow a migration. 
- At some point support for an older version will be dropped.

## Git hooks 

* Do not enforce git hooks. 
* Optional git hooks are fine.
* The ci system is testing everything anyway, so git hooks should be an individual developer settings.

## Templates

The [service-template](./service-template) directory contains templates to get a starting point for
introducing a new service:
* [Jenkinsfile-k8s](./service-template/Jenkinsfile-k8s)
* [Dockerfile](./service-template/Dockerfile)

These templates could outdate quickly though. To get a living standard, look into actual service repositories.
