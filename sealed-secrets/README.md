# Introduction
A Kubernetes controller and tool for one-way encrypted Secrets

# Instructions

## Kubeseal Installation
1. `helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets`
2. `helm repo update`
3. `helm install sealed-secrets -n kube-system --set-string fullnameOverride=sealed-secrets-controller sealed-secrets/sealed-secrets`

## Usage

Create a json/yaml-encoded Secret somehow:
- `echo -n bar | kubectl create secret generic mysecret --dry-run=client --from-file=foo=/dev/stdin -o yaml >sealed-secrets/mysecret.yaml`

Load it to kubeseal (default is JSON)
- `kubeseal <sealed-secrets/mysecret.yaml -o yaml >sealed-secrets/mysealedsecret.yaml`

At this point mysealedsecret.json is safe to upload to Github,
post on Twitter, etc.

Eventually:
- `kubectl create -f sealed-secrets/mysealedsecret.yaml`

Profit!
- `kubectl get secret mysecret`
- `kubectl get secret mysecret mysecret -o jsonpath='{.items[0].data.foo}' | base64 -d`

## Advanced
Fetch current certification: (The renewal time is 30 days by default)
- `kubeseal --fetch-cert`

How can I do a backup of my SealedSecrets?
- If you do want to make a backup of the encryption private keys, it's easy to do from an account with suitable access:
- `kubectl get secret -n kube-system -l sealedsecrets.bitnami.com/sealed-secrets-key -o yaml >main.key`

To restore from a backup after some disaster, just put that secrets back before starting the controller - or if the controller was already started, replace the newly-created secrets and restart the controller:
- `kubectl apply -f main.key`
- `kubectl delete pod -n kube-system -l name=sealed-secrets-controller`