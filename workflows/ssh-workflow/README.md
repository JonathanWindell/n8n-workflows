# Proxmox & LXC Automated Updater Workflow

## Highlights
[![n8n](https://img.shields.io/badge/n8n-FF6D5B?logo=n8n&logoColor=white)](#)
[![Proxmox](https://img.shields.io/badge/Proxmox-E57000?logo=proxmox&logoColor=white)](#)
[![SSH](https://img.shields.io/badge/SSH-231F20?logo=ssh&logoColor=white)](#)
[![Discord](https://img.shields.io/badge/Discord-5865F2?logo=discord&logoColor=white)](#)

* **Fully Automated Maintenance:** Runs on a defined schedule to keep your Proxmox server and its containers up to date without manual intervention.
* **Pre-flight Health Checks:** Pings the Proxmox web interface before attempting SSH connections, immediately alerting you if the server is offline.
* **Dynamic LXC Mapping:** Automatically fetches a list of *currently running* LXC containers and iterates through them to run updates individually.
* **Robust Error Handling & Reporting:** Aggregates pass/fail statuses for every single container and sends a clean, formatted summary report directly to Discord.

## Overview
Keeping home lab servers patched can be tedious. This workflow automates the monthly maintenance of a Debian-based Proxmox environment. Triggered automatically on the first Sunday of every month, it verifies the server is online via HTTP. If successful, it establishes an SSH connection using a private key to run `apt-get update && apt-get dist-upgrade` on the host. It then executes `pct list` to find all active LXC containers, loops through them to run `apt-get upgrade`, and finally posts a success/failure summary to a Discord channel.

## Architecture
To view the design of the workflow, press here. 
- [Architecture](./Architecture.md)

## Prerequisites
Before importing this workflow, ensure you have:
1. A running **n8n instance**.
2. A **Proxmox Host** (or standard Debian/Ubuntu server if you remove the LXC-specific nodes) accessible from your n8n environment.
3. An **SSH Private Key** configured as a credential inside n8n with root (or sudo) access to the Proxmox host.
4. One or more **Discord Webhook URLs** for alerting and reporting.

## Usage Instructions
1. Copy the contents of the `workflow.json` file.
2. Open your n8n workspace, create a new workflow, and press `CTRL + V` (or `Cmd + V`) to paste the nodes.
3. Follow the Configuration Steps below to connect your server and notification channels.

## Configuration Steps

### 1. Configure the Schedule
By default, the trigger is set to run at `02:00` on the first Sunday of every month. 
* Open the **"First sunday every month"** Schedule Trigger node to adjust the Cron expression to fit your preferred maintenance window.

### 2. Set the Target Server IP
* Open the **"HTTP - Server live"** node. 
* Replace `<YOUR_PROXMOX_IP_OR_DOMAIN>` with your server's actual IP address or local hostname.

### 3. Set up Proxmox SSH Key
To allow n8n to execute commands on your server, you need an SSH key pair.

* **Step 1:** Generate an SSH key pair (if you don't already have one) on your local machine or inside your n8n environment using the command `ssh-keygen -t ed25519`.
* **Step 2:** Open your **Proxmox Shell** and open the authorized keys file: `nano /root/.ssh/authorized_keys`. 
* **Step 3:** Paste your **Public Key** (`id_ed25519.pub`) into this file and save it.
* **Step 4:** Verify your SSH settings. Open `/etc/ssh/sshd_config` in the Proxmox shell and ensure `PermitRootLogin`, `PubkeyAuthentication`, and `AuthorizedKeysFile` are uncommented and enabled. Restart the SSH service if you made changes (`systemctl restart sshd`).

### 4. Attach your SSH Key in n8n
* Double-click the **"SSH - Proxmox server"** node (and all subsequent SSH action nodes).
* Under credentials, create a new **SSH Private Key** credential and paste your **Private Key** (the pair to the public key you added to Proxmox). 
* *Note: Ensure the host IP and port are correctly defined inside the credential settings themselves.*

### 5. Connect Discord Webhooks
This workflow uses three separate Discord nodes to alert you depending on where the workflow succeeds or fails.
* Open each **Discord Webhook** node.
* Provide your specific Webhook URL. You can route the summary report to a standard channel, and the failure alerts to an urgent/admin channel if you prefer.