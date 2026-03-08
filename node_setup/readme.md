# Raspberry Pi Node Setup (Automated)

This guide explains how to provision a new Raspberry Pi node using **Cloud-Init** on the Ubuntu Server (64-bit) image.

## 1. Flash the SD Card
1.  Flash the **Ubuntu Server (64-bit)** image for Raspberry Pi.
2.  **Keep the SD card inserted!** A partition named `system-boot` will appear.

## 2. Pre-Configure via Boot Partition
Copy and rename the example files from this repository to the `system-boot` partition:

### Step A: Configure Hostname and Root Password
1.  Copy `user-data.example` to `system-boot/user-data`.
2.  Edit `user-data`:
    *   Change `hostname: k8s-node-xx` to your desired name (e.g., `k8s-master-01`).
    *   Update `PLACEHOLDER_PASSWORD` under `chpasswd` for the **root** user.
    *   (Recommended) Add your public SSH key under the `ubuntu` user for fallback access.

### Step B: Configure Static IP
1.  Copy `network-config.example` to `system-boot/network-config`.
2.  Edit `network-config`:
    *   Set the `addresses` to your desired static IP (e.g., `192.168.1.11/24`).
    *   Ensure the `gateway4` matches your router's IP.

## 3. First Boot
1.  Insert the SD card into the Raspberry Pi and power it on.
2.  Wait 2-3 minutes for the setup to complete.
3.  Verify connectivity: `ssh root@<configured-ip>`.

## Why this is better?
*   **Root Enabled**: Ansible can connect as `root` immediately with the preset password.
*   **K3s Ready**: Cloud-init automatically enables `cgroups`, disables `swap`, and installs dependencies like `open-iscsi` and `nfs-common`.
