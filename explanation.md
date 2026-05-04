# Unified Agentic Architecture — Beginner's Guide

## What is this system?

Imagine you have **3 Shopify apps**. Each app has a chat widget where merchants (store owners) can ask for help. Instead of a human support agent answering, an **AI** handles it — and if the AI needs to fix code, a human developer reviews it before anything is deployed.

That's the whole system in one sentence.

---

## The 4 Layers (Top to Bottom)

### 🖥️ Top Layer — The 3 Chat Apps
- Three separate Shopify apps, each with a chat UI.
- When a merchant sends a message, the app sends that message along with an **App ID** (so the system knows which app it came from) to the AI in the cloud.

### ☁️ Middle Layer — The AI Brain (DigitalOcean Agent)
This is where the smart stuff happens. The AI has 3 internal steps:

| Step | Name | What it does |
|------|------|--------------|
| 1 | **Reader** | Loads the manual for that specific app. Checks the merchant's settings in the database. |
| 2 | **Router** | Decides what kind of task this is. Simple chat? Use a cheap AI model (Llama). Need to fix code? Switch to a powerful model (Claude 3.5 Sonnet). |
| 3 | **Coder** | If a code fix is needed, it reads the Shopify theme file and writes a patch (Liquid/CSS code). |

> Think of it like a small team inside the AI: one reads, one decides, one codes.

### 🧑‍💻 Side Layer — Human Review (Ticketing Desk)
- The AI **never** pushes code directly to a live store.
- Instead, it sends a **"Proposed Fix"** to an internal dashboard (like a ticket system).
- A human developer reviews it and clicks **"Approve & Deploy"** only if it looks good.

### 🗄️ Bottom Layer — The Servers & Database
- **3 Node.js servers** — one backend per app.
- **1 Central MongoDB database** — stores all conversations and tickets from all 3 apps in one place, so the team can see everything in one dashboard.

---

## The 4 Bridges (How AI talks to your servers)

The AI cannot directly touch your database or Shopify store. It can only use specific "bridges" (API endpoints) that your developers build.

| Bridge | Purpose |
|--------|---------|
| **Diagnostic Bridge** | AI asks: "Does this merchant have X setting enabled?" |
| **File Bridge** | AI asks: "Give me the theme file for this store." (read-only) |
| **Action Bridge** | After approval, writes the fixed code back to Shopify. |
| **Ticketing Bridge** | Saves every conversation to the shared MongoDB dashboard. |

---

## The Golden Rule

> **The AI does the thinking. Your Node.js servers do the work.**

The AI never has direct access to your core app logic or database. It only uses the controlled tools your developers build for it. This keeps the system **safe, predictable, and easy to audit**.

---

## Quick Visual Summary

```
[ 3 Shopify Chat Apps ]
         ↓
[ AI Agent in the Cloud ]
   Reader → Router → Coder
         ↓              ↓
[ Your Node.js Servers ] [ Ticketing Desk → Human Review ]
         ↓
[ Central MongoDB ]
```

