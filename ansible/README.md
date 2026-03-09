# Ansible Automation

This directory contains the Ansible playbooks and configuration to automate the setup of the K3s cluster and the bootstrap of FluxCD.

## Prerequisites

1.  **Nodes Provisioned**: Ensure your nodes are flashed and configured with static IPs and root access. See [Node Setup Guide](../node_setup/readme.md).
2.  **Ansible Installed**: You need `ansible` and `sshpass` (if using password auth).
3.  **Bitwarden CLI (`bw`)**: Installed and authenticated for secret retrieval.

## Usage

### 1. Configure Inventory
Update `hosts.ini` with your node IP addresses:
```ini
[masters]
k8s-master-01 ansible_host=192.168.1.10

[workers]
k8s-node-01 ansible_host=192.168.1.11
...
```

### 2. Login to Bitwarden
```bash
bw login
export BW_SESSION=$(bw unlock --raw)
```

### 3. Run the Playbook
To provision the entire cluster:
```bash
ansible-playbook -i hosts.ini site.yml
```

This will:
- Prepare nodes (disable swap, enable cgroups).
- Install K3s (Master & Workers).
- Bootstrap FluxCD on the cluster.

## Playbook Structure
- `site.yml`: Main entry point.
- `k3s.yml`: K3s installation and cluster join logic.
- `flux-bootstrap.yml`: FluxCD bootstrap using Bitwarden for the GitHub token.
- `users.yml`: User management (if needed).
