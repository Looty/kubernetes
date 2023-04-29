# Introduction
Datree secures your Kubernetes by blocking the deployment of misconfigured resources.

# Instructions

## Datree Installation
1. `helm repo add datree-webhook https://datreeio.github.io/admission-webhook-datree`
2. `helm repo update`
3. `helm install -n datree datree-webhook datree-webhook/datree-admission-webhook --debug \
--create-namespace \
--set datree.token=<ADD_TOKEN_HERE> \
--set datree.clusterName=$(kubectl config current-context)
`

## Configuring Datree CLI
`datree config set token <ADD_TOKEN_HERE>`

## Run the CLI against any Kubernetes YAML file.
`datree test <path-to-yaml-file>`

## Run manual re-scan
`kubectl delete jobs/scan-job -n datree; kubectl create job --from=cronjob/scan-cronjob scan-job -n datree`