# Argo CD (Kustomize)

This is a simple, single-environment Argo CD install using the upstream manifest.

Apply
```
kubectl apply -k argocd
```

Delete
```
kubectl delete -k argocd
```

Notes
- The install manifest is vendored in `argocd/base/install.yaml`.
- To update Argo CD, replace that file with a newer upstream manifest.
