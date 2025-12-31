# CloudNativePG on k3s (Kustomize)

Structure
- base: namespaces
- components/operator: CNPG operator manifest (vendored)
- components/cluster: base cluster spec
- components/observability: PodMonitors and basic alerts for kube-prometheus-stack
- overlays/dev: single-instance, small storage
- overlays/prod: 3 instances, larger storage

Notes
- Operator manifest is vendored in `components/operator/cnpg-1.28.0.yaml`.
- Update this file when bumping CNPG.
- Observability resources target namespace `monitoring` and expect the `kube-prometheus-stack` release label.
- `components/cluster/cluster.yaml` is a minimal example; add backups, resources, and policies as needed.

Apply
```
kubectl apply -k overlays/dev
kubectl apply -k overlays/prod
```
