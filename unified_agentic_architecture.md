# The Unified Agentic Architecture

## 1. Step-by-Step Logic Flow

| Stage | Component | Action |
|-------|-----------|--------|
| **ENTRY** | Merchant Chat | The chat UI (App 1, 2, or 3) sends a message + `App_ID` to the DigitalOcean Agent API. |
| **STEP 1** | The Reader | Accesses the Knowledge Base. Pulls the "Manual" for the specific `App_ID`. Runs a diagnostic check (e.g., calling your App's API to check database toggles). |
| **STEP 2** | The Router | Analyzes the Reader's findings. If it's a chat task, it keeps it cheap (Llama). If it's a code fix, it upgrades the "Brain" to Claude 3.5 Sonnet. |
| **STEP 3** | The Coder | Requests the theme file from your App Server. Generates a Liquid/CSS patch to fix the widget visibility. |
| **STEP 4** | Ticketing Desk | Instead of auto-applying, the "Proposed Fix" is sent to your Internal Dashboard (the Intercom alternative). |
| **EXIT** | Human Developer | Your team reviews the AI's work on the desk and clicks **"Approve & Deploy."** |

---

## 2. Developer Integration Points

> The "Work" for your team — your Node.js developers need to build these 4 specific bridges:

### The Diagnostic Bridge
An API endpoint that lets the **Reader** check if a merchant has specific settings enabled in your database.

### The File Bridge
An API endpoint that lets the **Coder** "Read" a Shopify theme file (safely scoped to only Liquid/CSS).

### The Action Bridge
An API endpoint that "Writes" the approved code back to the Shopify store.

### The Ticketing Bridge
A shared MongoDB where the AI saves every conversation, allowing your team to see all 3 apps in one dashboard.

---

## 3. Interview "Whiteboard" Summary

If you ask a developer to draw this on a whiteboard during an interview, it should look like this:

```
┌─────────────────────────────────────────────────┐
│        TOP LAYER: 3 Shopify Apps (Frontend Chat) │
└─────────────────────────┬───────────────────────┘
                          │
┌─────────────────────────▼───────────────────────┐
│         MIDDLE LAYER (Cloud): DO AI Agent        │
│                                                  │
│        Reader  →  Router  →  Coder               │
└──────┬──────────────────────────────────┬────────┘
       │                                  │
       │                        ┌─────────▼────────┐
       │                        │   SIDE LAYER:     │
       │                        │  Private Internal │
       │                        │  Desk (Human-in-  │
       │                        │  the-Loop Review) │
       │                        └──────────────────┘
┌──────▼──────────────────────────────────────────┐
│          BOTTOM LAYER (Infrastructure)           │
│   3 Node.js Servers  +  1 Central MongoDB        │
└─────────────────────────────────────────────────┘
```

---

## Key Design Principle

> **Separation of Concerns** — the AI does the thinking, and your Node.js servers do the work.

This setup ensures that **AI never has direct access to your core app logic**, only to the specific "Tools" your developers build for it.
