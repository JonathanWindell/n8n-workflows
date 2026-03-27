# n8n-workflows
[![n8n](https://img.shields.io/badge/n8n-FF6D5B?logo=n8n&logoColor=white)](#)

# Highlights

- **Ready to Import:** Fully functional JSON templates. Just connect your credentials and go.
- **Low-Code Flexibility:** Built entirely in n8n, making them easy to tweak for your specific needs.

# Overview

This repository serves as a library for my personal n8n automations. Whether you are looking to manage your inbox, scrape news, or handle server backups, these templates provide a robust foundation that you can import into your own n8n instance in seconds.

# Author

I'm Jonathan and I develop projects in my sparetime that help myself and others become better and more efficient developers!
- [Linkedin](https://www.linkedin.com/in/jonathan-windell-418a55232/)
- [Portfolio](https://portfolio.jonathans-labb.org/)

# Workflows

| Name | Description | Status |
| :--- | :--- | :--- |
| [ Gmail Workflow](./workflows/gmail-workflow/README.md) | Automates email filtering and AI-driven categorization. Notifications through Discord & Telegram | ![Active](https://img.shields.io/badge/Status-Active-success?style=flat-square) |
| [ News Workflow](./workflows/news-workflow/README.md) | Scrapes specific sources and delivers summaries via HTML formatted mail. |  ![Active](https://img.shields.io/badge/Status-Active-success?style=flat-square) |
| [ Backup Workflow](./workflows/backup-workflow/README.md) | Automates database or file backups to github. | ![Updates](https://img.shields.io/badge/Status-Updates-orange?style=flat-square) - *V2 In Progress* |
| [ SSH Workflow](./workflows/ssh-workflow/README.md) | Executes remote commands to proxmox server for updating lxc & docker containers. | ![Active](https://img.shields.io/badge/Status-Active-success?style=flat-square) |
| [ Budget Workflow](./workflows/budget-workflow/README.md) | Automates expense tracking using AI, Google Drive & Sheets | ![Active](https://img.shields.io/badge/Status-Active-success?style=flat-square) |
| [ Job Searching Workflow](./workflows/job%20searching-workflow/) | Fetch job postings based on choosen keywords and send personalised mail | ![Active](https://img.shields.io/badge/Status-Active-success?style=flat-square) |
| *In Progress: CV & Cover Letter Manipulation* | Manipulate CV & Cover Letter based on requirements for job | ![Status](https://img.shields.io/badge/Status-Upcoming-lightgrey?style=flat-square) |

# Usage Instructions
1. Browse to the specific workflow folder.
2. Open the `workflow.json` file.
3. Copy the entire JSON content.
4. In your n8n editor, press `CTRL + V` (or `Cmd + V`) to paste and import the nodes.

# Installation Instructions

### Prerequisites
- A running **n8n instance** (Self-hosted via Docker, Desktop app, or n8n Cloud).
- Required API keys for the specific services used (e.g., OpenAI, Google, Discord).

### Configuration
Most workflows use **Environment Variables** or **Global Constants**. Ensure you check the "Nodes" settings after importing to map your specific credentials.

# Contributions
Contributions are welcome! 

1. **Fork** the project.
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`).
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4. **Test it:** Ensure the workflow runs without errors in a clean n8n environment.
5. Push to the Branch (`git push origin feature/AmazingFeature`).
6. Open a **Pull Request**.

# License
Distributed under the MIT License. See `LICENSE` file for more information.
