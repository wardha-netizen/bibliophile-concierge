# 📱 WhatsApp — Permanent Token + Allow All Users

Two separate goals — do them in order.

---

# PART 1 — Get a Permanent Token (never expires)

The temporary token Meta gave you expires every 24 hours. Here's how to make one that lasts forever.

## Step 1 — Create a Meta Business Account (if you don't have one)
- [ ] Go to https://business.facebook.com
- [ ] Click **Create Account**
- [ ] Enter your name, business name (can be "Bibliophile Concierge"), email
- [ ] Follow the steps — it's free

## Step 2 — Connect your App to your Business Account
- [ ] Go back to https://developers.facebook.com → **My Apps** → click your **Bibliophile Concierge** app
- [ ] Top-left, click **App Settings → Basic**
- [ ] Scroll down to **Business Account**
- [ ] Click **Add** → select your Business Account → Save

## Step 3 — Create a System User
A System User is like a "robot account" that holds the permanent token.

- [ ] Go to https://business.facebook.com/settings
- [ ] Left sidebar → **Users → System Users**
- [ ] Click **Add** (top-right)
- [ ] Name: `n8n-bot`
- [ ] Role: **Admin**
- [ ] Click **Create System User**

## Step 4 — Give the System User access to your WhatsApp App
- [ ] Still on the System Users page, click on `n8n-bot`
- [ ] Click **Add Assets**
- [ ] Choose **Apps** from the left list
- [ ] Find your **Bibliophile Concierge** app → toggle it ON → give it **Full Control**
- [ ] Click **Save Changes**

## Step 5 — Generate the permanent token
- [ ] Still on the `n8n-bot` page, click **Generate New Token**
- [ ] Select your **Bibliophile Concierge** app from the dropdown
- [ ] Token expiration: choose **Never**
- [ ] Under permissions, scroll and check these two boxes:
  - ✅ `whatsapp_business_messaging`
  - ✅ `whatsapp_business_management`
- [ ] Click **Generate Token**
- [ ] **COPY THE TOKEN NOW** — Meta only shows it once. Paste it in a Notepad file immediately.

## Step 6 — Update the token in n8n
- [ ] In n8n, go to **Credentials** (left sidebar, key icon)
- [ ] Find your **WhatsApp Business Cloud** credential → click the pencil (edit) icon
- [ ] Replace the old token with your new permanent token
- [ ] Click **Save** ✅

**That's it — your token will never expire again.**

---

# PART 2 — Allow Other People to Receive WhatsApp Messages

Right now your WhatsApp is in **"sandbox mode"** — it can ONLY send to phone numbers you manually approved. This is fine for testing but blocks your group members from receiving messages.

You have two options:

---

## 🅰️ Option A — Just add your friends' numbers (easiest, for demo)

Still in sandbox mode, but add your group members' numbers as approved recipients. Free, takes 2 minutes per person.

- [ ] Go to https://developers.facebook.com → your app → **WhatsApp → API Setup**
- [ ] Scroll to **"Send and receive messages"**
- [ ] Under **"To"**, click **Manage phone number list**
- [ ] Click **+ Add phone number**
- [ ] Type your friend's number with country code (e.g. `+923001234567`)
- [ ] WhatsApp sends them a verification code — they enter it
- [ ] They're approved ✅

Repeat for each group member. You can add up to **5 numbers** on the free sandbox. That's enough for your project team.

---

## 🅱️ Option B — Go Live (remove sandbox, send to ANYONE)

This removes the 5-number limit so any WhatsApp user can receive messages from your system. Free, but requires Meta to review your app (~1-2 days).

### Step 1 — Add a real phone number
- [ ] Go to your app → **WhatsApp → Phone Numbers**
- [ ] Click **Add phone number**
- [ ] Enter a real phone number that isn't already a WhatsApp Business account
  - (This can be a SIM card, Skype number, Google Voice number, etc.)
- [ ] Verify via SMS or call

### Step 2 — Set up your WhatsApp Business Profile
- [ ] Go to **WhatsApp → Business Profile**
- [ ] Fill in: Display name, Category (Education), Description
- [ ] This is what people see when they get a message from you

### Step 3 — Submit for Live Access
- [ ] Go to **App Review → Permissions and Features**
- [ ] Find `whatsapp_business_messaging` → click **Request Advanced Access**
- [ ] Fill in the short form explaining how you use it (write: "Library management system for a school project — sends book intake confirmations and loan reminders to library members")
- [ ] Submit
- [ ] Meta approves in 1-3 days ✅

### Step 4 — Update the n8n credential
Once approved, your permanent token (from Part 1) + your real phone number ID works for everyone. Update the `WHATSAPP_PHONE_NUMBER_ID` variable in n8n with the new phone number's ID.

---

## Which option is right for you?

| | Option A (Add friends' numbers) | Option B (Go Live) |
|---|---|---|
| Time | 2 min per person | 1-3 days wait |
| Limit | 5 numbers max | Unlimited |
| Approval needed? | No | Yes (Meta review) |
| Cost | Free | Free |
| Good for school demo? | ✅ Yes, perfect | Only if you have days to wait |

**Recommendation: Use Option A for your demo — add all 3 group members' numbers. It's instant and free. You can go live later if needed.**

---

## Summary — What to do right now

1. ✅ Get permanent token (Part 1, takes ~10 min)
2. ✅ Add your 3 group members' numbers as approved recipients (Option A, 2 min each)
3. ✅ Update n8n credential with the permanent token
4. ✅ Test by submitting a book through your intake form
