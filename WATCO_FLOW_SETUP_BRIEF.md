# EDMS — WATCO Flow Setup Brief
**Platform:** tnedms.com | **WhatsApp Number:** connected via Meta Cloud API
**Prepared by:** Ram Rishikesh, SIGNAL / NexoVent Labs

---

> This document contains every flow, message, creative specification, and content
> that WATCO must configure. Nothing is missing — build exactly as described below.

---

## WEBHOOK CONFIGURATION

| Setting | Value |
|---------|-------|
| Webhook URL (POST) | `https://tnedms.com/api/whatsapp/webhook` |
| Verification URL (GET) | `https://tnedms.com/api/whatsapp/webhook` |
| Flow Endpoint (POST) | `https://tnedms.com/api/whatsapp-flow-endpoint` |
| Verify Token | `election2026_verification_token` |

All message routing logic lives in our backend. WATCO forwards incoming messages to the webhook — our server decides what to send back.

---

## FLOW 1 — CHOOSE SERVICE MENU

**Trigger:** User sends any greeting ("hi", "hello", "Hey", etc.)
**Our backend checks the DB and responds with one of two versions:**

---

### FLOW 1A — New User (number NOT in DB)

```
MESSAGE TYPE : Interactive Flow Message
HEADER       : Image
               → File: register_header
               → Spec: 800×418px JPG/PNG, max 5MB
               → Content: EDMS branding / election campaign visual

MESSAGE BODY :
Namaste

Welcome to EDMS. Tap Choose Service below.

FOOTER       : EDMS
BUTTON       : [ Choose Service ]  ← opens the 6-option menu flow
```

**Inside the flow — 6 menu options:**

| # | ID | Title | Description |
|---|----|-------|-------------|
| 1 | `register` | Register | Create your EDMS account |
| 2 | `demo` | Demo | Watch a quick demo |
| 3 | `benefits` | Benefits | Why EDMS |
| 4 | `faq` | FAQ | Common questions |
| 5 | `website` | Website | Visit our website |
| 6 | `support` | Support | Talk to our team |

---

### FLOW 1B — Returning User (number IS in DB)

```
MESSAGE TYPE : Interactive Flow Message
HEADER       : Image
               → File: welcome_back_header
               → Spec: 800×418px JPG/PNG, max 5MB
               → Content: "Welcome back" visual / EDMS brand

MESSAGE BODY :
Namaste

Welcome to EDMS. Tap Choose Service below.

FOOTER       : EDMS
BUTTON       : [ Choose Service ]  ← opens the 8-option menu flow
```

**Inside the flow — 8 menu options:**

| # | ID | Title | Description |
|---|----|-------|-------------|
| 1 | `credentials` | My Credentials | Get your login details |
| 2 | `social` | Social Media Request | WhatsApp / Audio / SMS broadcast |
| 3 | `purchase` | Purchase *(or "My Plan" if paid)* | Activate / view subscription |
| 4 | `demo` | Demo | Watch a quick demo |
| 5 | `benefits` | Benefits | Why EDMS |
| 6 | `faq` | FAQ | Common questions |
| 7 | `website` | Website | Visit our website |
| 8 | `support` | Support | Talk to our team |

---

## FLOW 2 — REGISTRATION (7 screens, inside WhatsApp flow)

**Trigger:** User selects "Register" from the menu
**Flow Type:** Meta WhatsApp Flow (data_exchange)
**Flow Endpoint:** `https://tnedms.com/api/whatsapp-flow-endpoint`

| Screen | Title | Input |
|--------|-------|-------|
| 1 — WELCOME | Candidate Registration | Full Name (text), Role (dropdown), Affiliation (dropdown) |
| 2 — PARTY_SELECT | Choose Party | Radio: DMK / AIADMK / BJP / INC / TVK / NTK / PMK / VCK / MDMK / AMMK / DMDK / Independent / Other |
| 3 — LOCAL_BODY | Local Body Type | Dropdown: Rural (Panchayats) / Urban (Municipal/Corp) |
| 4 — POSITION | Select Position | Dropdown: dynamic (loaded from server by body type) |
| 5 — DISTRICT | Select District | Dropdown: All 38 TN districts (loaded from server) |
| 6 — ASSEMBLY | Select Assembly | Dropdown: dynamic by district (loaded from server) |
| 7 — BOOTH_SELECT | Select Booth | Dropdown: dynamic by assembly (loaded from server) |
| 8 — SUMMARY_SUBMIT | Confirm & Register | Shows summary → Submit button |

**On Submit → our backend creates the account → MESSAGE A fires automatically:**

---

### MESSAGE A — Registration Success (sent right after submit)

```
MESSAGE TYPE : Interactive URL Button Message
HEADER       : Image
               → File: register_success_header
               → Spec: 800×418px JPG/PNG, max 5MB
               → Content: Congratulations / celebration visual

MESSAGE BODY :
Congratulations *{Name}*! 🎉 Your registration has been completed
successfully.

🔑 *Username:* `{mobile number}`
🔒 *Passcode:* `{password}`

Tap below to log in to your Election Management Dashboard!

BUTTON       : [ Login Now ]  → https://tnedms.com/login
```

> **Note:** Simultaneously, a WATI template (`edms_login_v1`) also fires
> as a backup delivery. See WATI section at the end.

---

## FLOW 3 — MY CREDENTIALS

**Trigger:** Returning user selects "My Credentials" → INFO screen shown → taps Close

**INFO screen shown inside flow:**
```
Title : Your Credentials
Body  : Tap Close and your login details will be sent to you in the chat.
Button: [ Close ]
```

**On Close → MESSAGE B fires:**

### MESSAGE B — Credentials

```
MESSAGE TYPE : Interactive URL Button Message
HEADER       : Image
               → File: welcome_back_header
               → Spec: 800×418px JPG/PNG, max 5MB
               → Content: EDMS "Welcome back" visual

MESSAGE BODY :
Welcome back *{Name}*! You are already registered.

🔑 *Username:* `{mobile number}`
🔒 *Passcode:* `{password}`

Use these credentials to log in to the Election Management Dashboard.

BUTTON       : [ Login Now ]  → https://tnedms.com/login
```

---

## FLOW 4 — DEMO

**Trigger:** User selects "Demo" → INFO screen shown → taps Close

**INFO screen shown inside flow:**
```
Title : Demo
Body  : Tap Close and your demo will be sent to you in the chat.
Button: [ Close ]
```

**On Close → MESSAGE C fires:**

### MESSAGE C — Demo Video

```
MESSAGE TYPE : Interactive Flow Message
HEADER       : VIDEO
               → File: svc_demo_video
               → Spec: MP4, max 16MB, 16:9 recommended
               → Content: EDMS product demo / walkthrough video

MESSAGE BODY :
Here is a quick demo of EDMS.

FOOTER       : EDMS
BUTTON       : [ Choose Service ]  → reopens the services menu
```

---

## FLOW 5 — BENEFITS

**Trigger:** User selects "Benefits"
**Type:** Screen shown INSIDE the flow (no chat message sent)

```
INSIDE FLOW SCREEN:
Banner       : Image [svc_benefits_banner]
               → Spec: 1000×125px JPG/PNG (wide banner format)
               → Content: EDMS benefits visual / infographic

Content text :
✅ Know your voters — booth & ward level
✅ Analyse every ward & booth
✅ Identify key voter groups
✅ Find strong & weak areas
✅ Plan your campaign with data
✅ Real-time voter intelligence

Button       : [ Close ]  → no chat message sent
```

---

## FLOW 6 — FAQ

**Trigger:** User selects "FAQ"
**Type:** Screen shown INSIDE the flow (no chat message sent)

```
INSIDE FLOW SCREEN:
Banner       : Image [svc_faq_banner]
               → Spec: 1000×125px JPG/PNG (wide banner format)
               → Content: FAQ / Help visual

Content text :
Q: What is EDMS?
A: Election Data Management System — booth-level voter intelligence
   for local body election candidates in Tamil Nadu.

Q: How do I register?
A: Tap Register in this menu and complete the 7-step form.
   You'll receive your login credentials instantly on WhatsApp.

Q: What data will I get access to?
A: Booth-wise voter lists for your assigned ward / constituency.

Q: How much does it cost?
A: 1 booth: ₹2,360 | 2–10 booths: ₹5,900 |
   11–25 booths: ₹11,800 | 25+ booths: ₹29,500 (incl. 18% GST)

Q: Is my data secure?
A: Yes. Your data is protected and only accessible with your credentials.

Q: How do I make payment?
A: Tap Purchase in this menu to get a secure Razorpay payment link.

Button       : [ Close ]  → no chat message sent
```

---

## FLOW 7 — WEBSITE

**Trigger:** User selects "Website" → INFO screen shown → taps Close

**INFO screen shown inside flow:**
```
Title : Website
Body  : Tap Close and our website link will be sent to you in the chat.
Button: [ Close ]
```

**On Close → MESSAGE D fires:**

### MESSAGE D — Website Link

```
MESSAGE TYPE : Interactive URL Button Message
HEADER       : Image
               → File: svc_website_image
               → Spec: 800×418px JPG/PNG, max 5MB
               → Content: EDMS website / brand visual

MESSAGE BODY :
Explore EDMS on our website.

BUTTON       : [ Visit Website ]  → https://tnedms.com
```

---

## FLOW 8 — SUPPORT

**Trigger:** User selects "Support" → INFO screen shown → taps Close

**INFO screen shown inside flow:**
```
Title : Support
Body  : Tap Close and our support details will be sent to you in the chat.
Button: [ Close ]
```

**On Close → MESSAGE E fires:**

### MESSAGE E — Support Details

```
MESSAGE TYPE : Interactive Flow Message
HEADER       : Image
               → File: svc_support_image
               → Spec: 800×418px JPG/PNG, max 5MB
               → Content: Support / contact visual

MESSAGE BODY :
Need help? Our team is here for you.

Call us: +918106811285

FOOTER       : EDMS
BUTTON       : [ Choose Service ]  → reopens the services menu
```

---

## FLOW 9 — SOCIAL MEDIA REQUEST

**Trigger:** Returning user selects "Social Media Request"
**Type:** Screens shown INSIDE the flow

```
INSIDE FLOW SCREEN — Channel Select:
Banner  : Image [svc_social_banner]
          → Spec: 1000×125px JPG/PNG
          → Content: Social media / broadcast visual

Options :
  ├── WhatsApp Broadcast
  ├── Audio SMS
  └── SMS Broadcast
```

### 9A — WhatsApp Broadcast
```
Form fields : Message content, Target audience
Submit      → ✅ Request logged in DB, team notified
             → MESSAGE F fires:
```
**MESSAGE F:**
```
MESSAGE TYPE : Text
MESSAGE BODY : ✅ Your request has been submitted.
               Our team will get in touch with you shortly.
```

### 9B — Audio SMS
```
Form fields : Target audience, notes
Submit      → Bot sends MESSAGE G asking for audio file:
```
**MESSAGE G:**
```
MESSAGE TYPE : Text
MESSAGE BODY : Please reply with your audio file (mp3/ogg/m4a).
```
```
User replies with audio file
→ File received and stored
→ MESSAGE H fires:
```
**MESSAGE H:**
```
MESSAGE TYPE : Text
MESSAGE BODY : ✅ Your audio has been received.
               Our team will process and send your broadcast shortly.
```

### 9C — SMS Broadcast
```
Form fields : Message content, Target audience
Submit      → ✅ Request logged
             → MESSAGE F fires (same as 9A)
```

---

## FLOW 10 — PURCHASE (not paid)

**Trigger:** Registered user (not paid) selects "Purchase"
**Type:** Screen shown INSIDE the flow

```
INSIDE FLOW SCREEN:
Banner  : Image [svc_purchase_banner]
          → Spec: 1000×125px JPG/PNG
          → Content: Subscription / pricing visual

Content :
Activate your EDMS subscription.

Plan        : [tier name based on booth count]
Booths      : [count]
Amount      : ₹[price] (incl. 18% GST)

Pricing tiers:
  1 booth       → ₹2,360
  2–10 booths   → ₹5,900
  11–25 booths  → ₹11,800
  25+ booths    → ₹29,500

Button  : [ Get Payment Link ]  → then Close
```

**On Close → MESSAGE I fires:**

### MESSAGE I — Razorpay Payment Link

```
MESSAGE TYPE : Text
MESSAGE BODY : [Secure Razorpay payment link]
               (generated dynamically by our backend)

After payment → subscription activated automatically
```

---

## FLOW 11 — MY PLAN (paid user)

**Trigger:** Paid user selects "My Plan"
**Type:** Screen shown INSIDE the flow (no chat message sent)

```
INSIDE FLOW SCREEN:
Banner  : Image [svc_purchase_banner]  (same as Purchase)
          → Spec: 1000×125px JPG/PNG

Content :
Status   : ✅ Active
Booths   : {count}
Assembly : {assembly name}

Button  : [ Close ]  → no message sent
```

---

## WEBSITE — OTP & CREDENTIALS (via WATI)

These two messages are sent from our server using WATI templates.
WATI must have these two templates approved and active:

---

### WATI TEMPLATE 1 — OTP (`edms_otp`)

**Trigger:** User enters mobile number on tnedms.com/register and clicks Send OTP
**Category:** UTILITY

```
CREATIVE     : None (plain template)
PARAMETER    : {{code}} or {{1}} — the 6-digit OTP

MESSAGE BODY :
Your EDMS verification code is: *{code}*

This code is valid for 10 minutes.
Do not share this code with anyone.
```

---

### WATI TEMPLATE 2 — Credentials (`edms_login_v1`)

**Trigger:** User completes registration (web or WhatsApp)
**Category:** UTILITY

```
CREATIVE     : None (plain template)
PARAMETERS   :
  detail_one   = Login: https://tnedms.com/login
  detail_two   = User ID: {mobile number}
  detail_three = Password: {password}

MESSAGE BODY :
Your EDMS login credentials:

{detail_one}
{detail_two}
{detail_three}

Keep these safe. Do not share with anyone.
```

---

## CREATIVE ASSETS SUMMARY

All images/videos must be uploaded and linked to the keys listed below.
Upload via tnedms.com/flow-images (admin panel).

| Asset Key | Type | Dimensions | Used In |
|-----------|------|------------|---------|
| `register_header` | Image | 800×418px | Flow 1A — greeting for new user |
| `welcome_back_header` | Image | 800×418px | Flow 1B — greeting for returning user + My Credentials |
| `register_success_header` | Image | 800×418px | Message A — after registration |
| `svc_demo_video` | **Video** | MP4, max 16MB | Message C — demo |
| `svc_website_image` | Image | 800×418px | Message D — website |
| `svc_support_image` | Image | 800×418px | Message E — support |
| `svc_welcome_banner` | Image | 1000×125px | Inside services menu (top banner) |
| `svc_benefits_banner` | Image | 1000×125px | Inside Benefits screen |
| `svc_faq_banner` | Image | 1000×125px | Inside FAQ screen |
| `svc_social_banner` | Image | 1000×125px | Inside Social Media screen |
| `svc_purchase_banner` | Image | 1000×125px | Inside Purchase + My Plan screen |

---

## SUMMARY — ALL FLOWS

| Flow | Trigger | Type | Chat Message? |
|------|---------|------|--------------|
| 1A | User says "hi" (new) | Interactive flow | Yes — Choose Service menu |
| 1B | User says "hi" (returning) | Interactive flow | Yes — Choose Service menu |
| 2 | Selects Register → submits | 7-screen flow | Yes — Message A (credentials) |
| 3 | Selects My Credentials → Close | INFO screen | Yes — Message B (credentials) |
| 4 | Selects Demo → Close | INFO screen | Yes — Message C (video) |
| 5 | Selects Benefits | In-flow screen | No |
| 6 | Selects FAQ | In-flow screen | No |
| 7 | Selects Website → Close | INFO screen | Yes — Message D (link) |
| 8 | Selects Support → Close | INFO screen | Yes — Message E (phone) |
| 9A | Selects Social → WA Broadcast | In-flow form | Yes — Message F (confirm) |
| 9B | Selects Social → Audio SMS | In-flow form | Yes — Messages G + H (audio) |
| 9C | Selects Social → SMS Broadcast | In-flow form | Yes — Message F (confirm) |
| 10 | Selects Purchase → Close | In-flow screen | Yes — Message I (payment link) |
| 11 | Selects My Plan | In-flow screen | No |
| WATI-1 | Website OTP request | WATI template | Yes — OTP code |
| WATI-2 | Registration complete (web/WA) | WATI template | Yes — credentials |

---

*End of WATCO Setup Brief*
