# iac-flux-github-action-runners
Flux Configuration for GitHub action runners

## Developer Guide
To test the changes, ensure that you are on your developer machine and that the context is set correctly to your local instance please amend the following script to use the target branch:

```bash
kubectl config use-context docker-desktop
kubectl create namespace github-runner
kubectl create secret generic pat-token --from-literal=token=<YourPatToken> --namespace github-runner
flux create source git github-runner --url="https://github.com/lsc-sde/iac-flux-github-action-runners" --branch=main --namespace=github-runner
flux create kustomization github-runner-cluster-config --source="GitRepository/github-runner" --namespace=github-runner --path="./cluster/local" --interval=1m --prune=true --health-check-timeout=10m --wait=false
flux create kustomization github-runner-sources --source="GitRepository/github-runner" --namespace=github-runner --path="./sources" --interval=1m --prune=true --health-check-timeout=10m --wait=false
```

This should in turn deploy all of the resulting resources on your local cluster.