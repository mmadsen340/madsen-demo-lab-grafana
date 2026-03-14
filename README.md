# Prometheus + Grafana Deployment (Argo CD)

This repo contains the manifests and Helm values to deploy `kube-prometheus-stack` into the `observability` namespace.

## Files

- `argocd-app-kube-prometheus-stack.yaml`: Argo CD Application for the Helm chart.
- `values.yaml`: Helm values for Prometheus, Grafana, and Alertmanager.
- `kustomization.yaml`: Single entrypoint to apply all Kubernetes manifests in this repo.

## Prerequisites

- A running Kubernetes cluster.
- Argo CD installed in namespace `argocd`.

## 1) Update placeholders

Edit these placeholders before deploying:

- In `argocd-app-kube-prometheus-stack.yaml`:
  - Confirm repo URL points to `https://github.com/mmadsen340/madsen-demo-lab-grafana.git` (or your fork if you changed it).
- In `values.yaml`:
  - Replace Grafana `adminPassword` with a secure value, or use an external/managed secret pattern.

## 2) Deploy kube-prometheus-stack via Argo CD

Apply the Argo CD application:

```bash
kubectl apply -f argocd-app-kube-prometheus-stack.yaml
```

Or apply everything in this repo in one command:

```bash
kubectl apply -k .
```

Verify sync and health:

```bash
kubectl -n argocd get application kube-prometheus-stack
kubectl -n observability get pods
```

## 3) Validate

- Open `https://grafana.madsen-demo-lab.com`.
- Login with Grafana admin credentials from `values.yaml`.
- Confirm Prometheus datasource and Kubernetes dashboards are present.

## Notes

- This setup keeps Grafana as internal `ClusterIP`.
- Prometheus, Alertmanager, and Grafana include PVC-backed persistence.
- CRDs are managed safely with Argo CD `ServerSideApply=true`.
