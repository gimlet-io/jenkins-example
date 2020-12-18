# jenkins-example

Example repo for using Gimlet CLI with Jenkins

Check the Gimlet manifest file in `.gimlet/` and the `Jenkinsfile`

## GitOps repo

https://github.com/gimlet-io/jenkins-example-gitops

## Forking this repo

If you fork the repo, you must add Jenkins credentials called `Github`

Create a Personal Access Token in your settings and add the `repo` scope for the GitOps repo writing, and the `write:packages` scope for Github Container Registry push access.
