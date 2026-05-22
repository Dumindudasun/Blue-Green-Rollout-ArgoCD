# Blue-Green Rollout with ArgoCD

This repository demonstrates a simple blue-green deployment pattern for Kubernetes using ArgoCD and an Argo Rollouts `Rollout` manifest.

## Repository structure

- `argocd/application.yaml` - ArgoCD Application definition to sync the `k8s/` directory into the `my-app` namespace.
- `k8s/namespace.yaml` - Namespace resource for the application.
- `k8s/services.yaml` - Two Kubernetes `Service` objects:
  - `my-app-active` for the active deployment
  - `my-app-preview` for preview traffic during the blue-green promotion
- `k8s/rollout.yaml` - `Rollout` resource configured for blue-green strategy with auto-promotion disabled.

## What it does

- Creates a namespace `my-app`.
- Deploys an NGINX-based application with two replicas.
- Uses Argo Rollouts blue-green strategy to manage active and preview service switching.
- Uses ArgoCD to continuously sync and reconcile this state from the Git repository.

## Prerequisites

- Kubernetes cluster
- ArgoCD installed in the cluster
- Argo Rollouts installed in the cluster
- Access to the GitHub repo URL configured in `argocd/application.yaml`

## Deploying with ArgoCD

1. Apply the ArgoCD application manifest to your ArgoCD instance:

```bash
git clone https://github.com/Dumindudasun/Blue-Green-Rollout-ArgoCD.git
kubectl apply -f argocd/application.yaml
```

2. In ArgoCD, sync the `my-app` application.

3. Verify the rollout and services:

```bash
kubectl get rollout -n my-app
kubectl get svc -n my-app
kubectl get pods -n my-app
```

## Notes

- The Rollout is configured with `autoPromotionEnabled: false`, so promotion from preview to active must be done manually.
- Update the container image in `k8s/rollout.yaml` to trigger a new deployment.

## Optional manual promotion

If you have Argo Rollouts CLI installed, you can promote the rollout manually:

```bash
kubectl argo rollouts promote my-app -n my-app
```
