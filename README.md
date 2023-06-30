# flux-kind

## Prerequisites

```
brew install podman
brew install tilt-dev/tap/ctlptl
brew install skaffold
brew install kustomize
brew install helm
brew install fluxcd/tap/flux
brew install linkerd
```

## Stop collecting my data...

```
ctlptl analytics opt out
skaffold config set --global collect-metrics false
```

## Start Kind Cluster

```
# this command manages the /var/run/docker.sock link
sudo podman-mac-helper install

podman machine init \
  --cpus 8 --memory 24576 --disk-size 100 \
  --timezone 'Etc/UTC' --rootful
podman machine start
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
  --personal
```

## Cleanup

```
podman machine stop
podman machine rm --save-image --force
```
