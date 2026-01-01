# NATS on k3s (Helm)

Structure
- Chart.yaml, values.yaml: Helm chart root and defaults
- values-dev.yaml: small 3-node cluster defaults
- values-prod.yaml: production-ish defaults for a small 3-node cluster

Notes
- This is a wrapper chart that pulls the upstream NATS Helm chart as a dependency.
- Use values in `values.yaml` or a separate values file to customize auth, JetStream, storage, and resources.
- JetStream file storage uses a PVC; set `storageClassName` if your k3s cluster does not have a default.

Install/Upgrade
```
cd nats
helm repo add nats https://nats-io.github.io/k8s
helm repo update
helm dependency update
helm upgrade --install nats . -n nats --create-namespace -f values-dev.yaml
helm upgrade --install nats . -n nats --create-namespace -f values-prod.yaml
```

Smoke test (nats-box)
```
kubectl -n nats get pods
kubectl -n nats exec deploy/nats-nats-box -- nats --server nats://nats:4222 ping
kubectl -n nats exec deploy/nats-nats-box -- nats --server nats://nats:4222 pub demo.hello "hi"
kubectl -n nats exec deploy/nats-nats-box -- nats --server nats://nats:4222 sub demo.hello
```
