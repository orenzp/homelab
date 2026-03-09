# Applications

This directory contains the FluxCD application manifests and configuration for the Homelab cluster.

## Structure
- `base/`: Common manifests and configurations used across all environments.
- `production/`: Overlays and specific configurations for the production cluster.
- `staging/`: Overlays and specific configurations for the staging cluster.

## Deployment Strategy
All applications are managed via FluxCD using **Kustomize**.
1.  **Define Base**: Create manifests in `base/` (e.g., `apps/base/pihole/`).
2.  **Define Overlays**: Create an environment-specific overlay in `production/` or `staging/` (e.g., `apps/production/kustomization.yaml`).
3.  **Deploy**: FluxCD will automatically detect the changes in Git and apply them to the cluster.

## Current Applications
- **Pi-Hole**: DNS ad-blocking and network-wide monitoring.
- **Podinfo**: Demo application for testing FluxCD sync.
- **Home Assistant**: Home automation platform.
- **WireGuard**: VPN for secure remote access.
- **Plex**: Media server.

## Adding a New Application
1. Add the Kubernetes manifests to `base/`.
2. Reference the new application in the `kustomization.yaml` of the target environment (`production/` or `staging/`).
3. Push to Git. Flux will take care of the rest.
