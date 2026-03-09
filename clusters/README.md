# FluxCD Clusters

This directory defines the GitOps state for the different Kubernetes clusters.

## Structure
- `production/`: FluxCD system configuration and application overlays for the production cluster.
- `staging/`: FluxCD system configuration and application overlays for the staging cluster.

## FluxCD Configuration
Each environment directory contains:
- `flux-system/`: The core FluxCD components and sync definition.
- `infrastructure.yaml`: Kustomization for shared infrastructure (MetalLB, Longhorn, etc.).
- `apps.yaml`: Kustomization for application deployments.

## Bootstrapping a New Cluster
To manually bootstrap a cluster using the GitHub provider:
```bash
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=homelab \
  --branch=production \
  --path=./clusters/production \
  --personal
```

## Maintenance
To verify the health of the FluxCD reconciliation:
```bash
flux get kustomizations
flux get sources git
flux check
```
