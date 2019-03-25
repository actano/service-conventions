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
* HelmRelease in gitops repository
* Secret templates in GitOps repository
