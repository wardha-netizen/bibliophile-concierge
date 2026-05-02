# 📱 WhatsApp Cloud API — Step by Step

WhatsApp's free Cloud API is run by Meta (Facebook). Free tier = 1,000 messages/month. Enough for your project.

---

## 🅰️ Option A — WhatsApp Cloud API (free, takes ~15 min)

### Step 1 — Create a Meta Developer Account
- [ ] Go to https://developers.facebook.com
- [ ] Click **Get Started** (top-right)
- [ ] Log in with your Facebook account (or create one — it's free)
- [ ] Accept the developer terms

---

### Step 2 — Create an App
- [ ] Once logged in, click **My Apps** (top-right)
- [ ] Click **Create App**
- [ ] You'll see a screen asking "What do you want your app to do?"
- [ ] Choose **Other** → click Next
- [ ] Choose **Business** → click Next
- [ ] Fill in:
  - App name: `Bibliophile Concierge`
  - App contact email: your email
  - Business account: skip (click "I don't want to connect a business portfolio yet")
- [ ] Click **Create App**

---

### Step 3 — Add WhatsApp to your App
- [ ] You'll land on the "Add products to your app" dashboard
- [ ] Scroll down and find **WhatsApp**
- [ ] Click **Set up** under WhatsApp
- [ ] Click **Start using the API**

---

### Step 4 — Get your keys (this is what n8n needs)

You're now in the **WhatsApp API Setup** page. You'll see two important things:

**🔑 Key 1: Temporary Access Token**
- At the top of the page, there's a box that says **"Temporary access token"**
- Click **Copy** — this is your `WHATSAPP_ACCESS_TOKEN`
- ⚠️ This token expires in 24 hours! (That's OK for testing. See Step 7 for a permanent one.)

**🔑 Key 2: Phone Number ID**
- A little below, you'll see **Phone number ID**
- Copy that number — this is your `WHATSAPP_PHONE_NUMBER_ID`

---

### Step 5 — Add a test recipient (your own phone)
WhatsApp sandbox only sends to approved numbers. Add your number first:
- [ ] On the same setup page, find the section **"To"** (or "Send and receive messages")
- [ ] Click **Manage phone number list** 
- [ ] Click **+ Add phone number**
- [ ] Type your phone number with country code (e.g., `+923001234567` for Pakistan)
- [ ] WhatsApp will send a code to your phone — enter it to verify ✅

---

### Step 6 — Test it (optional but satisfying)
On the setup page, there's a "Send message" section:
- [ ] Make sure "To" is set to your verified phone number
- [ ] Click **Send message**
- [ ] Check your WhatsApp — you should receive "Hello World!" 🎉

---

### Step 7 — Put the keys in n8n
- [ ] In n8n, go to **⚙️ Settings → Variables**
- [ ] Click **+ Add Variable** and add these one by one:

| Variable name | Value |
|---|---|
| `WHATSAPP_PHONE_NUMBER_ID` | the number you copied in Step 4 |
| `OWNER_WHATSAPP_NUMBER` | your number, no +, e.g. `923001234567` |

- [ ] Now in n8n, open the **💬 WhatsApp Planting Confirmation** node (in Workflow 1)
- [ ] Credential field → **Create new credential** → WhatsApp Business Cloud
- [ ] It will ask for **Access Token** — paste the temporary token from Step 4
- [ ] Click **Save**

---

### Step 8 — Get a permanent token (so it doesn't expire)
The 24-hour token is fine for testing. For your demo/presentation, make a permanent one:

- [ ] In Meta for Developers, go to **Tools → Graph API Explorer** (from the top menu)
- [ ] OR go directly to https://business.facebook.com/settings
- [ ] Go to **System Users** → **Add**
- [ ] Create a system user, give it admin role
- [ ] Click **Generate New Token**
- [ ] Check the `whatsapp_business_messaging` permission
- [ ] Copy this token — it doesn't expire ✅
- [ ] Update the n8n credential with this new token

---

## 🅱️ Option B — Swap to Telegram (10x easier, 2 minutes)

If WhatsApp is giving you headaches, **Telegram does the exact same thing** — sends you a notification when a book is planted. Your teacher won't notice the difference.

Here's how:

### Step 1 — Create a Telegram Bot
- [ ] Open Telegram app on your phone
- [ ] Search for **@BotFather** and open the chat
- [ ] Type `/newbot` and send it
- [ ] It asks for a name → type `Bibliophile Concierge`
- [ ] It asks for a username → type something like `bibliophile_myname_bot`
- [ ] BotFather replies with a **token** that looks like `1234567890:ABCdefGHIjklMN...` — copy it ✅

### Step 2 — Get your Chat ID
- [ ] Search for **@userinfobot** in Telegram, open it
- [ ] Press Start
- [ ] It immediately replies with your **Id number** (e.g., `987654321`) — copy it ✅

### Step 3 — Add credentials in n8n
- [ ] In n8n, open the **💬 WhatsApp Planting Confirmation** node in Workflow 1
- [ ] Delete this node (right-click → Delete)
- [ ] Click the **+** after the "Plant in Airtable Vault" node
- [ ] Search for **Telegram** → choose **Send a Text Message**
- [ ] In the node settings:
  - **Credential**: click Create new → paste your Bot token → Save
  - **Chat ID**: `={{ $vars.OWNER_TELEGRAM_CHAT_ID }}`
  - **Text**: paste this:
    ```
    🌿 Planting Confirmation 🌿

    📖 Book planted!
    🌸 Plant Spirit: {{ $('🤖 AI: Genre + Plant Spirit').item.json.message.content.plant_spirit }}
    📚 Genre: {{ $('🤖 AI: Genre + Plant Spirit').item.json.message.content.genre }}
    💰 Estimated value: ${{ $('🤖 AI: Genre + Plant Spirit').item.json.message.content.estimated_price_usd }}
    ```
- [ ] In n8n Variables, add `OWNER_TELEGRAM_CHAT_ID` = your chat ID from Step 2

That's it. ✅ Same notification, easier setup, and Telegram is free forever.

---

## Which one should I pick?

| | WhatsApp | Telegram |
|---|---|---|
| Time to set up | ~15 min | ~2 min |
| Difficulty | Medium | Very easy |
| Looks impressive in demo? | ✅ Yes (everyone has WhatsApp) | ✅ Yes too |
| Free? | Yes (1000 msg/mo) | Yes (unlimited) |
| Recommended for first time? | Only if you have time | ✅ Yes |

**My honest advice: use Telegram for now so you can move on. You can always add WhatsApp later.**
