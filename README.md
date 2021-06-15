[![test](https://github.com/orenzp/gitops/actions/workflows/test.yaml/badge.svg)](https://github.com/orenzp/gitops/actions/workflows/test.yaml) - [![e2e](https://github.com/orenzp/gitops/actions/workflows/e2e.yaml/badge.svg)](https://github.com/orenzp/gitops/actions/workflows/e2e.yaml)

# Description

## Project Goals

- The goal of the project is to fully automate my personal hosting environment at home. 

- To have the entire state of the environment declared in GIT 

# Architecture

- **Infrastructure**
  - [FluxCD - GitOps toolkit](https://fluxcd.io/)
  - [K3SUP🚀](https://github.com/alexellis/k3sup)
  - [GitHub Actions](https://github.com/features/actions) 
  - [METALLB - Layer 2 load balancer](https://metallb.universe.tf/)
- **Applications**
  - [podinfo: Demo app](https://github.com/stefanprodan/podinfo)
  - [Home Assistant](https://www.home-assistant.io/)
  - [Pi-Hole](https://pi-hole.net/)
  - [Plex](https://www.plex.tv/)



# Requirements



# Bootstrapping


## Production

```bash
flux check --pre

flux bootstrap github \
    --context=homeK3S \
    --owner={$GITHUB_USER} \
    --repository={$GITHUB_REPO} \
    --branch=main \
    --personal \
    --path=./clusters/production
```

## Staging

```bash
kind create cluster --name staging
```

```bash
flux check --pre

flux bootstrap github \
    --context=kind-staging \
    --owner={$GITHUB_USER} \
    --repository={$GITHUB_REPO} \
    --branch=staging \
    --personal \
    --path=./clusters/staging
```

# CICD Github workflow



flux check --pre

```
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=gitops \
  --branch=wip \
  --path=./ \
  --personal
```

Deploy podinfo application:

```
flux create source git podinfo \
  --url=https://github.com/stefanprodan/podinfo \
  --branch=master \
  --interval=30s \
  --export > ./gitops-test/podinfo-source.yaml
```

```
flux create kustomization podinfo \
  --source=podinfo \
  --path="./kustomize" \
  --prune=true \
  --validation=client \
  --interval=5m \
  --export > ./gitops-test/podinfo-kustomize.yaml
```

kubectl edit configmap -n kube-system kube-proxy
Set stricARP to true

```
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
mode: "ipvs"
ipvs:
  strictARP: true
```

