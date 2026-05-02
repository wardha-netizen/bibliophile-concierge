# 🌱 Beginner Walkthrough — From Zero to Bibliophile Concierge

Hey! Don't worry — you're closer than you think. Your screenshot shows you already built:

> **Google Sheets Trigger → Get many volumes (Google Books) → Basic LLM Chain (Groq) → Send Discord message → Update sheet**

That is **exactly** the n8n pattern you'll repeat ~20 more times. You've already learned the hardest part. 🎉

This guide shows you, in order, what to do next.

---

## 🧠 The 4 Concepts You Need (that's it)

1. **Trigger** — what kicks off a workflow (a form submission, a schedule like "every day at 9am", a new row in a sheet, a Telegram message…)
2. **Node** — one box on the canvas that does ONE thing (look up a book, send a message, write a row…)
3. **Connection** — the line between two nodes. Data flows from left to right.
4. **Expression** — when you want a node to use data from a previous node. They look like `{{ $json.fieldName }}` and you wrap them in `=` at the start of the field.

That's literally it. Everything else is just "which node, which field."

---

## 📍 Where you are vs. where you're going

### What you have right now (great starting point!)
- ✅ n8n running locally
- ✅ Google Sheets connected
- ✅ Google Books working
- ✅ Groq LLM working (Basic LLM Chain)
- ✅ Discord working
- ✅ A "Bibliophile Vault" sheet with Timestamp / Book Title / User / AI Summary

### What we're going to add
- 3 separate workflows (one per group member)
- 24 apps total
- A pretty Tangled-style intake form

You **don't need to throw away** what you have. Your current workflow ≈ a tiny version of Member 1. Either:
- **Option A (recommended):** Import the 3 ready-made workflows I built (`01-`, `02-`, `03-` JSON files) and wire up credentials.
- **Option B:** Keep your current workflow as practice and slowly add nodes to match Member 1.

I'll explain Option A because it gets you to a finished demo fastest.

---

## 🚀 Step-by-Step Setup (do these in order)

### Step 1 — Open architecture.html
Double-click `architecture.html` in this folder. It opens in your browser. Keep it open in another tab — it shows what every box does and how the 24 apps connect. Show this to your teacher too. 🎯

### Step 2 — Import the 3 workflows
In n8n (the page in your screenshot):
1. Click the **☰** menu (top-left) → **Workflows**
2. Click **Import from File** (top-right area)
3. Select `01-acquisition-multilingual-librarian.json`
4. Repeat for `02-` and `03-`

You'll now see 3 new workflows. They'll have **red dots** on most nodes — that means "credentials missing." That's normal.

### Step 3 — Create the free accounts (in this order — easiest first)

Don't try to do all 24 at once. Tackle them in this order — each one takes 2–5 minutes:

#### 🟢 Easy (start here, same Google login covers most)
| App | Where | What you get |
|---|---|---|
| **Groq** | https://console.groq.com → "API Keys" → Create | A free, fast LLM key |
| **Google Cloud** | https://console.cloud.google.com → New Project → "APIs & Services" | Enable: Google Books API, Google Translate API, YouTube Data API v3, Drive, Sheets, Calendar, Gmail |
| **Airtable** | https://airtable.com sign up → create base "Bibliophile" | Personal Access Token from airtable.com/create/tokens |
| **Telegram** | Open Telegram app → search **@BotFather** → `/newbot` | A bot token |
| **Discord** | You already have this ✅ | (Use webhook URL from channel settings) |
| **OpenWeatherMap** | https://openweathermap.org/api → free plan | API key |

#### 🟡 Medium (require an extra step or business profile)
| App | Notes |
|---|---|
| **WhatsApp Business Cloud API** | https://developers.facebook.com → create app → WhatsApp product → free 1k convos/mo. If too much hassle, **swap for Telegram** in the WhatsApp nodes. |
| **Cloudinary** | https://cloudinary.com signup → create "unsigned upload preset" in Settings → Upload |
| **Unsplash** | https://unsplash.com/developers → Create app → use Access Key |
| **Todoist** | Settings → Integrations → API token |
| **Spotify** | https://developer.spotify.com → create app |
| **GitHub** | Settings → Developer settings → fine-grained PAT |

#### 🟠 Optional (skip if running short on time — workflow still works)
- Instagram (needs Facebook Business + Instagram Business account — fiddly)
- LinkedIn (needs OAuth app approval)
- Pinterest (needs developer app)
- Microsoft Excel (needs Microsoft account)
- Dropbox (easy actually — OAuth via n8n)

> **Tip:** If a node is too painful to set up, just **deactivate that single node** (right-click → Deactivate). The rest of the workflow keeps working. Tell your teacher upfront which apps are "demo-mocked."

### Step 4 — Add your keys to n8n

In n8n:
1. **Settings (bottom-left ⚙️) → Variables** — paste in the simple keys (Groq, Unsplash, OpenWeather, Cloudinary, etc.) using the names from `README.md`.
2. **Credentials → New** — for OAuth ones (Google, Airtable, Telegram, Discord, etc.), click the credential type, click **Connect**, and follow the popup. n8n handles the messy OAuth dance for you.

### Step 5 — Create the Airtable base
Open your Airtable, create a base called **Bibliophile** with a table **Books** and these fields (the README has the full list):

```
Title, Author, Language, Genre, Plant Spirit, Summary, Notes,
Price (USD), Status, Lent To, Due Date, Cover URL, Owner, Posted
```

Copy the base ID (the part of the URL starting with `app...`) into the n8n variable `AIRTABLE_BASE_ID`.

### Step 6 — Update your Google Sheet
Your "Bibliophile Vault" sheet has `Timestamp / Book Title / User / AI Summary`. Add **two more tabs** to the same file:
- **Books** — same columns as Airtable
- **Loans** — Title / Friend / Due / Status
- **Completed** — auto-populated

Copy the file ID (the long string between `/d/` and `/edit` in the URL) into `GOOGLE_SHEETS_BACKUP_ID`.

### Step 7 — Activate and test 🎉

For each workflow:
1. Open it
2. Click **Execute Workflow** (the orange button at the bottom of your screenshot) to test manually
3. Fix any red errors (usually missing credentials)
4. When green, flip the **Active** toggle (top-right)

For form-trigger workflows, n8n shows you a **public URL** at the top of the form node. That's your beautiful intake form — share that link!

---

## 🎓 What to say in your demo / documentation

Use the architecture diagram (architecture.html) and these talking points:

1. **"We used webhooks, AI, databases, and multi-channel notifications — every workflow has all four."** ← Teacher's exact requirement ✅
2. **Semantic Search** = Member 3's Telegram bot searches Airtable Notes + Google Drive PDFs in parallel, then a Groq LLM ranks the results.
3. **Data Integrity** = Every Airtable write is mirrored to Google Sheets (primary vs. backup) and Microsoft Excel.
4. **Dynamic UI / Tangled theme** = the n8n form trigger uses custom CSS (see the `customCss` field) — cream + sage palette, Georgia serif, soft pill buttons.
5. **Weather-Genre Sync** = OpenWeatherMap → mapping function → suggests a book from that genre's shelf.

---

## 🆘 Common beginner gotchas

- **"My expression doesn't work!"** — Did you start the field with `=`? n8n expressions ONLY work in fields that begin with `=`.
- **"Red error: cannot read property X"** — A previous node didn't return the data you expected. Click the previous node, run it alone, look at the output JSON on the right side.
- **"Credentials don't connect"** — For Google services on localhost, you need to add `http://localhost:5678/rest/oauth2-credential/callback` to the OAuth redirect URIs in Google Cloud Console.
- **"Rate limited"** — All free tiers have limits (Unsplash 50/hr, Groq is generous). Don't loop test 100 times.
- **Sharing with teammates** — n8n self-hosted is single-user by default. Either run it on one shared machine, or each member runs their own copy and you split the workflows.

---

## 📦 What's in this folder

| File | What to do with it |
|---|---|
| `architecture.html` | **Open in browser** — your visual diagram for the demo |
| `README.md` | Full credentials list and free signup links |
| `BEGINNER_GUIDE.md` | This file — read first |
| `01-acquisition-multilingual-librarian.json` | Import into n8n |
| `02-inventory-loan-manager.json` | Import into n8n |
| `03-engagement-semantic-search.json` | Import into n8n |

---

You've got this. 🌷 The fact that you got Sheets + Books + Groq + Discord all talking to each other already means you understand n8n better than you think.
