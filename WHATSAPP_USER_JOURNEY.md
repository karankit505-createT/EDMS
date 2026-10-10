# WhatsApp User Journey — EDMS (tnedms.com)

---

## NEW USER (not registered) — 6 options

```
User sends "hi"
│
└── Sees: Welcome message + "Choose Service" button
          │
          └── Taps "Choose Service"  →  Opens 6-item menu inside WhatsApp
                   │
                   ├── 1. Register
                   │       │
                   │       ├── Screen 1: WELCOME         →  Intro text shown
                   │       ├── Screen 2: PARTY_SELECT    →  Dropdown: DMK / AIADMK / BJP / INC /
                   │       │                                          TVK / NTK / PMK / VCK /
                   │       │                                          MDMK / AMMK / DMDK / Independent / Other
                   │       ├── Screen 3: LOCAL_BODY      →  Dropdown: Panchayat / Municipality /
                   │       │                                          Town Panchayat / Corporation
                   │       ├── Screen 4: POSITION        →  Dropdown: Ward Member / President /
                   │       │                                          Councillor / Chairman / Mayor
                   │       ├── Screen 5: DISTRICT        →  Dropdown: All 38 TN districts
                   │       ├── Screen 6: ASSEMBLY        →  Dropdown: Assemblies in selected district
                   │       ├── Screen 7: BOOTH_SELECT    →  Dropdown: Booths in selected assembly
                   │       └── Submit
                   │               └── ✅ Account created
                   │                       └── Username + Passcode sent in chat
                   │
                   ├── 2. Demo
                   │       └── INFO screen: "Tap Close and your demo will be sent in the chat"
                   │               └── Close  →  ✅ Demo video sent in chat
                   │                               └── "Choose Service" button to go back
                   │
                   ├── 3. Benefits
                   │       └── BENEFITS screen
                   │               ├── Know your voters — booth & ward level
                   │               ├── Analyse every ward & booth
                   │               ├── Identify key voter groups
                   │               ├── Find strong & weak areas
                   │               ├── Plan campaign with data
                   │               └── Close
                   │
                   ├── 4. FAQ
                   │       └── FAQ screen
                   │               ├── Q: What is EDMS?
                   │               ├── Q: How do I register?
                   │               ├── Q: Is my data secure?
                   │               └── Close
                   │
                   ├── 5. Website
                   │       └── INFO screen: "Tap Close and our website link will be sent in the chat"
                   │               └── Close  →  ✅ "Visit Website" button → tnedms.com sent in chat
                   │
                   └── 6. Support
                           └── INFO screen: "Tap Close and our support details will be sent in the chat"
                                   └── Close  →  ✅ "Call Support" button + phone number sent in chat
```

---

## RETURNING USER (registered, not purchased) — 8 options

```
User sends "hi"
│
└── Sees: Welcome back message + "Choose Service" button
          │
          └── Taps "Choose Service"  →  Opens 8-item menu inside WhatsApp
                   │
                   ├── 1. My Credentials
                   │       └── INFO screen: "Tap Close and your login details will be sent in the chat"
                   │               └── Close  →  ✅ Username + Passcode + "Login Now" button sent in chat
                   │
                   ├── 2. Social Media Request
                   │       └── SOCIAL_SELECT screen  →  3 channel options
                   │               │
                   │               ├── WhatsApp Broadcast
                   │               │       └── Form (message content, target audience)
                   │               │               └── Submit  →  ✅ Request logged → team notified
                   │               │
                   │               ├── Audio SMS
                   │               │       └── Form  →  Submit
                   │               │               └── Bot: "Please reply with your audio file (mp3)"
                   │               │                       └── User sends mp3
                   │               │                               └── ✅ Request logged → team notified
                   │               │
                   │               └── SMS Broadcast
                   │                       └── Form (message content, target audience)
                   │                               └── Submit  →  ✅ Request logged → team notified
                   │
                   ├── 3. Purchase
                   │       └── PURCHASE screen
                   │               ├── Shows: Plan name / Booths / Amount (incl. 18% GST)
                   │               └── "Get Payment Link"
                   │                       └── Close  →  ✅ Secure Razorpay payment link sent in chat
                   │                                       └── Payment done → subscription activated
                   │
                   ├── 4. Demo
                   │       └── INFO screen  →  Close  →  ✅ Demo video sent in chat
                   │
                   ├── 5. Benefits
                   │       └── BENEFITS screen  →  Reads benefits text  →  Close
                   │
                   ├── 6. FAQ
                   │       └── FAQ screen  →  Reads FAQ text  →  Close
                   │
                   ├── 7. Website
                   │       └── INFO screen  →  Close  →  ✅ "Visit Website" button → tnedms.com sent in chat
                   │
                   └── 8. Support
                           └── INFO screen  →  Close  →  ✅ "Call Support" button + phone number sent in chat
```

---

## RETURNING USER (registered + purchased) — 8 options

```
User sends "hi"
│
└── Sees: Welcome back message + "Choose Service" button
          │
          └── Taps "Choose Service"  →  Opens 8-item menu inside WhatsApp
                   │
                   ├── 1. My Credentials
                   │       └── INFO screen: "Tap Close and your login details will be sent in the chat"
                   │               └── Close  →  ✅ Username + Passcode + "Login Now" button sent in chat
                   │
                   ├── 2. Social Media Request
                   │       └── SOCIAL_SELECT screen  →  3 channel options
                   │               │
                   │               ├── WhatsApp Broadcast
                   │               │       └── Form  →  Submit  →  ✅ Request logged → team notified
                   │               │
                   │               ├── Audio SMS
                   │               │       └── Form  →  Submit
                   │               │               └── Bot: "Please reply with your audio file (mp3)"
                   │               │                       └── User sends mp3  →  ✅ Request logged
                   │               │
                   │               └── SMS Broadcast
                   │                       └── Form  →  Submit  →  ✅ Request logged → team notified
                   │
                   ├── 3. My Plan                         ← was "Purchase", now shows active plan
                   │       └── PURCHASE screen
                   │               ├── Status:   ✅ Active
                   │               ├── Booths:   <count>
                   │               ├── Assembly: <assembly name>
                   │               └── Close
                   │
                   ├── 4. Demo
                   │       └── INFO screen  →  Close  →  ✅ Demo video sent in chat
                   │
                   ├── 5. Benefits
                   │       └── BENEFITS screen  →  Reads benefits text  →  Close
                   │
                   ├── 6. FAQ
                   │       └── FAQ screen  →  Reads FAQ text  →  Close
                   │
                   ├── 7. Website
                   │       └── INFO screen  →  Close  →  ✅ "Visit Website" button → tnedms.com sent in chat
                   │
                   └── 8. Support
                           └── INFO screen  →  Close  →  ✅ "Call Support" button + phone number sent in chat
```
