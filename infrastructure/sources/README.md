flux create source helm metrics-server \
  --url https://kubernetes-sigs.github.io/metrics-server \
  --interval 10m \
  --export > metrics-server.yaml

flux create source helm jaeger \
  --url https://jaegertracing.github.io/helm-charts \
  --interval 10m \
  --export > jaeger.yaml

flux create source helm kong \
  --url https://charts.konghq.com \
  --interval 10m \
  --export > kong.yaml

flux create source helm keda \
  --url https://kedacore.github.io/charts \
  --interval 10m \
  --export > keda.yaml

flux create source helm gatekeeper \
  --url https://open-policy-agent.github.io/gatekeeper/charts \
  --interval 10m \
  --export > gatekeeper.yaml

kustomize create --autodetect
