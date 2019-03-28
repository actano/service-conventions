# Deploying a new service into production

This guide roughly explains how to get your service up and running.

## Steps to do

* Create a `Dockerfile` for your service. There is [a template for it in this repository](./service-template/Dockerfile). Use it as a base. Test whether everything works as intended locally by building the image and running it.
* Create a `Jenkinsfile-k8s`. Again, [the template for it](./service-template/Jenkinsfile-k8s) could be helpful.
* After you checked those into git and pushed, you should be seeing a build starting in our [k8s Jenkins](https://jenkins-k8s.actano.de).
* Create a `charts` directory in the root of your service. Inside of that, create a helm chart with your service's name by using `helm create <serviceName>`.
* Read the Helm and Kubernetes documentation to know what you actually need. Modify the templates and values to your needs.
* Clone the [`rplan-gitops`](https://github.com/actano/rplan-gitops) repository.
* Look at the existing `HelmRelease` configurations. Create a new branch to add your `HelmRelease` resource.
* Push your branch and create a pull request describing why you did those changes.
* Once the PR is merged, you should see your service running in the cluster.
