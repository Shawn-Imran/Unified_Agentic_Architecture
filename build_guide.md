# How to Build the Unified Agentic Architecture — Step by Step

---

## Phase 1 — Set Up the Database

1. Create a **MongoDB Atlas** cluster (free tier is fine to start).
2. Create a single database called `wenexus_central`.
3. Add two collections: `conversations` and `tickets`.
4. Save the **connection string** — all 3 Node.js servers will use it.

---

## Phase 2 — Build the 3 Node.js Backend Servers

Each Shopify app gets its own Node.js + Express server. Do this once per app.

1. Initialize a Node.js project: `npm init -y`
2. Install dependencies: `npm install express mongoose dotenv`
3. Connect the server to the central MongoDB using the connection string from Phase 1.
4. Create a basic `/health` route to confirm the server is running.
5. Deploy each server (e.g., on DigitalOcean App Platform or a Droplet).

---

## Phase 3 — Build the 4 Bridges (API Endpoints)

Add these routes to your Node.js servers:

| Bridge | Method | Example Route | What it does |
|--------|--------|---------------|--------------|
| Diagnostic | `GET` | `/api/merchant/settings` | Returns merchant's settings from DB |
| File | `GET` | `/api/theme/file` | Returns a Liquid/CSS file (read-only) |
| Action | `POST` | `/api/theme/apply` | Writes approved code to Shopify |
| Ticketing | `POST` | `/api/tickets/save` | Saves conversation/ticket to MongoDB |

> Keep each route simple and focused. No bridge should do more than one job.

---

## Phase 4 — Set Up the DigitalOcean AI Agent

1. Go to **DigitalOcean** → Create a new **AI Agent**.
2. Create a **Knowledge Base** and upload a manual for each App ID (a text/markdown file describing what the app does).
3. Connect the agent to your 4 bridge endpoints as **Tools** (DigitalOcean lets you register external API endpoints as tools).
4. Set the default model to **Llama** (cheap, for chat).
5. Configure a rule: if the task involves code, switch to **Claude 3.5 Sonnet**.

---

## Phase 5 — Build the Internal Ticketing Dashboard

This is the internal tool your team uses to review AI-proposed fixes.

1. Create a simple web app (Next.js or plain React).
2. Connect it to the central MongoDB.
3. Show a list of open tickets (proposed fixes from the AI).
4. Add two buttons per ticket: **"Approve & Deploy"** and **"Reject"**.
5. Approve triggers a call to the **Action Bridge** to push code to Shopify.

---

## Phase 6 — Build the Shopify App Chat UI

Do this for each of the 3 Shopify apps.

1. Use **Shopify CLI** to scaffold the app.
2. Add a chat widget to the app's frontend.
3. On message send, call the DigitalOcean Agent API with:
   - The merchant's message
   - The `App_ID` to identify which app it came from
4. Display the AI's response in the chat window.

---

## Phase 7 — Test End to End

1. Open the chat in one of your Shopify apps.
2. Send a message like: *"My widget is not showing."*
3. Confirm the AI reads the correct manual (check logs).
4. Confirm a ticket appears in your internal dashboard.
5. Click **Approve** and verify the fix is applied to the Shopify store.

---

## Tech Stack Summary

| Layer | Technology |
|-------|-----------|
| Shopify Apps | Shopify CLI + React |
| Backend Servers | Node.js + Express |
| Database | MongoDB Atlas |
| AI Agent | DigitalOcean AI Agent |
| AI Models | Llama (chat), Claude 3.5 Sonnet (code) |
| Internal Dashboard | React / Next.js |

---

> **Rule of thumb:** Build and test one phase fully before moving to the next. Don't wire the AI to live stores until Phase 7.

