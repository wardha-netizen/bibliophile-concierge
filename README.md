# 🌿 Bibliophile Concierge — n8n Setup Guide

A 3-workflow, 24-app library management system styled like an enchanted "Tangled" garden.

## What you're getting

| File | Member | Purpose |
|------|--------|---------|
| `01-acquisition-multilingual-librarian.json` | Member 1 | Book intake form → language detect → AI genre + plant spirit → Unsplash cover → Cloudinary host → Airtable + Sheets + WhatsApp confirm |
| `02-inventory-loan-manager.json` | Member 2 | Issue/return form, Calendar + Todoist reminders, daily weather→genre suggestion, overdue Telegram + Discord nudges |
| `03-engagement-semantic-search.json` | Member 3 | Daily AI quote (Gmail+Telegram), Telegram semantic search across Airtable + Drive PDFs, completed-book archiving to Instagram, LinkedIn, GitHub, Pinterest, YouTube, Spotify, Dropbox, Excel |

All 24 apps from your spec are wired up.

---

## 1. Install n8n (free, self-hosted)

```bash
npx n8n
```
Open http://localhost:5678 and create your owner account. Self-hosted n8n is free and unlimited.

> Cloud option: n8n.cloud has a 14-day trial. Self-host is recommended for free use.

---

## 2. Import the 3 workflows

In n8n:
1. Click **Workflows → Import from File**
2. Pick each `.json` file from this folder one at a time
3. Each will appear as inactive — leave them off until credentials are set

---

## 3. Set up free accounts & credentials

### 🔑 Variables (Settings → Variables)

Add these as n8n variables (`$vars.NAME`). All are free:

| Variable | Where to get it (free) |
|---|---|
| `GOOGLE_TRANSLATE_API_KEY` | Google Cloud free tier — enable Translate API |
| `UNSPLASH_ACCESS_KEY` | https://unsplash.com/developers (free, 50 req/hr) |
| `CLOUDINARY_CLOUD_NAME` | https://cloudinary.com signup → Dashboard |
| `CLOUDINARY_UPLOAD_PRESET` | Cloudinary → Settings → Upload → add unsigned preset |
| `AIRTABLE_BASE_ID` | https://airtable.com → create base → copy from URL `appXXXX` |
| `GOOGLE_SHEETS_BACKUP_ID` | Sheet URL `/d/<ID>/edit` |
| `OPENWEATHER_API_KEY` | https://openweathermap.org/api (free 1k calls/day) |
| `OWNER_CITY` | e.g. `Lahore` |
| `OWNER_WHATSAPP_NUMBER` | your number with country code, no `+` |
| `OWNER_TELEGRAM_CHAT_ID` | message @userinfobot on Telegram |
| `WHATSAPP_PHONE_NUMBER_ID` | Meta for Developers → WhatsApp Cloud API (free tier) |
| `OWNER_EMAIL` / `GMAIL_FROM` | your Gmail |
| `DISCORD_WEBHOOK_URL` | Discord channel → Edit → Integrations → Webhooks |
| `GITHUB_OWNER` / `GITHUB_REPO` | your username + a public repo |
| `PINTEREST_BOARD_ID` / `PINTEREST_TOKEN` | https://developers.pinterest.com (free) |
| `YOUTUBE_API_KEY` | Google Cloud → YouTube Data API v3 (free) |
| `SPOTIFY_VIBE_PLAYLIST_ID` / `SPOTIFY_DEFAULT_TRACK_ID` | Spotify → playlist/track URI |
| `MS_EXCEL_FILE_ID` | OneDrive → file ID via Graph Explorer |

### 🔐 Credentials (Credentials → New)

For these, n8n has built-in OAuth — just click **Connect**:
- **Airtable** (Personal Access Token from airtable.com/create/tokens)
- **Google Sheets / Drive / Calendar / Gmail** (one Google OAuth covers all)
- **Telegram** (BotFather → `/newbot` → token)
- **WhatsApp Business Cloud** (Meta for Developers, free 1k convos/mo)
- **Todoist** (Settings → Integrations → API token)
- **GitHub** (Settings → Developer settings → fine-grained PAT)
- **LinkedIn** (OAuth via n8n)
- **Spotify** (OAuth via n8n)
- **Dropbox** (OAuth via n8n)
- **Microsoft Excel** (OAuth via n8n)
- **OpenAI node — point it at Groq for FREE**:
  - Base URL: `https://api.groq.com/openai/v1`
  - API key: from https://console.groq.com (free, very fast)
  - Model already set: `llama-3.3-70b-versatile`

---

## 4. Set up your Airtable base

Create a base called **Bibliophile** with a table **Books** and these fields:

| Field | Type |
|---|---|
| Title | Single line text |
| Author | Single line text |
| Language | Single line text |
| Genre | Single select (Novels, Poetry, Education, Religion, Biography, Science, Children, Other) |
| Plant Spirit | Single line text |
| Summary | Long text |
| Notes | Long text |
| Price (USD) | Number |
| Status | Single select (Available, Issued, Completed) |
| Lent To | Single line text |
| Due Date | Date |
| Cover URL | URL |
| Owner | Single select (Main Garden, Member 1, Member 2, Member 3) |
| Posted | Checkbox |

Mirror the same columns in Google Sheets tab `Books`, plus tabs `Loans` and `Completed`.

---

## 5. Activate the workflows

Once credentials are saved:
1. Open each workflow → toggle **Active** in the top right
2. Webhook URLs appear on the form-trigger nodes — share them as your "intake" and "loan" forms
3. Send a Telegram message to your bot to test semantic search

---

## 6. The "Tangled" / Cartoon Garden look

The form triggers in workflows 1 & 2 already include cartoon-garden CSS:
- Warm cream + sage palette (`#fef6e4`, `#7a9e7e`, `#6b4226`)
- Georgia serif for that storybook feel
- Rounded pill buttons and soft shadows

To customize further, edit the `customCss` field on the form-trigger nodes.

---

## 7. Technical highlights for your documentation

- **Semantic Search**: Workflow 3 searches Airtable `Notes`/`Summary`/`Title` AND Google Drive PDFs in parallel, then an LLM merges + ranks results.
- **Data Integrity**: Every Airtable write is mirrored to Google Sheets (primary vs. backup) and a Microsoft Excel log on completion.
- **Multi-channel notifications**: WhatsApp, Telegram, Discord, Gmail, and platform-native posts (Instagram, LinkedIn, Pinterest, GitHub).
- **Webhooks + AI + DB**: All three boxes from your teacher's checklist are demonstrated in every workflow.
- **Weather-Genre Sync**: A daily cron checks OpenWeatherMap, maps weather → genre, picks an Available book from that shelf, sends to Telegram.

---

## 24-app checklist

✅ n8n Forms ✅ Airtable ✅ Google Sheets ✅ Google Books ✅ Open Library ✅ OpenAI/Groq ✅ Google Translate ✅ WhatsApp ✅ Telegram ✅ Gmail ✅ Google Drive ✅ Dropbox ✅ Cloudinary ✅ Unsplash ✅ Todoist ✅ Google Calendar ✅ Spotify ✅ YouTube ✅ Instagram ✅ LinkedIn ✅ Pinterest ✅ Discord ✅ GitHub ✅ OpenWeatherMap ✅ Microsoft Excel
