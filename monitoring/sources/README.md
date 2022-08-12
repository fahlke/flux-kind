flux create source helm prometheus \
  --url https://prometheus-community.github.io/helm-charts \
  --interval 10m \
  --export > prometheus.yaml

kustomize create --autodetect
