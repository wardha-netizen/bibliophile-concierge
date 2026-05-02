# 🌱 START HERE — Absolute Beginner Guide

Hi! Take a deep breath. We are going to do this **one tiny step at a time**. Don't skip ahead. After each ✅ checkbox, you should see something working before moving on.

By the end of **Part 1** you'll have n8n running on your computer.
By the end of **Part 2** you'll have your FIRST tiny workflow saving a book to a Google Sheet.
By the end of **Part 3** you'll have all 3 big workflows imported.

---

# 🟢 PART 1 — Install n8n (10 minutes)

## Step 1.1 — Install Node.js
n8n needs Node.js to run.

- [ ] Go to https://nodejs.org
- [ ] Click the big green **LTS** button (left button)
- [ ] Download and install it (just keep clicking Next)
- [ ] **Check it worked:** Open Terminal (Mac) or Command Prompt (Windows) and type:
  ```
  node --version
  ```
  You should see something like `v20.11.0`. If yes ✅ keep going. If not, restart your computer and try again.

## Step 1.2 — Start n8n
- [ ] In the same Terminal/Command Prompt, type:
  ```
  npx n8n
  ```
- [ ] Wait. The first time it takes 2-3 minutes (it's downloading n8n). You'll see lots of text scrolling.
- [ ] When you see something like **"Editor is now accessible via: http://localhost:5678"** → it's ready! ✅
- [ ] **Don't close this Terminal window.** As long as it's open, n8n is running. If you close it, n8n stops.

## Step 1.3 — Open n8n in your browser
- [ ] Open Chrome / Edge / Firefox
- [ ] Go to: **http://localhost:5678**
- [ ] You'll see a signup page — fill in email + password (this is just for you, it stays on your computer)
- [ ] You'll land on the n8n dashboard 🎉

> 💡 **From now on, "open n8n" means: keep that terminal open, then go to http://localhost:5678**

---

# 🟡 PART 2 — Build Your First Tiny Workflow (15 min)

We'll make a workflow that takes a book title from a form and saves it to Google Sheets. That's it. No AI yet. Just to learn how n8n works.

## Step 2.1 — Make the Google Sheet first
- [ ] Go to https://sheets.google.com
- [ ] Click the **+ Blank** spreadsheet
- [ ] Rename it to **Bibliophile Vault** (top-left)
- [ ] In row 1, type these column headers:
  ```
  A1: Timestamp    B1: Title    C1: Author    D1: Status
  ```
- [ ] **Copy the URL of the sheet** — you'll need part of it. The URL looks like:
  ```
  https://docs.google.com/spreadsheets/d/1AbCDeFgH-XXXXXXXXX/edit
  ```
  The part between `/d/` and `/edit` is the **Sheet ID** — copy it somewhere safe (a notepad file).

## Step 2.2 — Create a new workflow in n8n
- [ ] In n8n (http://localhost:5678), click **Workflows** in the left sidebar
- [ ] Click the **+ Create Workflow** button (top-right)
- [ ] You'll see an empty canvas with a big **+** button in the middle
- [ ] Click **+** → search for **"On form submission"** → click it
- [ ] On the right panel that opens:
  - Form Title: **Add a Book**
  - Click **+ Add Field** three times:
    - Field 1: Label = `Title`, Type = Text, Required = ON
    - Field 2: Label = `Author`, Type = Text
    - Field 3: Label = `Status`, Type = Dropdown, Options = Available / Issued / Completed
- [ ] Close the panel (X in top-right of panel)
- [ ] You'll see a node on the canvas called "On form submission" ✅

## Step 2.3 — Add the Google Sheets node
- [ ] Click the small **+** that appeared to the right of your form node
- [ ] Search for **"Google Sheets"** → click it
- [ ] Choose **"Append row in sheet"**
- [ ] Now we need to connect your Google account ↓

## Step 2.4 — Connect Google Sheets (do this slowly — it's the trickiest part)
- [ ] In the Google Sheets node, click the **Credential to connect with** dropdown → **Create new credential**
- [ ] A popup opens. It needs a Google **OAuth Client ID** and **Client Secret**. Here's how to get them for free:

  **Get Google OAuth credentials (one-time setup, used for ALL Google services):**
  1. Open https://console.cloud.google.com (login with Google)
  2. Top-left, click the project dropdown → **New Project** → name it "n8n" → Create
  3. Make sure the new project is selected (top-left dropdown)
  4. Left menu (☰) → **APIs & Services → Library**
  5. Search "Google Sheets API" → click it → **Enable**
  6. (Repeat the search+enable for: **Google Drive API**, **Gmail API**, **Google Calendar API**, **Google Books API**, **YouTube Data API v3** — do this once now and you'll never have to come back)
  7. Left menu → **APIs & Services → OAuth consent screen**
     - User Type: **External** → Create
     - App name: n8n
     - User support email: your email
     - Developer email: your email
     - Save and Continue, Save and Continue, Save and Continue
     - Click **+ Add Users** → add your own Gmail → Save
  8. Left menu → **APIs & Services → Credentials**
     - Click **+ Create Credentials → OAuth client ID**
     - Application type: **Web application**
     - Name: n8n
     - **Authorized redirect URIs** → click **+ Add URI** → paste:
       ```
       http://localhost:5678/rest/oauth2-credential/callback
       ```
     - Click **Create**
     - A popup shows your **Client ID** and **Client Secret** — copy both ✅

- [ ] Back in n8n's Google credential popup:
  - Paste the **Client ID** and **Client Secret**
  - Click **Sign in with Google**
  - A new tab opens → choose your Google account → click **Continue / Allow**
  - You'll see "Account connected" ✅
  - Click **Save** in n8n

## Step 2.5 — Tell the node which sheet to write to
- [ ] Back in the Google Sheets node (still open):
  - **Document**: paste your Sheet ID from Step 2.1
  - **Sheet**: choose **Sheet1** from the dropdown
  - **Mapping Mode**: choose **Map each column manually**
  - You'll see your column names appear (Timestamp, Title, Author, Status). Fill them like this (the curly braces pull data from your form):
    - Timestamp → click the field, type `=` first, then `{{ $now }}`
    - Title → click, type `=`, then `{{ $json.Title }}`
    - Author → click, type `=`, then `{{ $json.Author }}`
    - Status → click, type `=`, then `{{ $json.Status }}`
- [ ] Close the panel ✅

## Step 2.6 — Test it!
- [ ] Click **Save** (top-right) — name the workflow "My First Workflow"
- [ ] Click the **Active** toggle (top-right) → flip it ON
- [ ] Click on your **form node** → you'll see two URLs at the top: **Test URL** and **Production URL**
- [ ] Copy the **Production URL**, paste it in a new browser tab
- [ ] You'll see your form! Fill it in: Title = "Harry Potter", Author = "JK Rowling", Status = Available
- [ ] Click Submit
- [ ] Open your Google Sheet → you should see a new row! 🎉🎉🎉

**If you got here, you understand n8n.** Everything else is just more nodes.

---

# 🔵 PART 3 — Import the 3 Big Workflows

Now that the basics work, let's bring in the full system.

## Step 3.1 — Import the JSON files
- [ ] In n8n, go to **Workflows** (left sidebar)
- [ ] Click the **⋯ menu (three dots)** at the top right of the workflows list → **Import from File**
- [ ] Choose `01-acquisition-multilingual-librarian.json`
- [ ] Wait for it to load — you'll see a workflow with red error dots. That's expected — it just needs credentials.
- [ ] Repeat for `02-inventory-loan-manager.json` and `03-engagement-semantic-search.json`

## Step 3.2 — Get the keys, ONE service at a time

**The golden rule:** Don't try to do all the keys at once. Pick the workflow you'll demo first, get only the keys it needs, test it, then move to the next.

I recommend this order — it gets you a working demo fastest:

### 🎯 Goal A: Get Member 1 (Book Intake) working — needs 6 keys
| # | Service | Where to sign up | Time | Skill needed |
|---|---|---|---|---|
| 1 | **Groq (free AI)** | https://console.groq.com → API Keys → Create | 2 min | Easy |
| 2 | **Google** (Sheets, Books, Translate, Drive, Gmail, Calendar) | Already done in Step 2.4 ✅ | 0 min | Done! |
| 3 | **Airtable** | https://airtable.com sign up | 5 min | Easy |
| 4 | **Unsplash** (cartoon covers) | https://unsplash.com/developers → New Application | 3 min | Easy |
| 5 | **Cloudinary** (image hosting) | https://cloudinary.com signup | 5 min | Easy |
| 6 | **WhatsApp** OR swap to Telegram | Telegram is way easier — see below | 5 min | Easy |

### 🎯 Goal B: Add Member 2 (Loans + Reminders) — needs 3 more keys
| # | Service | Where |
|---|---|---|
| 7 | **Telegram bot** | Open Telegram → message **@BotFather** → `/newbot` |
| 8 | **OpenWeatherMap** | https://openweathermap.org/api → free plan |
| 9 | **Todoist** | Settings → Integrations → API token |
| 10 | **Discord webhook** | Discord channel → ⚙️ Edit Channel → Integrations → Webhooks → New Webhook → Copy URL |

### 🎯 Goal C: Add Member 3 (Search + Social) — optional fancy ones
GitHub (easy), Spotify (easy), Dropbox (easy), Pinterest (medium), LinkedIn (medium), Instagram (hard — skip if needed), Microsoft Excel (medium).

## Step 3.3 — How to add a key in n8n

There are TWO places to put keys:

### A) **Variables** (for simple text keys like API tokens)
- [ ] Bottom-left ⚙️ → **Variables** → **+ Add Variable**
- [ ] Name: `GROQ_API_KEY` (use exact names from `README.md`)
- [ ] Value: paste your key
- [ ] Save

### B) **Credentials** (for OAuth services like Google, Telegram, Spotify)
- [ ] In any workflow, open a node that uses that service
- [ ] Click the credential dropdown → **Create new credential**
- [ ] Fill in what it asks → Save
- [ ] After it's saved once, every node using that service can re-use it

## Step 3.4 — Set up Airtable (the main "vault")
- [ ] Go to https://airtable.com → **+ Create a base** → Start from scratch
- [ ] Name it **Bibliophile**
- [ ] Rename Table 1 to **Books**
- [ ] Add these fields (click + at the right of the columns):

| Field name | Type |
|---|---|
| Title | Single line text (this is the default first one) |
| Author | Single line text |
| Language | Single line text |
| Genre | Single select → add: Novels, Poetry, Education, Religion, Biography, Science, Children, Other |
| Plant Spirit | Single line text |
| Summary | Long text |
| Notes | Long text |
| Price (USD) | Number |
| Status | Single select → add: Available, Issued, Completed |
| Lent To | Single line text |
| Due Date | Date |
| Cover URL | URL |
| Owner | Single select → add: Main Garden, Member 1, Member 2, Member 3 |
| Posted | Checkbox |

- [ ] Get your **base ID**: copy it from the URL — `https://airtable.com/appXXXXXXXX/...` → the `appXXXXXXXX` part
- [ ] Get your **token**: go to https://airtable.com/create/tokens → **Create new token** → name it "n8n", give it `data.records:read`, `data.records:write`, `schema.bases:read` scopes, and add your Bibliophile base under "Access" → Create → copy the token
- [ ] In n8n: Variables → add `AIRTABLE_BASE_ID` = your base ID
- [ ] In n8n: any Airtable node → Credential → New → paste the token

## Step 3.5 — Activate one workflow at a time
- [ ] Open Workflow `01-acquisition-...`
- [ ] Click **Execute Workflow** at the bottom (this runs it once for testing)
- [ ] Fix any red errors (usually a missing credential — the node will tell you what's missing)
- [ ] When it runs green ✅ → flip the **Active** toggle ON
- [ ] Click on the form node → copy the **Production URL** → that's your beautiful book-intake form!
- [ ] Move on to workflow `02-` and `03-`

---

# 🆘 If You Get Stuck

| Problem | Fix |
|---|---|
| n8n won't start | Make sure Node.js is installed (`node --version` should show v18+) |
| http://localhost:5678 shows nothing | The terminal must stay open. Re-run `npx n8n` |
| Google won't connect | The redirect URI in Google Cloud Console MUST be exactly `http://localhost:5678/rest/oauth2-credential/callback` |
| "Cannot read property X" | The previous node didn't return data the way the next one expects. Click the previous node, click Execute, look at the JSON output on the right |
| A node has a red ! | Hover over the ! — it tells you what's wrong (usually missing credential or empty required field) |
| WhatsApp setup is too hard | Replace WhatsApp nodes with Telegram nodes — same idea, much easier |
| I broke a workflow | Click ⋯ menu → Workflow History → restore an earlier version |

---

# 🎯 Your Action List Right Now

Just do these 5 things tonight, in order. Don't worry about the rest until tomorrow.

1. [ ] Install Node.js (Step 1.1)
2. [ ] Run `npx n8n` and open http://localhost:5678 (Steps 1.2–1.3)
3. [ ] Build the tiny "Add a Book" form workflow (all of Part 2)
4. [ ] Once that works → import the 3 JSON files (Step 3.1)
5. [ ] Get the Groq + Airtable keys and try to make Workflow 1 run

That's enough for one day. Tomorrow you can add Telegram, weather, etc. one by one.

You got this. 🌷
