# Conventions for services

We want to meet a certain standard when developing and deploying services.

* Use rplan logger module ([`@rplan/logger`](https://github.com/actano/rplan-logger)) for log output
* Shut the server down gracefully by handling SIGTERM and SIGINT
* Use [actano eslint config](https://github.com/actano/javascript)
* Expose `/health` endpoint which checks external dependencies (e.g. working database connection)
* Expose `/metrics` endpoint (use [`prom-client`](https://www.npmjs.com/package/prom-client))
* Write a `Dockerfile` (Docker 17)
* Write a `Jenkinsfile-k8s` (see [jenkins-k8s](https://github.com/actano/jenkins-k8s))

* Artifact: Docker image
    * For `master` branch: tag=`1.0.0-<buildNumber>`
    * For other branches: tag=`<branch>-<GIT_SHA>`
* Helm chart in service repsitory
* HelmRelease in [gitops repository](https://github.com/actano/rplan-gitops)
    * For `master` branch use
        ```
        flux.weave.works/tag.chart-image: "semver:~1.0.0"
        ```
    * For other branches use
        ```
        flux.weave.works/tag.chart-image: "glob:<branchname>-*"
        ```
* Secret templates in GitOps repository

## SemVer for production

We are making use of [semantic versioning](https://semver.org/) to have a strictly monotonically increasing number as the Docker image tag.
This enables us to revert to older versions because this will still create new image tags.

## Templates

The [service-template](./service-template) directory contains templates to get a starting point for
introducing a new service:
* [Jenkinsfile-k8s](./service-template/Jenkinsfile-k8s)
* [Dockerfile](./service-template/Dockerfile)
