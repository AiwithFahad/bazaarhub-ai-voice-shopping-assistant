# Setup Guide — BazaarHub AI Voice Shopping Assistant

This guide explains how to recreate a system like the one in the demo. It is split into three honesty tiers:

- ✅ **Verified from video** — visible on screen in the demonstration.
- 📣 **Stated** — claimed by the project's published caption or the video's title card.
- 🛠️ **Suggested reconstruction** — a reasonable way to rebuild it; **not** how the original is confirmed to work.

> **No secrets are included anywhere in this guide.** Only credential *types* are named. Never commit API keys to a repository.

---

## 1. What you need

| Component | Purpose | Tier |
|---|---|---|
| ElevenLabs account with a conversational voice agent | The voice assistant embedded on the store page | ✅ Verified (branding/widget in video) |
| A knowledge source: product data + store content (catalogue, policies, store facts) | Grounding for the agent's answers | 📣 Stated ("Product Data & Store Content → RAG Knowledge Base") |
| A RAG / knowledge-base mechanism attached to the agent | Retrieval of relevant content per question | 📣 Stated; 🛠️ implementation choice is yours |
| An e-commerce page (the BazaarHub demo storefront) to embed the widget on | Shopper entry point | ✅ Verified |
| A Google account + Google Sheet with columns **Summary \| Date** | Conversation summary log | ✅ Verified (sheet visible) |
| A mechanism that summarises each conversation and appends it to the sheet | Logging | 🛠️ Suggested reconstruction — the original mechanism is **not confirmed from video** |

### Credential types required (names only — no values)

| Credential | Used by | Tier |
|---|---|---|
| ElevenLabs API key / agent credentials | Agent configuration and widget embedding | 🛠️ Suggested reconstruction |
| Google account access (OAuth) for Sheets | Whatever writes summary rows to the sheet | 🛠️ Suggested reconstruction |
| Knowledge-base / vector-store credentials (if an external store is used) | Retrieval layer | 🛠️ Suggested reconstruction |

## 2. Build steps

### Step 1 — Prepare the knowledge content 📣/🛠️
- Assemble the store's product data: names, models, attributes (colours, sizes, memory options), and categories. The demo answers questions about headphone models and MacBook Air colours, so attribute-level data must be present.
- Add store content: return policy (the demo quotes a **30-day hassle-free return policy with full buyer protection for eligible products**), store facts (a logged summary shows the agent knew the owner's name), shipping/buyer-protection text, and FAQs.
- Keep one fact per passage where possible so retrieval returns clean, answerable chunks. *(Chunking specifics: 🛠️ suggested — the original's are not confirmed from video.)*

### Step 2 — Build the RAG knowledge base 🛠️
- Load the product/store content into the knowledge mechanism your agent platform provides (or an external vector store).
- Test retrieval with the demo's own questions before going live:
  - "Which headphones are available?" → must return the Sony WH-1000XM5 and Bose QuietComfort 45.
  - "What colors are available for the MacBook Air 15-inch M3?" → midnight, starlight, space gray, silver.
  - "What is the return policy?" → the 30-day policy text.
- The embedding model, vector store, and chunk settings are your choice — **none are confirmed from the video**.

### Step 3 — Configure the ElevenLabs agent ✅/🛠️
- Create a conversational agent and attach the knowledge base from Step 2.
- Set the greeting to match the demo: *"Hello! Welcome to BazaarHub. What can I help you find today?"* ✅
- Instruct the agent to (observed behaviours ✅):
  - answer only from store knowledge for product/policy facts,
  - speak model names and numbers in voice-friendly form,
  - end answers with a relevant follow-up offer (more detail, next attribute, another policy),
  - send a context-aware re-engagement message when the shopper goes idle ("Are you still there? …").
- Enable the typed-input fallback in the widget ("Send a message…") ✅ and interruption ("Talk to interrupt") ✅.

### Step 4 — Embed the agent on the storefront ✅
- Add the ElevenLabs voice card to the store page next to the shopping content, as in the demo: a branded card ("Shop Hands-free") with suggestion chips ("Search for…", "Apply coup[on]…") and a microphone-enabled state.
- Confirm the widget's Listening state, mic/mute/send controls, and message field all work on desktop and mobile.

### Step 5 — Create the conversation log ✅
- Create a Google Sheet with (at least) the columns **Summary** and **Date** — exactly as seen in the video (columns B and C of the shown sheet; a column A exists but its header is cut off in the frame — **not confirmed from video**).
- Decide the timestamp format; the demo's rows read like `2026-10-11 21:25` and `2026-10-11 22:25`.

### Step 6 — Connect conversation summaries to the sheet 🛠️
- The video proves the *output* (one summary row per conversation) but not the *mechanism*. Suggested reconstruction options:
  - the agent platform's post-conversation webhook → a small server/automation that summarises and appends a row, or
  - the platform's built-in analytics/export, if it can write summaries to Sheets.
- Whichever route you choose: one row per conversation, summary text + timestamp, appended (never overwriting earlier rows).
- **Do not document your chosen route as "the" original architecture** — the original is not confirmed from video.

### Step 7 — Verify end to end ✅ (mirror the demo)
1. Ask for available headphones → expect both named models + a follow-up offer.
2. Ask for MacBook Air 15" M3 colours → expect the four colours + a memory-options offer.
3. Ask for the return policy → expect the 30-day policy + buyer protection.
4. Go idle mid-conversation → expect the re-engagement nudge.
5. End the conversation → check a new summary row with the current timestamp appears in the sheet.

## 3. What this guide cannot tell you

- The original's LLM, voice ID, or agent settings beyond the behaviours listed.
- The original's RAG stack (vector store, embeddings, chunking, refresh process).
- The original's summary-writing component.
- Any live deployment URL — none is published with the demo.
