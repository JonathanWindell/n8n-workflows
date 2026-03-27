# Automated n8n to GitHub Backup Workflow

## Highlights
[![n8n](https://img.shields.io/badge/n8n-FF6D5B?logo=n8n&logoColor=white)](#)
[![GitHub](https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=white)](#)
[![Discord](https://img.shields.io/badge/Discord-5865F2?logo=discord&logoColor=white)](#)

* **GitOps for n8n:** Automatically backs up your active n8n workflows directly to a GitHub repository.
* **Smart Diffing (No empty commits!):** Uses a custom JavaScript node to strip out dynamic metadata (like `updatedAt`, `versionId`, and canvas coordinates) before comparing. It only commits to GitHub if you *actually* made logical changes to the nodes!
* **Automated Notifications:** Pings you on Discord whenever a new workflow is backed up or an existing one is updated.

## Overview
Keeping versions of your workflows safe is critical. This workflow acts as an automated version control system. Running nightly, it queries the n8n API to grab the latest JSON of a specific workflow and pulls the existing backup from GitHub. It sanitizes both JSON files to compare their core logic. If the workflow is brand new, it creates a new file in GitHub. If it has been modified, it pushes a commit to update the existing file. If nothing has changed, it silently finishes without spamming your commit history.

## Architecture
To view the design of the workflow, press here:
- [Architecture](./architecture.md)

## Prerequisites
Before importing this workflow, ensure you have:
1. A running **n8n instance**.
2. An **n8n API Key** (created in your n8n settings) to allow the workflow to query itself.
3. A **GitHub Account** and a **Personal Access Token (PAT)** with repository permissions.
4. A destination **GitHub Repository** created to store your backups.
5. A **Discord Webhook URL** for receiving commit notifications.

## Usage Instructions
1. Copy the contents of the `workflow.json` file.
2. Open your n8n workspace, create a new workflow, and press `CTRL + V` (or `Cmd + V`) to paste the nodes.
3. Follow the Configuration Steps below to map your APIs and repository info.

## Configuration Steps

### 1. Set up your n8n API Credential
* Open the **"Get a workflow version"** node.
* Create a new `n8n API` credential. You will need to generate this API key in your n8n instance settings (Settings > n8n API > Create API API).

### 2. Target your Workflow
* In the same **"Get a workflow version"** node, look for the `Workflow ID` field.
* Replace `<YOUR_WORKFLOW_ID>` with the actual ID of the workflow you want to back up. (You can find the ID in the URL of the workflow you are editing, e.g., `n8n.example.com/workflow/123xyz`).

### 3. Configure GitHub Integration
* Open all three GitHub nodes (**"Get a file"**, **"Create a file"**, and **"Edit a file"**).
* Set up your GitHub API credential using your Personal Access Token (PAT).
* Update the `Repository Owner` field (replace `<YOUR_GITHUB_USERNAME>`).
* Update the `Repository Name` field (replace `<YOUR_GITHUB_REPO>`).

### 4. Connect Discord Webhooks
* Open the **"New file"** and **"New version"** Discord nodes.
* Provide your specific Webhook URL to get notified when a successful backup triggers.