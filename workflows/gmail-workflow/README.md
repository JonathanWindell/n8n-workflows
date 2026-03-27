# AI-Powered Gmail Classifier & Inbox Organizer
[![n8n](https://img.shields.io/badge/n8n-FF6D5B?logo=n8n&logoColor=white)](#)
[![Google Gemini](https://img.shields.io/badge/Google%20Gemini-8E75B2?logo=googlegemini&logoColor=white)](#)
[![Gmail](https://img.shields.io/badge/Gmail-EA4335?logo=gmail&logoColor=white)](#)
[![Telegram](https://img.shields.io/badge/Telegram-26A69A?logo=telegram&logoColor=white)](#)
[![Discord](https://img.shields.io/badge/Discord-5865F2?logo=discord&logoColor=white)](#)

A sophisticated email management system that transforms your cluttered inbox into a structured, categorized archive. By leveraging **Google Gemini** for deep text analysis, this workflow automatically classifies emails into 10 distinct categories, triggers high-priority alerts via **Telegram** and **Discord**, and enforces "Inbox Zero" by automatically archiving processed mail.

## Highlights
* **Context-Aware Sorting:** Uses AI to distinguish between similar emails (e.g., separating university assignments from financial invoices based on context).
* **Multi-Stage Priority Alerts:** High-stakes categories like "Security" and "Finance" trigger instant push notifications across multiple platforms.
* **Intelligent Noise Reduction:** Automatically identifies and disregards repetitive newsletters or "Daily Feed" emails to preserve API tokens.
* **Rate-Limit Resilience:** Implements batch processing and wait timeouts to handle high email volumes without exceeding Gmail or Google AI limits.
* **Auto-Archiving System:** Applies custom labels and removes the primary "Inbox" tag once classification is successful.

## Overview
This workflow is a comprehensive template designed for total inbox control:
1.  **Gmail Classifier:** The core engine that listens for mail and manages the AI sorting logic.

## Architecture

### Gmail Workflow
To view the design and logic of the classification flow, press here:
- [Gmail Workflow](./architecture.md)

## Prerequisites
1.  A running **n8n instance**.
2.  **Google Cloud Project** with Gmail API enabled (OAuth2).
3.  **Google Gemini API Key** (via Google AI Studio).
4.  **Telegram Bot Token** and Chat ID for security/finance alerts.
5.  **Discord Webhook URL** for secondary priority notifications.

---

## Usage Instructions: AI Email Classification
This workflow is designed to run automatically in the background.

1.  **Trigger:** n8n polls your Gmail inbox every 3 minutes for new messages.
2.  **Pre-Filtering:** The system checks for "Daily News" headers; if found, the mail is disregarded to save processing power.
3.  **AI Analysis:** The **Text Classifier** node sends the Subject and Snippet to Google Gemini. Gemini matches the mail against 10 categories (Security, Education, Finance, Travels, etc.).
4.  **Alerting:** * **Security & Finance:** Sends an alert to Telegram, then Discord, then adds the Gmail label.
    * **Education:** Sends a Discord alert, then adds the Gmail label.
    * **All Others:** Adds the specific Gmail label directly.
5.  **Archiving:** Once labeled, the "Inbox" tag is removed, moving the email into your custom-labeled archive.

---

## Configuration Steps

### 1. Connect Credentials
Double-click the following nodes to select or create your credentials:
* **Gmail:** Select your Gmail OAuth2 account for the Trigger and Label nodes.
* **Google Gemini:** Select your Gemini(PaLM) API account in the "Chat Model" node.
* **Telegram/Discord:** Add your Bot Tokens and Webhook URLs to the notification nodes.

### 2. Map Label IDs
Gmail uses internal IDs for labels. You must replace the placeholders in the following nodes:
* **Add Label Nodes:** Open each "Add: [Category]" node and select your specific Gmail label from the dropdown.
* **Remove Inbox:** Ensure the "Remove Labels" node is set to target the `INBOX` label.

### 3. Customize AI Categories
Open the **"Mail - Text Classifier"** node to edit the classification logic. You can adjust the "Positive" and "Negative" indicators for each category to help the AI better understand your specific life (e.g., adding your specific University name or Bank name to the description).

---