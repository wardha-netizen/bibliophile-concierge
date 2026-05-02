# 📚 Member 2 & Member 3 — Full Explanation + Setup Steps

---

# 🟡 MEMBER 2 — Inventory & Loan Manager

## What this workflow does (plain English)

Member 2 has **3 separate mini-workflows** all in one file:

---

### 🔄 Mini-Workflow A — Loan / Return a Book
**Trigger:** Someone fills a form to issue or return a book

**Step-by-step what happens:**
1. Person opens the **"Issue / Return a Book" form** and fills in:
   - Book title
   - Action: Issue OR Return
   - Friend's name
   - Friend's contact
   - Return due date
2. n8n searches **Airtable** to find that book
3. If **Issue** → Airtable updates the book Status to "Issued", saves friend's name and due date
4. If **Return** → Airtable updates the book Status back to "Available"
5. If Issue, it also:
   - Creates a **Google Calendar** event on the due date
   - Creates a **Todoist** task as a reminder
   - Logs the loan in **Google Sheets**

---

### 🌤️ Mini-Workflow B — Daily Weather-Based Book Suggestion
**Trigger:** Every day at 9:00 AM automatically

**Step-by-step what happens:**
1. n8n wakes up at 9AM
2. Checks **OpenWeatherMap** for today's weather in your city
3. Maps weather → genre:
   - Cloudy/Rain/Drizzle → **Poetry**
   - Clear/Sunny → **Adventure** (Novels)
   - Snow → **Children**
   - Thunderstorm → **Religion**
   - Mist → **Biography**
4. Searches Airtable for an "Available" book in that genre
5. Sends that book suggestion to your **Telegram**

Example message you'd receive:
> 🌧️ Today is a **Poetry Day** (Rain)
> 📖 Suggestion: **Gulistan by Saadi**
> 🌱 Plant Spirit: Lily

---

### ⚠️ Mini-Workflow C — Overdue Book Reminders
**Trigger:** Every day at 10:00 AM automatically

**Step-by-step what happens:**
1. n8n checks Airtable for all books with Status="Issued" AND Due Date is past today
2. For each overdue book, it sends:
   - A **Telegram** message nudging the friend
   - A **Discord** message pinging you so you know

---

## Member 2 — Keys You Still Need

| # | Service | How to get it (free) | Time |
|---|---|---|---|
| 1 | **Telegram Bot** | Already done from Member 1 ✅ | 0 min |
| 2 | **OpenWeatherMap API key** | https://openweathermap.org/api → Sign up → API Keys tab → copy the key | 3 min |
| 3 | **Todoist API token** | https://todoist.com → Settings → Integrations → scroll down → copy Developer Token | 2 min |
| 4 | **Google Calendar** | Already connected via Google OAuth ✅ | 0 min |
| 5 | **Google Sheets** | Already connected ✅ | 0 min |
| 6 | **Discord Webhook URL** | See below | 3 min |

### How to get Discord Webhook URL:
1. Open Discord → go to any channel (or create one called #library-alerts)
2. Click the ⚙️ gear icon next to the channel → **Integrations**
3. Click **Webhooks → New Webhook**
4. Name it "Bibliophile Bot" → click **Copy Webhook URL**
5. That URL is your `DISCORD_WEBHOOK_URL`

---

## Member 2 — Variables to Add in n8n

Go to n8n → ⚙️ Settings → Variables → add each one:

| Variable Name | What to paste |
|---|---|
| `OPENWEATHER_API_KEY` | Key from openweathermap.org |
| `OWNER_CITY` | Your city name in English, e.g. `Lahore` |
| `DISCORD_WEBHOOK_URL` | The webhook URL from Discord |
| `OWNER_TELEGRAM_CHAT_ID` | Already added from Member 1 ✅ |
| `AIRTABLE_BASE_ID` | Already added from Member 1 ✅ |
| `GOOGLE_SHEETS_BACKUP_ID` | Already added from Member 1 ✅ |

## Member 2 — Credentials to Connect in n8n

| Credential | How |
|---|---|
| **Todoist** | Open any Todoist node → Credential → New → paste your API token |
| **Google Calendar** | Open the Google Calendar node → Credential → pick your existing Google connection |
| **Telegram** | Already connected ✅ |
| **Discord** | No credential needed — it uses the webhook URL directly as a variable |

---

## Member 2 — How to Test

**Test A (Loan form):**
1. Click the **📋 Loan Form** node → copy Production URL → open in browser
2. Fill in: Book Title (must already exist in Airtable), Action = Issue, friend's name, a future date
3. Check Airtable → book status should change to "Issued"
4. Check Google Calendar → new event should appear

**Test B (Weather suggestion):**
1. Click the **⏰ Daily 9AM Trigger** node → click **Execute Node** (just this one)
2. Follow the green arrows — eventually Telegram should send you a message
3. If you get an error on the weather node, check that OPENWEATHER_API_KEY and OWNER_CITY are set

**Test C (Overdue check):**
1. In Airtable, manually set one book's Status to "Issued" and Due Date to yesterday
2. Click the **⏰ Daily Overdue Check** node → Execute Node
3. You should get a Telegram message and a Discord ping

---
---

# 🔵 MEMBER 3 — Engagement & Semantic Search

## What this workflow does (plain English)

Member 3 has **3 separate mini-workflows** all in one file:

---

### 🌅 Mini-Workflow A — Daily "Water Your Mind" Quote
**Trigger:** Every day at 8:00 AM automatically

**Step-by-step what happens:**
1. n8n wakes up at 8AM
2. Asks **Groq AI** to generate:
   - A motivational book quote (under 25 words)
   - A unique 6-character code (like a "daily reading code")
   - The book/author it's from
3. Sends a beautiful HTML email via **Gmail** with the quote
4. Also sends it to **Telegram** at the same time

Example email subject: 🌱 Water Your Mind — Code BK7T2X
Example Telegram message:
> 🌅 Water Your Mind
> "A reader lives a thousand lives before he dies."
> — George R.R. Martin
> 🔑 Code: BK7T2X

---

### 🔍 Mini-Workflow B — Telegram Semantic Search
**Trigger:** You send any message to your Telegram bot

**Step-by-step what happens:**
1. You type a character name, quote, or any phrase in Telegram to your bot (e.g. "Hermione" or "be brave")
2. n8n receives that message
3. Simultaneously searches TWO places:
   - **Airtable** — looks in the Notes, Summary, and Title fields of all books
   - **Google Drive** — searches through any PDFs you've uploaded
4. **Groq AI** reads all the results and writes a smart reply telling you which book it found it in
5. Bot sends the reply back to you in Telegram

This is the "Semantic Search" / "Deep Search" feature your teacher mentioned!

---

### 🌙 Mini-Workflow C — Completed Book Social Archiving
**Trigger:** Every evening, checks for books newly marked as "Completed"

**Step-by-step what happens:**
1. n8n looks in Airtable for any book with Status="Completed" that hasn't been posted yet
2. **Groq AI** generates for that book:
   - An Instagram caption (with hashtags)
   - A LinkedIn post (professional summary)
   - A GitHub Markdown file (technical summary)
   - A Pinterest description
   - A YouTube search term (to find a book trailer)
   - A Spotify vibe search (genre-matching music)
3. All of these get posted/saved automatically:
   - **Instagram** post
   - **LinkedIn** post
   - **GitHub** — creates a `.md` file in your summaries folder
   - **Pinterest** — creates a pin
   - **YouTube** — finds and logs a related video
   - **Spotify** — adds a track to your vibe playlist
   - **Dropbox** — backs up the markdown summary
   - **Microsoft Excel** — logs the completed book

---

## Member 3 — Keys You Still Need (in priority order)

### 🟢 Easy ones (do these first — they complete the core demo):

| # | Service | Where | Time |
|---|---|---|---|
| 1 | **Gmail** | Already connected via Google ✅ | 0 min |
| 2 | **Telegram** | Already connected ✅ | 0 min |
| 3 | **Groq** | Already connected ✅ | 0 min |
| 4 | **Google Drive** | Already connected via Google ✅ | 0 min |
| 5 | **GitHub** | github.com → Settings → Developer Settings → Personal Access Tokens (classic) → New → check `repo` → Generate | 3 min |
| 6 | **Dropbox** | n8n → Dropbox node → Credential → New → Connect with Dropbox (OAuth popup) | 3 min |
| 7 | **Spotify** | https://developer.spotify.com → Dashboard → Create App → get Client ID + Secret | 5 min |

### 🟠 Medium ones (for full demo — try after easy ones):

| # | Service | Notes |
|---|---|---|
| 8 | **Pinterest** | https://developers.pinterest.com → create app → get token |
| 9 | **Microsoft Excel** | Microsoft account → OneDrive → n8n Microsoft Excel credential (OAuth) |
| 10 | **LinkedIn** | n8n → LinkedIn node → OAuth |
| 11 | **Instagram** | Needs Facebook Business + Instagram Business account (same as WhatsApp setup) |

---

## Member 3 — Variables to Add in n8n

| Variable Name | What to paste |
|---|---|
| `OWNER_EMAIL` | Your Gmail address |
| `GMAIL_FROM` | Same Gmail address |
| `OWNER_TELEGRAM_CHAT_ID` | Already added ✅ |
| `GITHUB_OWNER` | Your GitHub username (e.g. `wardha-netizen`) |
| `GITHUB_REPO` | `bibliophile-concierge` |
| `SPOTIFY_VIBE_PLAYLIST_ID` | Create a playlist in Spotify → share → copy the ID from the URL |
| `SPOTIFY_DEFAULT_TRACK_ID` | Any Spotify track URI (right-click any song → Share → Copy Song Link) |
| `PINTEREST_BOARD_ID` | Your Pinterest board URL → copy the ID |
| `PINTEREST_TOKEN` | From Pinterest developer app |
| `MS_EXCEL_FILE_ID` | OneDrive Excel file → share → copy ID from URL |

---

## Member 3 — How to Test

**Test A (Daily quote — mini-workflow A):**
1. Click the **🌅 Daily 8AM Trigger** node → Execute Node
2. Follow the chain — Gmail should send you an email, Telegram should send you the quote
3. Check your inbox and Telegram ✅

**Test B (Semantic search — mini-workflow B):**
1. Make sure Telegram bot is active (Workflow 3 must be ON)
2. Open Telegram → message your bot with a word like "love" or a book title
3. Your bot should reply with search results within ~10 seconds ✅

**Test C (Completed book archiving — mini-workflow C):**
1. In Airtable → change any book's Status to "Completed" (leave Posted unchecked)
2. Click the **🌙 Evening Completed Check** node → Execute Node
3. Watch the chain run — GitHub should get a new file, Dropbox should get a backup ✅

---

# 📋 MASTER CHECKLIST — Everything Left To Do

## Member 2 remaining steps:
- [ ] Get OpenWeatherMap API key → add as `OPENWEATHER_API_KEY` variable
- [ ] Set `OWNER_CITY` variable (your city name)
- [ ] Get Discord Webhook URL → add as `DISCORD_WEBHOOK_URL` variable
- [ ] Get Todoist API token → connect Todoist credential in n8n
- [ ] Connect Google Calendar credential (just pick existing Google OAuth)
- [ ] Test all 3 mini-workflows using the steps above
- [ ] Flip Workflow 2 **Active** toggle ON

## Member 3 remaining steps:
- [ ] Add `OWNER_EMAIL` and `GMAIL_FROM` variables (your Gmail)
- [ ] Add `GITHUB_OWNER` = `wardha-netizen` and `GITHUB_REPO` = `bibliophile-concierge`
- [ ] Connect GitHub credential (Personal Access Token)
- [ ] Connect Dropbox credential (OAuth)
- [ ] Connect Spotify credential → add playlist + track variables
- [ ] Test mini-workflow A (daily quote) — just execute the trigger node
- [ ] Test mini-workflow B (Telegram search) — send a message to your bot
- [ ] Test mini-workflow C (completed book) — mark a book Completed in Airtable then execute
- [ ] Flip Workflow 3 **Active** toggle ON
- [ ] Connect remaining social platforms (Pinterest, LinkedIn, Instagram, Excel) when time allows

## Final demo checklist:
- [ ] All 3 workflows Active (green toggle)
- [ ] At least one book in Airtable with Status = Available
- [ ] Form URLs copied and bookmarked (for live demo)
- [ ] Telegram bot responding to search queries
- [ ] GitHub repo showing summaries folder with .md files
- [ ] architecture.html open in a browser tab (to show teacher)
