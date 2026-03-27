# Job Searcher & AI Classifier Workflow Suite

![n8n](https://img.shields.io/badge/n8n-FF6D5B?logo=n8n&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-EA4335?logo=gmail&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Google%20Gemini-8E75B2?logo=googlegemini&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?logo=googlesheets&logoColor=white)

A smart low-code workflow suite that automates the discovery of job postings matching your specific criteria. The system utilizes Google Sheets, Large Language Models (LLM), and HTTP requests to scrape APIs from popular job sites and match opportunities based on your chosen keywords.

## Highlights

* **LLM Information Extraction:** Leverages the Information Extraction node in n8n to summarize matched job postings and format them into structured JSON for seamless email construction.
* **Daily Gmail Summary:** Once analysis is complete, a formatted digest is sent directly to your inbox with direct application links.
* **Agnostic LLM Architecture:** Configured for Google Gemini by default, but supports hot-swapping for OpenAI, Anthropic, or any other n8n-supported provider.
* **Relational Data Storing:** Uses Google Sheets as a relational database for easy keyword management. Update your skills or target roles in the spreadsheet without touching the workflow code.
* **Redundancy Safe:** Integrated tracking via Google Sheets ensures that previously found job postings are filtered out to prevent duplicate notifications.

## Overview

This workflow is triggered daily at 8:00 AM, starting with the Master Workflow. It contains two sub-workflow nodes that execute sequentially to accommodate the token rate limits of the Google Gemini free tier. If you have a paid subscription, you can easily configure both sub-workflows to run concurrently.

The system filters job postings against your chosen skills, roles, workplace preferences, and time requirements. Results are formatted into two daily emails delivered under your specified Gmail label.

> *Note: While this template uses the native Gmail trigger, you can swap the trigger node for a standard IMAP node to support Outlook or custom domains.*

## Architecture

### Job Searcher Wide - Workflow
* [Job Searcher Wide](./wide-workflow/architecture.md)

### Job Searcher Adzuna - Workflow
* [Job Searcher Adzuna](./adzuna-workflow/architecture.md)

### Master Job Searcher - Workflow
* [Job Searcher Master](./master-workflow/architecture.md)

## Prerequisites

1. A running **n8n instance** (Docker, Desktop, or Cloud).
2. A **Gmail Account** with OAuth2 or IMAP credentials.
3. A **Google Sheets Account** with OAuth2 credentials.
4. An API key for your **LLM of choice** (Gemini, OpenAI, etc.).

## Usage Instructions

1. Copy the contents of the `workflow.json` files.
2. In your n8n workspace, create a new workflow and press `CTRL + V` (or `Cmd + V`) to paste the nodes.
3. Map your credentials and variables as described in the configuration steps below.

## Configuration Steps

### 1. Connect Credentials
Double-click the following nodes to select or create your credentials:
* **Gmail Trigger** and all **Gmail Action** nodes.
* **Google Sheets** nodes.
* **Google Gemini Chat Model** (or your preferred LLM provider).

### 2. Set up Google Sheets
The workflow uses Google Sheets structured as a relational database. Create the following sheets with their respective headers.

#### Sheet 1: Job_URLs
This sheet stores the API endpoints for scraping.
> *Note: This workflow is tested with endpoints returning JSON data. It does not include logic for cleaning raw HTML scrapings.*

**Structure:**
| Job_URL |
| :--- |
| https://remoteok.com/remote-jobs.json |
| https://himalayas.app/jobs/api |

#### Sheet 2: Adzuna_Job_URLs
Adzuna requires country codes for its API.
> *Note: Adzuna requires a developer account. Sign up here: [Adzuna API](https://developer.adzuna.com/).*

**Structure:**
| Target_Countries | Country_Code |
| :--- | :--- |
| Austria | at |
| Germany | de |

#### Sheet 3: Filter_Skills
Enter the skills you want the workflow to prioritize.
> *Note: Since job postings use different naming conventions (e.g., .NET vs .Net), it is advised to include common variations.*

**Structure:**
| Skill Keywords |
| :--- |
| Python |
| .Net |

#### Sheet 4: Filter_Place_of_Work
Define your preferred cities or remote work status.
> *Note: If searching internationally, enter city names in both the local language and English.*

**Structure:**
| Place of Work Keywords |
| :--- |
| Remote |
| Gothenburg |

#### Sheet 5: Filter_Roles
Define the job titles you are searching for.
> *Note: For international searches, it is advised to enter roles in both the local language and English.*

**Structure:**
| Roles |
| :--- |
| DevOps Engineer |
| Scrum Leader |

#### Sheet 6: Filter_Time
Specify your desired work hours or employment types.
> *Note: Include language variations, e.g., "Student Work" (English) and "Studentarbete" (Swedish).*

**Structure:**
| Time Keywords |
| :--- |
| Full-Time |
| 20h |

#### Sheet 7: Log_Found_Jobs
A special sheet that the workflow uses for deduplication. You do not need to manually update this once configured.

**Structure:**
| Job_URL | Date Found | Status |
| :--- | :--- | :--- |
| [URL] | [Timestamp] | Not Applied |

## Logic and Troubleshooting

* **Keyword Validation:** A JavaScript node filters data against mandatory criteria. If no matches are found, the workflow terminates to prevent unnecessary LLM token usage.
* **Node execution errors:** If you see "Node hasn't been executed," run the entire workflow via the Execute Workflow button to populate the global context.
* **Multiple Matching Items:** Resolved by the Split Out node after aggregation to ensure 1-to-1 mapping when logging entries back to Google Sheets.