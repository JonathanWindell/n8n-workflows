# Stock Information Workflow

## Highlights
![n8n](https://img.shields.io/badge/n8n-FF6D5B?style=for-the-badge&logo=n8n&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=for-the-badge&logo=google-sheets&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-f55036?style=for-the-badge&logo=groq&logoColor=white)

## Overview
Keeping track of all information regarding your own holdings in the stock market can be quite tough with all the constant articles & news about the companies. This flow aims to search for news about your own holdings and read through all news and choose the top three most impactful news about the company. The workflow uses **Google** finance to every day get relevant data about the companies stock value. 

## Architecture
To view the design of the workflow, press here. 
- [Architecture](./architecture.md)

## Prerequisites
Before importing these workflows, ensure you have:
1. A running **n8n instance** (Self-hosted via Docker, Desktop app, or n8n Cloud).
2. A **Gmail Account** configured with OAuth credentials in n8n (or standard IMAP credentials).
3. A **Google Sheets Account** configured with OAuth credentials in n8n (or standard IMAP credentials).
4. An API key for your **LLM of choice** (Gemini, OpenAI, etc.). > **Note:** This workflow is configured with **Groq** using *Llama 3.3-70b-versatile*. You can set it up here [Groq LLM](https://console.groq.com/keys). 
5. An API key for **Brave Search API**. You can create one here [Brave Search API](https://brave.com/search/api/). 

## Usage Instructions
1. Copy the contents of the `workflow.json` file.
2. Open your n8n workspace, create a new workflow, and press `CTRL + V` (or `Cmd + V`) to paste the nodes.
3. Follow the Configuration Steps below to connect your server and notification channels.


## Configuration Steps
Double-click the following nodes and select or create your credentials:
* **Google Sheets** & all **Google Sheet** nodes.
* **Groq Chat Model** (Or delete it and attach your preferred LLM node).
* **Gmail** (Or your preferred mail)

**Google Sheet: Stock Information**

This is the only sheet needed for the workflow but it's very important that this sheet is set up correctly as the workflow fetches it's information from this worklflow. Following example is correct structure starting from **A1**.

| **Company** | **Ticker** | **Type** | **Price** | **Change Percentage** |
| ------- | ------ | ---- | ----- | ----------------- |
| Apple | AAPL | Stock | 120.31 | 0.65 |
| Tesla | TSLA | Stock | 156.65 | 1.45 |


**Google Sheet: Update Rows**
To avoid google sheets not updating the financial data setting up the **Update row(s)** node ensures that fresh data is fetched before the workflow starts. Currently it's setup to use the same sheet as the *Stock Information* but one can easily create a new sheet that **Update row(s)** node can use. 


