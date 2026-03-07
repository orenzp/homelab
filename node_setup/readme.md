# Raspberry Pi Node Setup (Automated)

This guide explains how to quickly provision a new Raspberry Pi node by pre-configuring it directly on the SD card before the first boot. This method uses **Cloud-Init**, which is natively supported by official Ubuntu Server images.

## 1. Flash the SD Card
1.  Download the **Ubuntu Server (64-bit)** image for Raspberry Pi.
2.  Use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) or [BalenaEtcher](https://www.balena.io/etcher/) to flash the image.
3.  **Do not eject the SD card yet!**

## 2. Pre-Configure via Boot Partition
Once flashed, a small FAT32 partition named `system-boot` will appear on your computer. This partition is accessible on Windows, macOS, and Linux.

### Step A: Configure Hostname and User
1.  Copy `user-data.example` from this repo to the `system-boot` partition and rename it to **`user-data`**.
2.  Edit `user-data`:
    *   Change `hostname: k8s-node-01` to your desired name.
    *   Add your public SSH key under `ssh_authorized_keys`.
    *   (Optional) Update the hashed password.

### Step B: Configure Static IP
1.  Copy `network-config.example` from this repo to the `system-boot` partition and rename it to **`network-config`**.
2.  Edit `network-config`:
    *   Change the `addresses` to your desired static IP (e.g., `192.168.1.11/24`).
    *   Ensure the `gateway4` matches your router's IP.

## 3. First Boot
1.  Safely eject the SD card and insert it into the Raspberry Pi.
2.  Connect an Ethernet cable and power it on.
3.  Wait 2-3 minutes for the initial setup to complete.
4.  You can now SSH into your node:
    ```bash
    ssh ubuntu@<configured-ip>
    ```

## Why this is better?
*   **No Linux needed:** You can configure everything on a Windows or Mac since the boot partition is FAT32.
*   **Automated:** No need to manually edit `/etc/shadow` or create empty `ssh` files; cloud-init handles it all.
*   **Scalable:** Just copy-paste these two files to every new SD card you flash.
