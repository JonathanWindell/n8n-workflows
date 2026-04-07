# Job Searcher & AI Classifier Workflow Suite
![n8n](https://img.shields.io/badge/n8n-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)
![Google Gemini](https://img.shields.io/badge/google%20gemini-8E75B2?style=for-the-badge&logo=google%20gemini&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-%2334A853?style=for-the-badge&logo=googlesheets&logoColor=white)

A smart low-code workflow that takes care of finding job postings that match your criteria. The workflow uses Google Sheets, Large Language Models (LLM) & HTTP Requests to scrape APIs from popular job posting sites and match job postings based on your chosen keywords. 

## Highlights
* **LLM Information Extraction:**
Using the Information Extraction node in n8n to summarize found and matched job postings and format each job posting in JSON format for easy construction of a beautiful daily email straight to your inbox. 
* **Daily Gmail Summary:** When the workflow has finished analyzing found job postings, it sends them directly to your inbox in a formatted email every day with the link to the job so you never miss a job opportunity. 
* **Agnostic LLM:**
By default, it is configured for Google Gemini, but you can swap out the LLM node for OpenAI, Anthropic, or any other supported provider in n8n.
* **Relational Data Storing:**
Using Google Sheets to create a relational database structure that allows for easy access and simple changing of keywords to search for jobs. Did you learn a new skill and want to find job postings based on that skill? No problem, just enter the new skill into Google Sheets and let the workflow do the work for you. No more entering code nodes to change hardcoded values!
* **Redundancy Safe:**
Using Google Sheets, the workflow appends job postings that have been sent to the user so that the workflow keeps track of all jobs it has already found. 

## Overview
This workflow triggers every day at 8 AM and starts in the Master Workflow which contains two sub-workflow nodes that follow each other when the other one is done due to token restraints of the Gemini free version. If you have a paid subscription, you can easily configure both workflows to start at the same time! 

It then passes the found job postings that match your keywords and validates them against your chosen skills, roles, place of work and time (hours per week) and formats them into two daily emails that end up under your own chosen label in your mail. Because the Google Sheets is structured for modularity, you can easily add to or remove from any of the sheets. 

> ***Note**: While this template uses the native Gmail trigger, you can easily swap the trigger node for a standard IMAP node to support Outlook or custom domains.*

## Architecture

### Job Searcher Wide - Workflow
* [Job Searcher Wide](./wide-workflow/architecture.md)

> ***Note**: In the node "Validate Against Keywords" it's currently set to find "Remote" & "Part-Time" job postings. If this is something you wish to change it's this code snippet you need to change.*

```json
// This you have to change if not correct search words. 
  if (isRemote && isPartTime) {
    let score = 0;
```

### Job Searcher Adzuna - Workflow
* [Job Searcher Adzuna](./adzuna-workflow/architecture.md)

### Master Job Searcher - Workflow
* [Job Searcher Master](./master-workflow/architecture.md)

## Prerequisites
Before importing these workflows, ensure you have:
1. A running **n8n instance** (Self-hosted via Docker, Desktop app, or n8n Cloud).
2. A **Gmail Account** configured with OAuth credentials in n8n (or standard IMAP credentials).
3. A **Google Sheets Account** configured with OAuth credentials in n8n (or standard IMAP credentials).
4. An API key for your **LLM of choice** (Gemini, OpenAI, etc.).

## Usage Instructions
1. Copy the contents of `workflow.json`.
2. Open your n8n workspace, create a new workflow, and press `CTRL + V` (or `Cmd + V`) to paste the nodes.
3. Follow the Configuration Steps below to connect your accounts.

> ***Note**: Every workflow has placeholders such as [YOUR_SPREADSHEET_ID] where you have to configure these yourself.*

## Configuration Steps

Because this workflow interacts with your personal labels and chat IDs, you will need to map a few variables after importing:

### 1. Connect Credentials
Double-click the following nodes and select or create your credentials:
* **Gmail Trigger** & all **Gmail Action** nodes.
* **Google Sheets** & all **Google Sheet** nodes.
* **Google Gemini Chat Model** (Or delete it and attach your preferred LLM node).

### 2. Set up Google Sheets 
This is where these workflows really shine! These workflows use Google Sheets constructed as a relational database. You can copy the structure below and enter your own keywords. 

**Sheet 1: Job URLs**
This sheet is where the HTTP links are stored so that the workflow can easily use them to scrape. Note that this workflow has only been tested with endpoints that return JSON formatted data. There is no node that cleans HTML scrapings. 

**Structure Example:**
- Row 1: https://remoteok.com/remote-jobs.json
- Row 2: https://himalayas.app/jobs/api

**Sheet 2: Adzuna_Job_URLs**
The Adzuna API is a bit different since it uses country codes to search for job postings. Given this difference, the structure of the Google Sheet has to look a bit different. This is something the workflow already takes care of, so just ensure your Google Sheet looks like this. 

> ***Note**: Adzuna is an API that you have to sign up for, which you can do here - [Adzuna API](https://developer.adzuna.com/).*

**Structure Example:**
| Target_Countries | Country_Code |
| :--- | :--- |
| Austria | at |
| Belgium | be |
| Schweiz | ch |
| Germany | de |
| Netherlands | nl |
| Italy | it |
| Spain | es |
| Poland | pl |
| Great Britain | gb |
| France | fr |

**Sheet 3: Filter_Skills**
The skills sheet is where you enter the skills you have that you want the workflow to take into account when filtering the different job postings. 

> ***Note**: Given that job postings might write keywords differently, e.g., .NET or .Net, it is always good to include both variations.*

**Structure Example:**
| Skill Keywords |
| :------------- |
| Python |
| Java |
| .Net |
| D2 Diagrams |

**Sheet 4:** **Filter_Place_of_Work**
Place of work is where you enter the city or how you would like to work, such as remote, part-time remote or in chosen cities you wish to work from. 

> ***Note**: If you search for job postings in different countries, it is advised to enter the name of the city in both the local language and English.*

**Structure Example:**
| Place of Work Keywords |
| :------------- |
| Remote |
| Gothenburg |
| Part-Time Remote |
| Copenhagen |

**Sheet 5:** **Filter_Roles**
Roles are where you choose the roles you wish to search for, such as DevOps Engineer, Scrum Master etc. 

> ***Note**: If you search for job postings in different countries, it is advised to enter the name of the role in both the local language and English.*

**Structure Example:**
| Roles |
| :------------- |
| DevOps Engineer |
| IT-Administrator |
| AI Engineer |
| Scrum Leader |

**Sheet 6:** **Filter_Time**
Time is where you choose how much you would like to work. This could be full-time or part-time, or you enter exactly how many hours you wish to work. 

> ***Note**: If you search for job postings in different countries, it is advised to enter the time spans in both the local language and English, e.g., "Student Work" (English) & "Studentarbete" (Swedish).*

**Structure Example:**
| Time Keywords |
| :------------- |
| Full-Time |
| Part-Time |
| 20h |
| Student Work |

**Sheet 7:** **Log_Found_Jobs**
Log jobs is special and you really don't have to think about it after you have set it up. Logged jobs is a sheet that the workflow appends found jobs to so it can remember what jobs it has already found and sent to you. 

**Structure Example:**
| Job_URL | Date Found | Status |
| :------| :--------- | :------|
| Found Job Posting 1 | 2026-03-07 | Not Applied |
| Found Job Posting 2 | 2026-03-08 | Not Applied |
