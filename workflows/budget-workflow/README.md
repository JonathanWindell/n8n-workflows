# Gmail Auto-Sorter & AI Classifier Workflow
[![n8n](https://img.shields.io/badge/n8n-FF6D5B?logo=n8n&logoColor=white)](#)
[![Discord](https://img.shields.io/badge/Discord-5865F2?logo=discord&logoColor=white)](#)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?logo=telegram&logoColor=white)](#)
[![Gmail](https://img.shields.io/badge/Gmail-EA4335?logo=gmail&logoColor=white)](#)
[![Google Gemini](https://img.shields.io/badge/Google%20Gemini-8E75B2?logo=googlegemini&logoColor=white)](#)

A smart, low-code n8n workflow that uses a Large Language Model (LLM) to read, categorize, and sort incoming emails automatically. It also sends high-priority notifications to Discord and Telegram.

## Highlights
* **LLM Text Classifier:** Uses advanced prompting to reliably classify emails into 10 distinct categories (Finance, Education, Security, Transactions, etc.) based on content context.
* **Smart Notifications:** Integrates with Discord & Telegram so you only get pinged for high-priority emails (like Security Alerts or Finance updates).
* **Agnostic LLM:** By default, it is configured for Google Gemini, but you can swap out the LLM node for OpenAI, Anthropic, or any other supported provider in n8n.
* **Auto-Archiving:** Automatically applies specific Gmail labels and removes emails from the Inbox to keep your main feed clean.

## Overview
This workflow triggers every time a new email arrives. It filters out expected noise (like your own daily automated newsletters), passes the email body to an AI Text Classifier, and then routes it through a switchboard. Based on the classification, it will alert you on specific chat platforms and file the email into the correct Gmail folder.

*Note: While this template uses the native Gmail trigger, you can easily swap the trigger node for a standard IMAP node to support Outlook or custom domains.*

## Architecture
To view design of workflow, press here. 
- [Architecture](./Architecture.md)

## Prerequisites
Before importing this workflow, ensure you have:
1. A running **n8n instance** (Self-hosted via Docker, Desktop app, or n8n Cloud).
2. A **Gmail Account** configured with OAuth credentials in n8n (or standard IMAP credentials).
3. **Discord Channels** with active Webhook URLs (this workflow uses three separate webhooks for different priorities).
4. A **Telegram Bot** token and Chat ID (for high-priority alerts).
5. An API key for your **LLM of choice** (Gemini, OpenAI, etc.).

## Usage Instructions
1. Copy the contents of `workflow.json`.
2. Open your n8n workspace, create a new workflow, and press `CTRL + V` (or `Cmd + V`) to paste the nodes.
3. Follow the Configuration Steps below to connect your accounts.

## Configuration Steps

Because this workflow interacts with your personal labels and chat IDs, you will need to map a few variables after importing:

### 1. Connect Credentials
Double-click the following nodes and select or create your credentials:
* **Gmail Trigger** & all **Gmail Action** nodes.
* **Telegram** nodes.
* **Discord** nodes.
* **Google Gemini Chat Model** (Or delete it and attach your preferred LLM node).

### 2. Update the "If" Node
The workflow currently filters out a daily newsletter and checks against an email address. This node is only needed if you are implementing [ News Workflow](./workflows/news-workflow/README.md). 
* Open the **"Check for daily news letter"** node.
* Replace `<your-email@example.com>` with your actual email address.

### 3. Customize AI Prompt Categories
* Open the **"Mail - Text Classifier"** node.
* Review the descriptions for `Education`, `Finance & Bills`, and `Transactional`. Update the placeholders (like `<Your University Name>` or `<Your Bank>`) with the specific institutions relevant to your life to improve the AI's accuracy.

### 4. Re-map Gmail Labels
Because Gmail label IDs are unique to every Google account, the imported nodes will show an error until you select your own labels.
* Open each **"Add: [Category]"** Gmail node.
* Under the `Label IDs` field, clear the `<YOUR_LABEL_ID_HERE>` text.
* Use the dropdown menu to select the corresponding label from your own Gmail account.

### 5. Set Telegram Chat IDs
* Open the two **Telegram** nodes.
* Replace `<YOUR_TELEGRAM_CHAT_ID>` with your personal Chat ID.