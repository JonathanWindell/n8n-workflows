# 📰 Tech News Aggregator & AI Summarizer Workflow

## Highlights
[![n8n](https://img.shields.io/badge/n8n-FF6D5B?logo=n8n&logoColor=white)](#)
[![Gmail](https://img.shields.io/badge/Gmail-EA4335?logo=gmail&logoColor=white)](#)
[![Google Gemini](https://img.shields.io/badge/Google%20Gemini-8E75B2?logo=googlegemini&logoColor=white)](#)

* **RSS Parsing & Filtering:** Connects to multiple RSS feeds and filters articles using a highly customizable keyword list (e.g., "Kubernetes", "Nvidia", "Zero-day").
* **AI Summarization:** Utilizes an LLM to read the full text of matching articles and generates concise, bulleted HTML summaries. It even automatically detects and outputs in the source language!
* **Automated Daily Digest:** Aggregates all the summarized articles into a single, beautifully formatted HTML email digest delivered straight to your inbox.

## Overview
This workflow is designed to combat information overload. Instead of scrolling through endless news feeds, this automation pulls articles published within the last 24 hours, scans them for topics you actually care about, and leverages AI to write a short executive summary for each. It then compiles these into a single daily email, labels it appropriately in Gmail, and removes it from your main inbox to keep things tidy.

## Architecture
To view the design of the workflow, press here:
- [Architecture](./Architecture.md)

## Prerequisites
Before importing this workflow, ensure you have:
1. A running **n8n instance** (Self-hosted via Docker, Desktop app, or n8n Cloud).
2. A **Gmail Account** configured with OAuth credentials in n8n (or standard IMAP/SMTP credentials if you swap the nodes).
3. An API key for your **LLM of choice** (Google Gemini, OpenAI, Anthropic, etc.).

## Usage Instructions
1. Copy the contents of the `workflow.json` file.
2. Open your n8n workspace, create a new workflow, and press `CTRL + V` (or `Cmd + V`) to paste the nodes.
3. Follow the Configuration Steps below to connect your accounts and customize your feeds.

## Configuration Steps

### 1. The Trigger Setup
By default, this workflow uses the **"When Executed by Another Workflow"** trigger, meaning it expects a "parent" workflow (like a master Cron job) to start it. 
* *If you want this to run independently:* Delete the trigger node and replace it with a **Schedule Trigger** set to run daily (e.g., every morning at 07:00).

### 2. Customize Sources and Keywords
* Open the **"Get News Sources and Interest Rules"** code node.
* Modify the `sources` array with your favorite RSS feed URLs.
* Modify the `interest_rules` array with the specific keywords you want to track (e.g., specific companies, programming languages, or hardware).

### 3. Connect Credentials
Double-click the following nodes and select or create your credentials:
* **Google Gemini Chat Model** (Or delete it and attach your preferred LLM node).
* All **Gmail** nodes (Send mail, Add label, Remove label).

### 4. Configure Email Recipient & Labels
* Open the **"Send mail"** node and change `<your-email@example.com>` to the email address where you want to receive the daily digest.
* Open the **"Add label - Tech news"** node. Clear the `<YOUR_TECH_NEWS_LABEL_ID>` placeholder and use the dropdown menu to select the specific Gmail label you want to apply to these digests.