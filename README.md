![Alt text](images\logo.jpg) 
# Cortana: The AI-Native Career Logistics Engine

**Cortana** is a fully autonomous, containerized recruitment pipeline designed to rebuild the legacy job search from the ground up. Instead of manual keyword-stuffing and endless scrolling, Cortana uses a multi-model AI orchestration layer to scrape the market, judge technical compatibility with "Engineering Intuition," and forge tailored LaTeX resumes—all while keeping the human "Commander" in control via Discord.


## 🛠 Tech Stack & Infrastructure

* **Orchestration:** [n8n](https://n8n.io/) (Self-hosted via Docker)
* **Inference:** Google Gemini 2.5 Flash (Architected for high-speed triage)
* **Editing:** Google Gemini 2.5 Pro (Architected for high quality LaTeX code with breaking structure) 
* **Scraping:** SerpAPI (Google Jobs Engine)
* **Communication:** Discord (Human-in-the-loop Gatekeeping)
* **Data Persistence:** Google Sheets API & Local File System
* **Environment:** Docker Compose (with persistent volumes for LaTeX document management)

---

## 🏗 System Architecture

### 1. Data Gathering (Scouting)
The workflow initiates via a manual trigger, firing a high-precision request to **SerpAPI**. It scans for specific roles (e.g., "Machine Learning Engineer") across targeted regions. The raw JSON results are ingested and handled via a **Split-In-Batches** loop ("Loop Over Items") to ensure sequential processing and rate-limit compliance.

### 2. Cognitive Triage (The Judge)
Unlike a standard ATS that looks for keywords, **The Judge (Gemini 2.5 Flash)** is prompted as a Senior Technical Recruiter.
* **Semantic Mapping:** It identifies transferable logic (e.g., mapping Mechanical Engineering "Root Cause Analysis" to Software "Reliability Engineering").
* **Noise Filtering:** Explicitly instructed to ignore irrelevant sections to prevent score dilution.
* **Scoring Engine:** Generates a 0-100 fit score based on a factual analysis of the **Master Resume** versus the **Job Description**.

### 3. The Neural Gate (Human-in-the-Loop)
Cortana utilizes a logic-based **Switch** to handle ambiguity:
* **High Fit (>=75):** Automated resume production proceeds immediately.
* **Low Fit (<=50):** Logged to the "Resume not made" Google Sheet for later audit and discarded from the active loop.
* **Medium Fit (50-75):** Triggers a **Discord Bot Notification**. Cortana sends the score, reasoning, and job link to the user’s phone. The workflow pauses until the user clicks **Approve** or **Disapprove**.

### 4. Precision Forging (The Editor)
Once a role is approved, **The Editor** ingests the raw LaTeX code of your master resume.
* **The Factuality Lock:** Strictly forbidden from altering experience bullet points to ensure 100% integrity.
* **Strategic Reordering:** Dynamically elevates relevant projects and skills based on the Judge's specific findings.
* **Automated Production:** Cortana creates a dedicated local directory for the company and writes the tailored `.tex` file to disk.

### 5. Operational Persistence
Every run is documented in a centralized **Google Sheets** ledger, recording the reasoning, fit score, and the specific location of the generated resume on the host machine.

![Alt text](images\n8n_workflow.png) 

---

## 🚀 Setup & Deployment

### 1. Environment Variables (`.env`)
```env
SerpAPIKey=your_api_key
discordServerId=your_server_id
discordChannelURL=your_webhook_url
sheetsID=your_google_sheets_spreadsheet_id
```

### 2. Deployment (`compose.yaml`)

```
docker compose up -d
```