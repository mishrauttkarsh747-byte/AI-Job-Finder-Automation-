# 🤖 AI Job Finder & Resume Matching Automation

> **Turn a resume into an intelligent, automated job-search pipeline.**

An AI-powered job discovery and matching system built with **n8n, LLMs, APIs, Google Sheets, and document automation**.

Instead of manually searching hundreds of job listings, opening each posting, comparing requirements with a resume, and maintaining a spreadsheet, this workflow automates the repetitive parts of the process and prioritizes the opportunities with the strongest candidate-job fit.

---

## 🚀 Why This Project?

Job searching is surprisingly repetitive.

A candidate often has to:

* Search multiple job platforms
* Generate different search queries
* Open individual job postings
* Read long descriptions
* Compare requirements with their resume
* Identify missing skills
* Remove duplicate jobs
* Decide which jobs are worth applying for
* Customize their resume
* Track opportunities

This project turns that process into an **AI-assisted automation pipeline**.

### The core idea

```text
Resume
   ↓
Candidate Profile
   ↓
Intelligent Job Search
   ↓
Individual Job Detection
   ↓
Job Data Extraction
   ↓
Duplicate Removal
   ↓
AI Candidate ↔ Job Matching
   ↓
Match Score
   ↓
Eligibility Filter
   ↓
Top Job Opportunities
   ↓
ATS Resume Generation
   ↓
Google Sheets / Google Drive
```

---

# ⭐ What Makes This Project Different?

This is not just an LLM wrapper.

The workflow combines:

**Workflow Automation + Data Processing + API Integration + LLM Reasoning + Decision Logic + Document Generation**

The system does not simply ask an AI:

> "Is this job good for me?"

Instead, it creates a structured pipeline that processes the candidate and job independently, cleans the data, evaluates multiple matching dimensions, applies business rules, and produces an actionable shortlist.

---

# 🧠 Key Features

### 📄 1. Resume Understanding

The candidate uploads a resume.

The workflow extracts structured information such as:

* Name
* Email
* Phone
* Education
* Experience
* Skills
* Projects
* Preferred roles
* Preferred locations
* Work mode
* Years of experience

The resume parser is instructed to extract information from the actual resume rather than inventing candidate information.

---

### 🔎 2. Personalized Job Discovery

The candidate profile is used to create relevant job-search queries.

Instead of relying on a single generic search, the workflow can search based on the candidate's actual:

* Skills
* Technologies
* Projects
* Experience
* Preferred roles

This makes the search more personalized.

---

### 🔗 3. Individual Job URL Detection

One of the important problems solved by this project is the difference between:

```text
Job Search Page
        ≠
Individual Job Posting
```

Search engines can return category pages, search pages, company pages, or actual job postings.

The workflow therefore contains a dedicated **Filter Individual Job URLs** stage before AI matching.

This prevents the LLM from evaluating an entire search-results page as if it were a single job.

---

### 🧹 4. Duplicate Removal

The same job can appear through multiple search queries.

The workflow therefore removes duplicate opportunities before expensive downstream processing.

```text
Job A
Job B
Job A
Job C
Job B

        ↓

Job A
Job B
Job C
```

This reduces unnecessary API/LLM calls and keeps the final results cleaner.

---

### 🧠 5. AI-Powered Candidate ↔ Job Matching

Each individual job is evaluated against the candidate profile and resume.

The AI evaluates multiple dimensions:

| Matching Dimension   | Purpose                                        |
| -------------------- | ---------------------------------------------- |
| Role Match           | Does the job fit the candidate's target role?  |
| Skill Match          | How many required skills are present?          |
| Experience Match     | Does experience align with requirements?       |
| Project Match        | Do projects demonstrate relevant capabilities? |
| Education Match      | Does education satisfy requirements?           |
| Location / Work Mode | Does location/work mode align?                 |

The workflow produces structured JSON instead of relying on free-form AI responses.

Example:

```json
{
  "match_score": 85,
  "eligible": true,
  "role_match": 95,
  "skill_match": 90,
  "experience_match": 70,
  "project_match": 90,
  "education_match": 100
}
```

---

# 📊 6. Explainable Match Scoring

The system doesn't only return a score.

It also identifies:

* Matched skills
* Missing skills
* Required skills missing
* Preferred skills missing
* Role compatibility
* Experience compatibility
* Project compatibility
* Education compatibility
* Location/work-mode compatibility
* Reason for the final decision

This makes the recommendation more explainable.

---

# 🎯 7. Eligibility Filtering

The workflow applies a defined threshold to avoid sending every discovered job downstream.

### Matching scale

|  Score | Classification |
| -----: | -------------- |
| 90–100 | 🟢 Excellent   |
|  80–89 | 🟢 Strong      |
|  70–79 | 🟡 Good        |
|  60–69 | 🟠 Weak        |
|   0–59 | 🔴 Poor        |

A job becomes eligible when:

```text
match_score >= 70
```

This converts an AI recommendation into an **automated business rule** rather than leaving the final decision entirely to free-form AI.

---

# 🏆 8. Best Opportunities First

Instead of presenting an unfiltered list of jobs, the workflow prioritizes stronger opportunities.

The current design retains the strongest opportunities for downstream processing, including a Best-20 style shortlist.

This helps reduce:

> 100+ raw opportunities

into something much more actionable:

> **The jobs most worth reviewing first.**

---

# 📄 9. ATS-Oriented Resume Generation

For selected opportunities, the workflow can generate an ATS-oriented resume tailored toward the target role.

The purpose is not to fabricate experience.

Instead, the system can reorganize and emphasize relevant:

* Skills
* Projects
* Experience
* Technologies
* Keywords

based on the selected job.

The generated resume should always be reviewed by the candidate before being used.

---

# 📊 10. Google Sheets Job Tracking

Selected job opportunities can be stored in Google Sheets.

This creates a centralized opportunity database instead of leaving jobs scattered across browser tabs.

Potential stored information includes:

```text
Job Title
Company
Job URL
Location
Work Mode
Experience Required
Match Score
Eligibility
Matched Skills
Missing Skills
Reason
```

Google Sheets was intentionally selected as a simple, accessible tracking layer without requiring a separate database or frontend application.

---

# ⚙️ Architecture

```text
                    ┌──────────────────┐
                    │  Resume Upload   │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ Resume Extraction│
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ Candidate Profile│
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ Search Generation│
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │   Job Retrieval  │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ Individual Job   │
                    │ URL Filtering    │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ Attach Candidate │
                    │     Profile      │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ Deduplication +  │
                    │   Job Limiting   │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │   AI Matching    │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ Structured Score │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ Score >= 70 ?    │
                    └───────┬───┬──────┘
                            YES  NO
                             ↓    ↓
                    ┌─────────────┐
                    │ Top Jobs     │
                    └──────┬──────┘
                           ↓
                    ┌─────────────┐
                    │ ATS Resume   │
                    │ Generation   │
                    └──────┬──────┘
                           ↓
                    ┌─────────────┐
                    │ Google Drive │
                    │ + Sheets     │
                    └─────────────┘
```

---

# 🔧 Technology Stack

| Technology              | Purpose                             |
| ----------------------- | ----------------------------------- |
| **n8n**                 | Workflow orchestration              |
| **Google Gemini / LLM** | Resume understanding & job matching |
| **HTTP APIs**           | Job/search/API integration          |
| **Google Sheets**       | Job tracking                        |
| **Google Drive**        | Document storage                    |
| **JSON**                | Structured data exchange            |
| **Prompt Engineering**  | AI behavior & structured outputs    |
| **Document Processing** | Resume extraction & generation      |

---

# 🔄 Workflow Components

The workflow is divided into small stages instead of building one large AI operation.

Important components include:

```text
Resume Upload
      ↓
Resume Parsing
      ↓
Candidate Profile
      ↓
Job Search
      ↓
Filter Individual Job URLs
      ↓
Attach Candidate Profile
      ↓
Remove Duplicates + Limit Jobs
      ↓
Basic LLM Chain
      ↓
Parse Match
      ↓
Active + Eligible + 70%
      ↓
Resume Generation
      ↓
File Conversion
      ↓
Google Drive
      ↓
Google Sheets
```

The exported workflow contains dedicated stages for candidate-profile processing, individual-job filtering, deduplication, LLM matching, match parsing, and final storage.

---

# 🧩 Important Design Decisions

### Why n8n?

n8n provides a visual orchestration layer where APIs, data transformations, AI models, file operations, and Google services can be connected in a single workflow.

### Why separate the workflow into nodes?

Each node has a specific responsibility.

This makes the system:

* Easier to debug
* Easier to modify
* Easier to understand
* Easier to scale

### Why filter individual job URLs?

Because an AI model should evaluate an actual job posting rather than an entire search-results page.

### Why remove duplicates?

Duplicate jobs waste API requests and AI processing.

### Why use a score threshold?

A numeric threshold allows the workflow to automatically decide which opportunities deserve further processing.

### Why use AI matching?

Traditional keyword matching can miss relationships between:

```text
Candidate Experience
        ↕
Projects
        ↕
Skills
        ↕
Job Requirements
```

LLM-based matching can evaluate these relationships together.

---

# 🚧 Challenges Solved

Building the workflow involved several practical automation challenges.

### 1. Search pages vs individual job pages

Search results often returned broad pages instead of individual postings.

**Solution:** Introduced individual-job URL filtering.

### 2. Duplicate opportunities

Multiple search queries could return the same job.

**Solution:** Added deduplication before AI processing.

### 3. LLM API rate limits

Processing many jobs simultaneously can trigger API limits.

**Solution:** Controlled batching and delay logic.

The workflow uses batch processing with controlled delays to reduce request bursts.

### 4. Inconsistent job data

Different job sources expose different fields.

**Solution:** The matching prompt checks multiple possible job fields such as title, description, company, location, skills, requirements, and experience.

### 5. AI output consistency

Free-form AI responses are difficult to process automatically.

**Solution:** The workflow forces structured JSON output.

### 6. Credential security

API keys and private credentials should never be committed to a public repository.

**Solution:** Credentials should be stored through n8n credential management or environment variables.

---

# 📈 Why This Project Has Real-World Value

The project follows a clear:

```text
Problem
   ↓
Automation
   ↓
AI Decision Support
   ↓
Actionable Output
```

### Problem

Manual job searching is time-consuming and difficult to personalize.

### Solution

Automate candidate understanding, job discovery, filtering, matching, ranking, and resume preparation.

### Value

The candidate spends less time searching and more time reviewing and applying to relevant opportunities.

---

# 💼 What This Project Demonstrates to Recruiters

This project demonstrates practical experience with:

* Workflow automation
* n8n
* REST APIs
* HTTP requests
* LLM integration
* Prompt engineering
* Structured JSON
* Resume parsing
* Data transformation
* Data filtering
* Deduplication
* AI-assisted ranking
* Rule-based automation
* Document generation
* Google Sheets integration
* Google Drive integration
* API rate-limit handling
* Credential security
* End-to-end system design

The project therefore demonstrates considerably more than simply "using AI."

---

# 🗂️ Recommended Repository Structure

```text
AI-Job-Finder-Automation/
│
├── README.md
│
├── workflow/
│   └── ai-job-finder-workflow.json
│
├── prompts/
│   ├── candidate-profile-prompt.txt
│   ├── job-matching-prompt.txt
│   └── ats-resume-prompt.txt
│
├── screenshots/
│   ├── workflow-overview.png
│   └── successful-execution.png
│
├── docs/
│   └── project-documentation.md
│
└── .gitignore
```

This structure keeps the README recruiter-friendly while allowing deeper technical documentation to live separately.

---

# 🔐 Security

**Never commit:**

```text
API Keys
Passwords
Bearer Tokens
Cookies
Google Credentials
Private Resume Data
Personal Contact Information
```

Before uploading the workflow to GitHub:

* Remove hard-coded API keys
* Remove private credentials
* Remove personal resume information
* Check exported n8n JSON carefully
* Use n8n credentials/environment variables
* Review generated resumes before real applications

---

# 🔮 Future Roadmap

The project can evolve into a complete AI-powered career automation platform.

### Phase 1 — Better Job Discovery

* [ ] Add more job sources
* [ ] Salary filtering
* [ ] Location filtering
* [ ] Remote / Hybrid / On-site filtering
* [ ] Experience-level filtering
* [ ] Employment-type filtering

### Phase 2 — Better Intelligence

* [ ] Job freshness scoring
* [ ] Application deadline detection
* [ ] Improved job deduplication using job IDs
* [ ] Better semantic skill matching
* [ ] Human feedback loop
* [ ] Match-score calibration

### Phase 3 — Application Automation

* [ ] Automatic cover-letter generation
* [ ] Application tracking
* [ ] Saved / Applied / Interview / Rejected / Offer stages
* [ ] Recruiter information
* [ ] Personalized application preparation

### Phase 4 — Notifications & Analytics

* [ ] Email notifications
* [ ] Telegram notifications
* [ ] WhatsApp notifications
* [ ] Job-search analytics dashboard
* [ ] Application conversion tracking
* [ ] Interview-rate analytics

These improvements would move the project from an automation prototype toward a more complete production-grade career platform.

---

# 📊 Potential Metrics

A future version can track:

```text
Jobs Discovered
       ↓
Valid Job URLs
       ↓
Duplicates Removed
       ↓
Jobs AI Evaluated
       ↓
Average Match Score
       ↓
Eligible Jobs
       ↓
Applications
       ↓
Interviews
       ↓
Offers
```

Possible KPIs:

* Total jobs discovered
* Valid individual jobs
* Duplicate jobs removed
* Average match score
* Number of eligible jobs
* Number of resumes generated
* Applications submitted
* Interview conversion rate

---

# ⚠️ Limitations

This system is an **AI-assisted decision-support system**, not an automated hiring predictor.

Important limitations include:

* AI match scores are estimates
* Job data depends on the source
* LLM outputs can occasionally be inconsistent
* Generated resumes should be reviewed
* API limits can affect processing capacity
* A high match score does not guarantee an interview or job offer

These limitations are part of the reason the workflow keeps human review in the final application process.

---

# 🎯 Project Impact

The objective isn't to completely replace the candidate.

It is to remove the repetitive work around the candidate.

```text
BEFORE

Search
 ↓
Open Job
 ↓
Read
 ↓
Compare Resume
 ↓
Track Job
 ↓
Customize Resume
 ↓
Repeat...

AFTER

Upload Resume
      ↓
AI Job Finder
      ↓
AI Matching
      ↓
Ranked Opportunities
      ↓
ATS Resume
      ↓
Human Review
      ↓
Apply
```

The candidate remains responsible for the final decision.

---

# 🏆 Key Takeaway

> **An end-to-end AI automation system that converts a resume into a personalized job-search pipeline, evaluates candidate-job fit using LLM-based scoring, prioritizes high-match opportunities, and prepares ATS-oriented application material.**

The strongest part of this project is not any single technology.

It is the combination of:

**Real-world problem solving + workflow automation + APIs + AI reasoning + data processing + decision logic + document generation.**

---

## 👨‍💻 Author

**Uttkarsh Mishra**

CSE — Artificial Intelligence & Machine Learning

Interested in:

* Data Analytics
* AI Automation
* Machine Learning
* Business Intelligence
* Data-driven applications

---

## ⭐ If You Find This Project Interesting

Give the repository a ⭐ and feel free to explore the workflow architecture.

---

## ⚖️ Disclaimer

This project is intended for educational and automation purposes.

AI-generated recommendations, match scores, and resumes should be reviewed by a human before being used for actual job applications.

A high match score does not guarantee selection by an employer.
