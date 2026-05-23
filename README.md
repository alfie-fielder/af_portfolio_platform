# af_portfolio_platform

ArgoCD platform configuration for the af_portfolio EKS cluster.

## Overview

This repo is the GitOps source of truth for platform-level tooling running on the
af_portfolio EKS cluster. It follows the ArgoCD app-of-apps pattern — a root
Application (bootstrapped via Terraform in af_portfolio) watches this repo and
deploys whatever Application manifests it finds in `platform-apps/`.

## Repository Structure

```
af_portfolio_platform/
  platform-apps/        # ArgoCD Application manifests — one per platform tool
  README.md
```

## How it works

1. Terraform (`af_portfolio/eks-addons/argocd/`) bootstraps ArgoCD onto the cluster
   and creates a root Application pointing at this repo
2. ArgoCD watches `platform-apps/` for Application manifests
3. Any Application manifest dropped into `platform-apps/` is automatically picked
   up and synced to the cluster
4. Changes to this repo go through PR review before merging — ArgoCD syncs on merge

## Platform Apps

| App | Namespace | Status |
|-----|-----------|--------|
| Karpenter NodePools | karpenter | Planned |
| cert-manager | cert-manager | Planned |
| external-dns | external-dns | Planned |
| kube-prometheus-stack | monitoring | Planned |

## Related Repos

- [af_portfolio](https://github.com/alfie-fielder/af_portfolio) — Terraform infrastructure
- [af_portfolio_modules](https://github.com/alfie-fielder/af_portfolio_modules) — Reusable Terraform modules
