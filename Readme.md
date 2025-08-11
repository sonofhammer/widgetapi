# ReadMe

This repository contains a WidgetApi helm chart capable of deploying the app to a Kubernetes cluster.

Since I did not have immediate access to the cluster, ingress is not fully configured and will need to be updated before this can go live. You can however test this helm chart locally 

# Testing the chart locally

## Prerequisites

You will need Docker Desktop installed with [Kubernetes server enabled](https://docs.docker.com/desktop/features/kubernetes/). Other local emulators like minikube and k3s may work as well but were not tested. 

You will need [Helm installed](https://helm.sh/docs/intro/install/).

## Deploying the helm chart

1. In your console, navigate to the directory with Chart.yaml
2. Run `helm install widgetapi .`
3. Observe the notes in the cli, feel free to follow them if you would like to expose the app for local testing on your machine

## Testing the deployed app

This Helm chart has built-in tests that test both the upload and the download functionality of the app

Test the upload by running `helm test widgetapi --filter name=widgetapi-test-upload --logs`

Test download by running `helm test widgetapi --filter name=widgetapi-test-download --logs`

## Removing the app

You can remove the app by running `helm uninstall widgetapi`

# Notes for the engineering team

* Consider expanding the api to include a liveness and a readiness probe so that we can confirm the app is up and alive before we ship traffic to it.
* We currently run only one copy of the app in each environment, depending on load, we may want to see if we should scale the app up or out.

 # Notes for the platform team

 * We'll want to design an approach to auto-increment the version of our helm chart any time we touch the files, either by auto-incrementing the `version` field in `Chart.yaml`. or by running `helm package` in a CI pipeline and passing in version based on a git tag, git commit height, or a conventional commit message format... but all of that would require our own helm registry, so further discussion is needed.
 * Kubernetes v.1.30 is EOL as of June 28th 2025, we should look into updating our clusters to a later version
 * The template has the values.yaml file all ready for ingress, we need to add them and test them in our cluster. 
 * This helm chart does not set up a namespace and will deploy to default namespace unless you pass in the appropriate args during deployment. We need to come to a consensus on how we plan on using namespaces.
 * built-in helm testing is a bit buggy, and will not run all test suites without filters, we should explore terratest or helm-unittest as alternatives.
 * We will need to design a deployment approach using our CI/CD tooling.