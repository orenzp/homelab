# Gemini CLI - Homelab Mandates

## 1. Security & Secrets
- **CRITICAL:** Never hardcode API keys, passwords, or tokens in any file.
- **Bitwarden Integration:** All secrets (GitHub tokens, SSH passwords) MUST be retrieved using the Bitwarden CLI (`bw`).
- **Secret Retrieval Pattern:** Use `delegate_to: localhost` in Ansible to run `bw get item` on the management machine.
- **Credential Protection:** Always use `no_log: true` in Ansible tasks that handle sensitive data from Bitwarden.

## 2. Ansible Conventions
- **User Access:** Always use `remote_user: root` for cluster provisioning tasks.
- **Inventory:** The source of truth for node IPs is `ansible/hosts.ini`.
- **Master Playbook:** Always use `ansible/site.yml` as the entry point for full deployments.
- **Idempotency:** Ensure all playbooks can be run multiple times without causing errors (e.g., check if K3s is already installed before running the script).

## 3. GitOps & FluxCD
- **Branching:** The primary sync branch for both staging and production is `production`.
- **Hierarchy:** Maintain the `base` -> `overlays` structure for all applications in `apps/`.
- **Bootstrap Path:** Always use `./clusters/{env}` as the target path for Flux bootstrap.
- **Validation:** Always verify FluxCD health using `flux check` after bootstrapping.

## 4. Hardware/Node Setup
- **Cloud-Init:** Always use the `node_setup/user-data.example` template for new nodes.
- **Dependencies:** Ensure `open-iscsi` and `nfs-common` are present for Longhorn storage.
- **Cgroups:** Verify that `cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1` is present in `cmdline.txt` during provisioning.
