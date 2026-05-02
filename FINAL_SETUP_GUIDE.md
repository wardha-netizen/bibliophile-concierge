# 🌿 Bibliophile Concierge — FINAL COMPLETE SETUP GUIDE

## ✅ WHAT WAS REMOVED (no longer needed)
| Removed | Reason |
|---|---|
| Unsplash | Needed paid API key |
| Cloudinary | Needed account setup |
| Spotify | Requires paid subscription to get API |
| Pinterest | Requires developer approval |
| LinkedIn | Complex OAuth + app review |
| Instagram | Meta app review needed |
| Microsoft Excel | Requires Microsoft 365 subscription |
| Dropbox | Replaced with Google Sheets (already connected) |
| Google Translate API | Paid API — replaced with free code node |
| Telegram | Blocked in Pakistan — replaced with Discord + Gmail |

---

## 📥 STEP 1 — IMPORT ALL 4 WORKFLOWS

Delete any old workflows first, then import these URLs one by one:

**In n8n → Workflows → + New → ··· → Import from URL**

```
Workflow 1:
https://raw.githubusercontent.com/wardha-netizen/bibliophile-concierge/main/01-acquisition-multilingual-librarian.json

Workflow 2:
https://raw.githubusercontent.com/wardha-netizen/bibliophile-concierge/main/02-inventory-loan-manager.json

Workflow 3:
https://raw.githubusercontent.com/wardha-netizen/bibliophile-concierge/main/03-engagement-semantic-search.json

Workflow 4:
https://raw.githubusercontent.com/wardha-netizen/bibliophile-concierge/main/04-whatsapp-team-hub.json
```

---

## 🗄️ STEP 2 — AIRTABLE SETUP

### Create ONE base called: `Bibliophile Concierge`
### Create ONE table called: `Books`

Add these exact columns (field names must match exactly):

| Field Name | Field Type | Notes |
|---|---|---|
| Title | Single line text | Primary field |
| Author | Single line text | |
| Language | Single line text | |
| Genre | Single select | Options: Novels, Poetry, Education, Religion, Biography, Science, Children, Other |
| Plant Spirit | Single line text | |
| Summary | Long text | |
| Price (USD) | Number | Decimal |
| Status | Single select | Options: Available, Issued, Completed |
| Cover URL | URL | |
| Owner | Single select | Options: Main Garden, Member 1, Member 2, Member 3 |
| Notes | Long text | |
| Lent To | Single line text | |
| Due Date | Date | |
| Posted | Checkbox | Default: unchecked |

**Get your Base ID:** Open Airtable → your base → copy the URL → it looks like:
`https://airtable.com/appXXXXXXXXXXXXXX/...`
The part starting with `app` is your AIRTABLE_BASE_ID.

---

## 📊 STEP 3 — GOOGLE SHEETS SETUP

### Create ONE spreadsheet called: `Bibliophile Library Backup`

Add these 4 tabs (sheets) with exact names:

**Tab 1: `Books`**
Columns: Title | Author | Genre | Language | Status | Owner

**Tab 2: `Loans`**
Columns: Title | Friend | Due | Status

**Tab 3: `Completed`**
Columns: Title | Author | Genre | Caption | Summary | Date

**Tab 4: `Requests`** (used by Workflow 4)
Columns: Requestor | Book Wanted | Status | Urgency | Date

**Get your Sheet ID:** Open the spreadsheet → copy the URL → it looks like:
`https://docs.google.com/spreadsheets/d/XXXXXXXXXXXXXXXXXXXXXXXX/edit`
The long ID in the middle is your GOOGLE_SHEETS_BACKUP_ID.

---

## ⚙️ STEP 4 — n8n VARIABLES

Go to: **n8n → Settings (bottom left) → Variables → + Add**

Add ALL of these:

| Variable Name | What to put | How to get it |
|---|---|---|
| `AIRTABLE_BASE_ID` | appXXXXXXXXXXXXXX | From Airtable URL |
| `GOOGLE_SHEETS_BACKUP_ID` | Long ID from Sheets URL | From Google Sheets URL |
| `DISCORD_WEBHOOK_URL` | Full webhook URL | Discord → Channel → ⚙️ → Integrations → Webhooks → New → Copy URL |
| `OWNER_EMAIL` | your@gmail.com | Your Gmail address |
| `GMAIL_FROM` | your@gmail.com | Same as OWNER_EMAIL |
| `OPENWEATHER_API_KEY` | key from openweathermap | Sign up FREE at openweathermap.org → API Keys |
| `OWNER_CITY` | Karachi,PK | Your city name |
| `GITHUB_OWNER` | wardha-netizen | Your GitHub username |
| `GITHUB_REPO` | bibliophile-concierge | Your repo name |
| `YOUTUBE_API_KEY` | key from Google Cloud | Google Cloud Console → Enable YouTube Data API v3 → Create Key |
| `WHATSAPP_PHONE_NUMBER_ID` | From Meta Developer | Meta → Your App → WhatsApp → API Setup → Phone Number ID |
| `WHATSAPP_ACCESS_TOKEN` | Permanent token | The permanent token you already generated |
| `MEMBER1_PHONE` | 923001234567 | Teammate 1 phone (country code + number, NO + sign) |
| `MEMBER2_PHONE` | 923009876543 | Teammate 2 phone (country code + number, NO + sign) |
| `MEMBER3_PHONE` | 923XXXXXXXXX | Your own phone (country code + number, NO + sign) |

---

## 🔑 STEP 5 — n8n CREDENTIALS

Go to: **n8n → Settings → Credentials → + Add**

You need exactly 4 credentials:

### 1. Google OAuth (covers EVERYTHING Google)
- Type: Google OAuth2
- Scopes needed: Gmail, Sheets, Drive, Calendar, Tasks
- Click "Sign in with Google" → use your Gmail account
- **This ONE credential covers:** Gmail ✅ Google Sheets ✅ Google Drive ✅ Google Calendar ✅ Google Tasks ✅

### 2. Airtable
- Type: Airtable Personal Access Token
- Go to: airtable.com/create/tokens → Create token → All scopes → All bases
- Paste the token in n8n

### 3. Groq AI (as OpenAI credential)
- Type: OpenAI API
- Base URL: `https://api.groq.com/openai/v1`
- API Key: Get FREE from console.groq.com → API Keys → Create new key
- ⚠️ If your old Groq key is erring, generate a NEW one — they expire

### 4. GitHub
- Type: GitHub Personal Access Token
- Go to: github.com → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new
- Scopes: check `repo` (full control)
- ⚠️ If your old GitHub token is erring, generate a NEW one

---

## 🔄 STEP 6 — LINK CREDENTIALS IN EACH WORKFLOW

After importing, open each workflow. Any node with a red warning needs you to click it and select your saved credential from the dropdown.

| Workflow | Nodes needing credentials |
|---|---|
| W1 | AI node → Groq, Airtable node → Airtable, Sheets node → Google OAuth |
| W2 | Airtable nodes → Airtable, Calendar → Google OAuth, Tasks → Google OAuth, Gmail → Google OAuth, Sheets → Google OAuth |
| W3 | AI nodes → Groq, Airtable nodes → Airtable, Drive → Google OAuth, Gmail → Google OAuth, Sheets → Google OAuth, GitHub → GitHub |
| W4 | Airtable nodes → Airtable, Sheets → Google OAuth |

---

## 🔑 OLD KEYS THAT MIGHT BE EXPIRED — REGENERATE THESE

| Key | Status | Action |
|---|---|---|
| Groq API Key | ⚠️ Likely expired | Go to console.groq.com → API Keys → Delete old → Create new |
| GitHub Token | ⚠️ May be expired | Go to GitHub Settings → Developer Settings → Delete old → Generate new (classic, repo scope) |
| OpenWeatherMap Key | ✅ Usually permanent | Keep same key, just add it as variable |
| Discord Webhook URL | ✅ Permanent | Keep same URL |
| WhatsApp Token | ✅ You made permanent | Keep the permanent token you generated |

---

## 📱 STEP 7 — WHATSAPP TEST MODE REQUIREMENT

Since your WhatsApp is still in testing mode:

**Each teammate MUST send a message first:**
1. Save this number in their phone: `+1 (555) 158-5349`
2. Send any message (even just "hi") to that number on WhatsApp
3. This opens the 24-hour window to receive messages from your business

After Meta approves your app, this step won't be needed anymore.

---

## 🧪 STEP 8 — TESTING EACH WORKFLOW

### Test Workflow 1:
1. Click "Execute Workflow"
2. Open the form URL that appears
3. Fill in: Book Title = "Atomic Habits", Author = "James Clear"
4. Submit → check Airtable for new row, Discord for notification

### Test Workflow 2A (Loan):
1. Click trigger node → "Execute Node"
2. Open form URL → Issue a book → check Airtable updated

### Test Workflow 2B (Weather):
1. Click "⏰ Daily 9AM Trigger" → "Execute Node"
2. Check Discord for book suggestion

### Test Workflow 3A (Quote):
1. Click "🌅 Daily 8AM Trigger" → "Execute Node"
2. Check Gmail + Discord for morning quote

### Test Workflow 3B (Search):
1. Click trigger → open form URL
2. Search for "Atomic Habits" → enter your email → submit
3. Check email for results

### Test Workflow 4 (WhatsApp):
1. Click "📋 Book Request Form" trigger → open URL
2. Fill form → submit
3. Check all 3 WhatsApp numbers for message

---

## 📋 COMPLETE SUMMARY — WHAT EACH WORKFLOW DOES

### Workflow 1 — Book Intake (Member 1)
- Form → Google Books + Open Library → Free cover image → AI detects genre → Airtable → Sheets → Discord

### Workflow 2 — Loans + Weather (Member 2)
- A: Loan form → Airtable update → Google Calendar + Tasks → Discord → Sheets
- B: 9AM daily → weather → picks matching genre book → Discord
- C: 10AM daily → finds overdue → Gmail + Discord alert

### Workflow 3 — Engagement + Search (Member 3)
- A: 8AM daily → AI quote → Gmail + Discord
- B: Search form → Airtable + Drive search → AI → email results
- C: 6PM daily → completed books → AI content → Discord + GitHub + Sheets

### Workflow 4 — WhatsApp Team (Member 4)
- A: Request form → check Airtable → WhatsApp all 3 teammates
- B: 9AM daily → library stats digest → WhatsApp all 3 teammates
- C: Return form → update Airtable → WhatsApp all 3 teammates
