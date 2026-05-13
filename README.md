# 🌿 Bibliophile Concierge

An enchanted, AI-powered library management system built with **n8n** — one unified workflow with four intelligent sub-flows tending your entire reading garden.

> **Live on Railway:** [View Workflow](https://n8n-production-a7830.up.railway.app/workflow/M25bDNquM81jUjQh)
> **Landing Page:** [bibliophile-concierge](https://github.com/wardha-netizen/bibliophile-concierge)

---

## 📁 Repository Structure

```
bibliophile-concierge/
├── bibliophile-concierge-complete.json   ← THE combined n8n workflow (import this)
├── index.html                            ← Landing page
├── workflows.html                        ← All 4 form URLs + webhook IDs
├── architecture.html                     ← System architecture diagram
├── summaries/                            ← AI-generated book archive markdowns
│   ├── Archive_2026-05-08.md
│   └── Archive_2026-05-10.md
└── README.md
```

---

## ⚡ One File. Four Sub-Flows.

The entire system lives in **one importable JSON** — all four sub-flows, all Airtable mappings, all AI nodes.

### Import URL (paste directly in n8n → Import from URL)

```
https://raw.githubusercontent.com/wardha-netizen/bibliophile-concierge/main/bibliophile-concierge-complete.json
```

| Sub-Flow | Purpose | Entry Point | Webhook ID |
|---|---|---|---|
| 📖 Acquisition | Add books (multilingual) | Form trigger | `6b4f9f6a-…-332547cc6f00` |
| 📊 Inventory | Issue / return + daily weather suggestion | Form + Cron | `ff3c257e-…-1a72ed65c511` |
| 🔍 Engagement | Semantic search + auto-archive | Form + Cron | `fd79b2ae-…-beaa417e16ca` |
| 📱 Team Hub | WhatsApp book requests + daily digest | Form + Cron | `d18624cb-…-bffc5ddf3fe5` |

---

## 🚀 Setup — 7 Steps

### 1. Import the Workflow
In your n8n instance → Workflows → ⋯ → **Import from URL**, paste the raw URL above.
Or download `bibliophile-concierge-complete.json` and use **Import from File**.

### 2. Google OAuth
Credentials → New → **Google OAuth2**
One credential covers Gmail, Sheets, Drive, Calendar, and Tasks. Grant all scopes.

### 3. Airtable PAT
Visit [airtable.com/create/tokens](https://airtable.com/create/tokens) → create a token with `data.records:read` and `data.records:write` on the **Bibliophile** base.

```
Base ID:  apptGlLDt9VHCE9ey
Table ID: tblENLBbFVBYaEahV  (Books)
```

### 4. Groq AI (OpenAI-compatible, free)
Credentials → New → **OpenAI**

```
Base URL:  https://api.groq.com/openai/v1
API Key:   your key from console.groq.com
Model:     llama-3.3-70b-versatile
```

### 5. GitHub PAT
GitHub → Settings → Developer Settings → Fine-grained PATs → create a token with **Contents: Read & Write** on this repo.

### 6. Airtable — Books Table Schema
Create a base called **Bibliophile** with a **Books** table containing these fields:

| Field | Type | Field | Type |
|---|---|---|---|
| Book Title | Text | Lending Status | Select |
| Author | Text | Added By | Select |
| Language | Text | Format | Select |
| Genre | Select | Price | Number |
| Summary | Long Text | Rating | Number |
| ISBN | Text | Tags | Text |
| Publisher | Text | Availability Worldwide | Select |
| Publication Date | Date | Book Condition | Select |
| Edition | Text | Is Archived | Checkbox |
| Number of Pages | Number | Motivational Quotes | Long Text |
| Cover Image | Attachment | Related Events | Text |
| Attachment Summary | Long Text | | |

### 7. Activate & Use
Toggle the workflow **Active** in n8n. Share the form URLs with your team:

| Form | URL |
|---|---|
| 📖 Book Intake | `https://n8n-production-a7830.up.railway.app/form/6b4f9f6a-6d9f-4c2d-8764-332547cc6f00` |
| 📊 Loan Manager | `https://n8n-production-a7830.up.railway.app/form/ff3c257e-b451-4071-9cc9-1a72ed65c511` |
| 🔍 Book Search | `https://n8n-production-a7830.up.railway.app/form/fd79b2ae-06be-4762-9e7a-beaa417e16ca` |
| 📱 Book Request | `https://n8n-production-a7830.up.railway.app/form/d18624cb-ad72-47e9-aa13-bffc5ddf3fe5` |

---

## 🔧 Tech Stack

| Service | Role |
|---|---|
| **n8n** (Railway Cloud) | Workflow automation engine |
| **Groq AI** — Llama 3.3 70B | Genre classification, AI summaries, search ranking |
| **Airtable** | Primary book database (`apptGlLDt9VHCE9ey`) |
| **Google Sheets** | Backup + archive logs (`Bibliophile Vault`) |
| **Gmail** | Overdue alerts, search results, daily quotes |
| **Google Calendar** | Return-date reminders |
| **Google Drive** | PDF document search |
| **Google Books API** | Book metadata + cover images |
| **Open Library API** | Cover image fallback (no key needed) |
| **WhatsApp Cloud API** | Book request alerts + daily team digest |
| **Discord Webhooks** | New book announcements + daily suggestions |
| **OpenWeatherMap** | Karachi weather → genre mapping |
| **YouTube Data API v3** | Book highlight videos for archives |
| **GitHub REST API** | Push archive markdown files |

---

## 👩‍💻 Team

| Member | Role | Sub-Flow |
|---|---|---|
| **Wardha Khalid** | Lead Developer & System Architect | 📖 Acquisition + 📱 WhatsApp Hub |
| **Teammate 1** | Inventory & Loan Specialist | 📊 Loan Manager |
| **Teammate 2** | Engagement & Search Specialist | 🔍 Semantic Search |

---

*Built with love in Karachi 🇵🇰 · University Project · Automation & AI · 2025*
