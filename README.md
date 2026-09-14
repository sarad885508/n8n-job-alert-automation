# 🚀 Automated DevOps & Cloud Job Alerts using n8n

A production-ready, zero-cost automated workflow built with **n8n** that monitors job boards via **RapidAPI (JSearch v2)**, filters openings for specific Cloud/DevOps roles and target product companies, and sends daily aggregated HTML reports via **Gmail SMTP**.

---

## 🏗️ Architecture

```text
[Schedule / Manual Trigger]
         ⬇
[HTTP Request Node] (JSearch v2 API - RapidAPI)
         ⬇
[Code Node (JavaScript)] (Custom filtering & Responsive HTML Table)
         ⬇
[Email Node (SMTP)] (Delivers digest to Gmail)

# 🚀 Automated DevOps & Cloud Job Alert Pipeline (n8n + JSearch)

An automated, self-hosted job alert pipeline built with **n8n** that fetches live job openings via **RapidAPI (JSearch v2)**, filters them for target Cloud/DevOps competencies and top tier product companies, formats them into a clean HTML email digest, and dispatches them via **Gmail SMTP**.

---

## 📌 Architecture & Workflow

```text
+-----------------------+     +------------------------+
|  Schedule / Manual    | --> |   HTTP Request Node    |
|  Trigger Node (Daily) |     |  (JSearch v2 RapidAPI) |
+-----------------------+     +------------------------+
                                           |
                                           v
+-----------------------+     +------------------------+
|   Send Email Node     | <-- | Code in JavaScript     |
|   (Gmail via SMTP)    |     | (Filter & HTML Table)  |
+-----------------------+     +------------------------+

🌟 Key FeaturesZero-Cost & Quota-Optimized: Built around a per-request billing model (200 requests/month hard limit) to prevent quota blowouts from multi-record responses.Granular Role Matching: Filters target competencies: DevOps, Platform Engineering, SRE, Linux Admin, AKS, Kubernetes, Azure, and GCP.Company Whitelisting: Highlights and prioritizes openings from tier-1 MNCs and product enterprises (e.g., Deloitte, HCL, Samsung, Salesforce, Cisco, Google).Region-Accurate Results: Configured explicitly for Indian tech hubs and global remote roles, bypassing default US biases.Direct Apply Links: Generates a mobile-friendly, responsive HTML table with direct links to job postings.🛠️ Tech StackWorkflow Orchestration: n8n (Self-hosted on macOS / Docker)Data Provider: JSearch v2 API via RapidAPIScripting / Data Transformation: JavaScript (Node.js runtime inside n8n)Notification Layer: Gmail SMTP with Google App Passwords📂 Project StructurePlaintextn8n-job-alert-automation/
├── .gitignore
├── .env.example
├── README.md
├── workflow.json            # Sanitized n8n workflow export
└── scripts/
    └── filter_and_format.js # Pure transformation logic for testing
🚀 Quickstart Guide1. PrerequisitesNode.js v18+ or Docker DesktopA free RapidAPI account subscribed to the JSearch Basic planA Gmail account with 2-Step Verification and an App Password generated2. Clone & SetupBashgit clone [https://github.com/](https://github.com/)<your-username>/n8n-job-alert-automation.git
cd n8n-job-alert-automation
3. Launch n8n LocallyBash# Via npx
npx n8n

# Or via Docker
docker run -it --rm --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n
4. Import & ConfigureOpen http://localhost:5678 in your browser.Navigate to Workflows > Import from File and upload workflow.json.Open the HTTP Request Node:Add your RapidAPI key to the x-rapidapi-key header.Open the Send an Email Node:Configure your Gmail SMTP credentials using your generated Google App Password.Save the workflow and click Execute Workflow to test.⚙️ Engineering Challenges & TroubleshootingChallengeRoot CauseResolutionRapidAPI Quota Burnout (101% in 2 calls)Initial LinkedIn API used object-based billing (each returned job counted against the 25 objects/mo limit).Migrated to JSearch v2, which enforces a per-request hard quota (200 calls/month).404 Not Found on API RequestOutdated /search endpoint was called instead of the newer version.Updated API path to /search-v2.Empty Email DigestData schema mismatch between APIs (organization vs employer_name, url vs job_apply_link).Refactored JavaScript transformation logic to dynamically map JSearch v2 schema keys.jobs is not iterable Runtime ErrorJSearch v2 nests items under data.jobs, while the initial script expected a direct array under data.Added safe fallback traversal: rawInput.data?.jobs || [].Foreign/US-tagged Job ListingsAPI defaulted to country: us and small batch pagination (num_pages: 1).Explicitly passed country: in and scaled num_pages to 5 to fetch a 50–100 job sample pool.🔒 Security & Best PracticesNever commit .env files or raw API keys.When exporting n8n workflows, ensure all private keys in headers and SMTP credentials are fully sanitized into placeholder strings (YOUR_KEY_HERE).
