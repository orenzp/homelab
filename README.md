> DRAFT - WORK IN PROGRESS
# Description
Last year my wife came to me a said: "Oren you should start a project so you will have something to do outside of work", So I created this repository :grin:

This repo holds all the configuration and documentation that is needed to set up my self-hosting home Kubernetes cluster on raspberry pie boards.

## Project Goals

- The goal of the project is to fully automate my hosting environment at home. 
- To have the entire state of the environment declared in GIT
- Learn and implement the latest DevOps tools and methods.

# Architecture

- **Infrastructure**
  - [FluxCD - GitOps Toolkit](https://fluxcd.io/)
  - [K3S - Kubernetes Distro](https://k3s.io/)
  - [METALLB - Layer 2 LB](https://metallb.universe.tf/)
  - [LongHorn - Distributed storage for K8S](https://rancher.com/products/longhorn/)

- **Applications**
  - [Podinfo - Demo app](https://github.com/stefanprodan/podinfo)
  - [Home Assistant - Home Automation](https://www.home-assistant.io/)
  - [Pi-Hole - DNS Ad Blocker](https://pi-hole.net/)
  - [Plex - Media Center](https://www.plex.tv/)
  - [WireGuard VPN](https://www.wireguard.com/)
<p align="center">
<td align="left"><img src="/docs/HomeLab.png" width="400" /></td>
</p>

## Continues Integration Pipeline [![test](https://github.com/orenzp/gitops/actions/workflows/test.yaml/badge.svg)](https://github.com/orenzp/gitops/actions/workflows/test.yaml) --- [![e2e](https://github.com/orenzp/gitops/actions/workflows/e2e.yaml/badge.svg)](https://github.com/orenzp/gitops/actions/workflows/e2e.yaml)
I use Github Action to enable a Continues Integration solution to check the new code and config that I plan to represent to the system.

  - [GitHub Actions](https://github.com/features/actions) 

# Bootstrapping
The requirements to bootstrapping the Kubernetes Cluster on my raspberry pies. The bootstrap process is divided into two steps. First bootstrap the cluster, then deploy the FluxCD Controller to deploy all my Kubernetes Resources.

- [Cluster Bootstrap](docs/cluster_bootstrap.md)
- [GitOps Bootstrap](docs/gitops_bootstrap.md)
