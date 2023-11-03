# flux-kind

## Prerequisites

Install all required tools with one command. Check out the `Brewfile` to see what will be installed/upgraded.

```
brew bundle install
```

## Stop collecting my data...

```
ctlptl analytics opt out
skaffold config set --global collect-metrics false
```

## Start Kind Cluster

```
podman machine init \
  --cpus 8 --memory 24576 --disk-size 100 \
  --timezone 'Etc/UTC' --rootful && \
podman machine start && \
export DOCKER_HOST="unix://$HOME/.local/share/containers/podman/machine/podman.sock"

pushd hack >/dev/null
  ctlptl apply -f ctlptl.yaml
  skaffold run
popd >/dev/null
```

## Install Flux and the toolchain

```
# create a personal access token https://github.com/settings/tokens
export GITHUB_TOKEN='xxxx'
flux bootstrap github \
  --owner      "fahlke" \
  --repository "flux-kind" \
  --path       "clusters/kind-kind" \
  --components "source-controller,kustomize-controller,helm-controller" \
  --personal \
  --token-auth
```

## Cleanup

```
podman machine stop && \
podman machine rm --save-image --force
```

## Optimize your pod resources with KRR

```
kubectl port-forward -n prometheus pod/prometheus-prometheus-kube-prometheus-prometheus-0 9090
krr simple -n cert-manager -p http://127.0.0.1:9090
```
