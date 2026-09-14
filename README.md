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
