# AI-Powered Budget & Receipt Automation
[![n8n](https://img.shields.io/badge/n8n-FF6D5B?logo=n8n&logoColor=white)](#)
[![Google Gemini](https://img.shields.io/badge/Google%20Gemini-8E75B2?logo=googlegemini&logoColor=white)](#)
[![Google Drive](https://img.shields.io/badge/Google%20Drive-4285F4?logo=googledrive&logoColor=white)](#)
[![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?logo=googlesheets&logoColor=white)](#)
[![Discord](https://img.shields.io/badge/Discord-5865F2?logo=discord&logoColor=white)](#)

A smart, low-code system that automates your expense tracking using two distinct workflows. Whether you have a physical receipt image or a manual entry for cash purchases, this system uses **Google Gemini 2.5 Flash** for extraction and the **Frankfurter API** for real-time currency conversion (EUR to SEK).

## Highlights
* **AI-Driven OCR:** Extracts merchant names, dates, amounts, and categories from images automatically.
* **Live Currency Sync:** Converts foreign expenses (EUR) to local currency (SEK) using the latest market rates.
* **Automated Database Management:** Maintains a master Google Sheets budget without manual copy-pasting.
* **Discord Notifications:** Get instant feedback on whether a receipt was successfully filed or if the AI encountered an error.
* **Self-Cleaning:** Manual entries are moved to the master list and deleted from the input tab, while receipt images are archived in a "Processed" folder.

## Overview
This system consists of two templates designed to work together:
1.  **Auto Receipt Parsing:** For digital/physical receipts stored in Google Drive.
2.  **Manual Data Entry:** For quick text-based entries in Google Sheets.

## Architecture
To view the design and logic of the workflows, press here:
- [Architecture](./Architecture.md)

## Prerequisites
1.  A running **n8n instance**.
2.  **Google Cloud Project** with Drive and Sheets APIs enabled (OAuth2).
3.  **Google Gemini API Key** (via Google AI Studio).
4.  **Discord Webhook URL** for status updates.

---

## Usage Instructions: Auto Receipt Parsing
This workflow is designed for physical or digital receipts (images/PDFs).

1.  **Upload:** Upload a photo or scan of your receipt to your designated "Watch" folder in Google Drive.
2.  **Processing:** n8n automatically detects the new file (checks every minute).
3.  **AI Extraction:** Google Gemini reads the image and identifies the Merchant, Date, Amount, and Category.
4.  **Conversion:** The workflow fetches the latest EUR/SEK exchange rate and calculates the local cost.
5.  **Completion:** Data is appended to your master Google Sheet, a Discord notification is sent, and the file is moved to your "Processed" archive folder.

## Usage Instructions: Manual Data Entry
This workflow is designed for cash purchases or expenses without a digital receipt.

1.  **Input:** Open your Google Sheet and go to the **"Manual Input"** tab.
2.  **Entry:** Enter the `Date`, `Merchant`, `Category`, and `Amount (EUR)`.
3.  **Processing:** n8n detects the new row in the spreadsheet.
4.  **Formatting:** The system formats the date and converts the amount from EUR to SEK using live rates.
5.  **Cleanup:** The entry is moved to the master **"Budget"** tab, and the original row is automatically deleted from the input tab to keep it clean.

---

## Configuration Steps

### 1. Connect Credentials
Double-click the following nodes to select or create your credentials:
* **Google Drive/Sheets:** Select your Google OAuth2 account.
* **Google Gemini:** Enter your API Key in the "Analyze Receipt" node.
* **Discord:** Add your Webhook URL to the notification nodes.

### 2. Map Folder & Spreadsheet IDs
Replace the placeholders in the following nodes:
* **Drive Triggers:** Choose the specific folder to watch.
* **Move File:** Select your "Processed" folder ID.
* **Google Sheets:** Select your Spreadsheet and the specific Sheet GIDs for both the "Budget" and "Manual Input" tabs.

### 3. Customize AI Categories
Open the **"Analyze Receipt"** node to edit the predefined categories (Groceries, Rent, Entertainment, etc.) in the prompt to match your specific budget structure. Do not forget to also implement this as a table etc in **Google Sheets** for easier management of expenses. 
