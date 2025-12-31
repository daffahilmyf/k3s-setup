# CloudNativePG on k3s (Helm)

Structure
- Chart.yaml, values.yaml: Helm chart root and defaults
- values-dev.yaml: single-instance, small storage
- values-prod.yaml: 3 instances, larger storage

Notes
- Install the CloudNativePG operator separately (Helm).
- Observability resources target namespace `monitoring` and expect the `kube-prometheus-stack` release label.
- `values.yaml` is a minimal example; add backups, resources, and policies as needed.

Operator install (Helm)
Source charts repo: https://github.com/cloudnative-pg/charts
```
helm repo add cnpg https://cloudnative-pg.github.io/charts
helm repo update
helm upgrade --install cnpg \
  --namespace cnpg-system \
  --create-namespace \
  cnpg/cloudnative-pg
```

Install/Upgrade
```
helm upgrade --install cnpg . -n postgres --create-namespace -f values-dev.yaml
helm upgrade --install cnpg . -n postgres --create-namespace -f values-prod.yaml
```
