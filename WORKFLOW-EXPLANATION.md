# Workflow Explanation — BazaarHub AI Voice Shopping Assistant

## A note on what "workflow" means here

This project has **no visible automation canvas**. The demo video never shows a workflow tool (no n8n canvas, no executions panel, no node graph of any kind), and the project's published caption does **not** claim one. So this document does not invent nodes.

Instead, it documents the **request lifecycle that is actually evidenced on screen**, stage by stage — the same discipline that would be applied node-by-node if a canvas existed. The one stage whose implementation is not visible (how summaries reach Google Sheets) is isolated and labelled in the final section.

---

## The evidenced request lifecycle

### Stage 0 — Entry point: the on-page voice card

- **What happens:** The BazaarHub storefront carries an ElevenLabs-branded voice card ("Shop Hands-[free]"). It invites the shopper to "Click the voice b[utton] and try:" and shows suggestion chips ("Search for…", "Apply coup[on]…") plus a "Microphone ena[bled]" state.
- **Purpose:** Convert a passive page view into a conversation without leaving the store page.
- **Input:** A click/tap on the voice button (or a suggestion chip).
- **Output / connection:** Opens the voice widget and starts Stage 1.

### Stage 1 — Session start and greeting

- **What happens:** The widget opens; the agent greets the shopper: *"Hello! Welcome to BazaarHub. What can I help you find today?"*
- **Purpose:** Establish the conversational session and invite a shopping intent.
- **Input:** Widget session start.
- **Processing:** Agent opens with a fixed welcome and an open-ended prompt.
- **Output / connection:** Shopper's first question → Stage 2.

### Stage 2 — Shopper input: voice or typed

- **What happens:** The shopper speaks (widget shows **"Listening"**) or types into the **"Send a message…"** field. The widget offers mic, mute, and send controls, and supports interruption — **"Talk to interrupt"** — while the agent is speaking.
- **Purpose:** Capture the question in the shopper's natural phrasing.
- **Input:** Spoken utterance or typed text (e.g. *"Which headphones are available on BazaarHub?"*).
- **Processing:** Voice input is handled by the ElevenLabs agent's speech pipeline (speech-to-text internals **not confirmed from video**).
- **Output / connection:** The understood question → Stage 3.

### Stage 3 — Knowledge retrieval (RAG)

- **What happens:** The question is answered from a knowledge base built — per the caption and the title-card pipeline graphic — from **"Product Data & Store Content"** feeding a **"RAG Knowledge Base."**
- **Purpose:** Ground the answer in this store's catalogue and policies instead of generic model knowledge.
- **Input:** The shopper's question.
- **Processing:** Retrieval of the relevant product/store content, then answer generation conditioned on it. **The vector store, embedding model, chunking, and ranking are not shown — not confirmed from video.**
- **Evidence it works this way:** answers contain specific store facts — two named headphone models, four named MacBook Air colours, the store's exact 30-day return policy, and (in a logged summary) the store owner's name.
- **Output / connection:** Retrieved facts → Stage 4.

### Stage 4 — Spoken answer with a follow-up offer

- **What happens:** The agent speaks the answer in voice-friendly phrasing (product names spelled for speech: *"Sony WH-one thousand XM five"*, *"Bose QuietComfort forty-five"*) and immediately offers the next step.
- **Demonstrated exchanges:**
  1. **Product discovery** — *"Which headphones are available on BazaarHub?"* → Sony WH-1000XM5 and Bose QuietComfort 45 → *"Would you like to know more about either of these models?"*
  2. **Product attribute** — *"What colors are available for the Apple MacBook Air fifteen-inch M3?"* → midnight, starlight, space gray, silver → *"Would you like to know about the available memory options for this laptop?"*
  3. **Store policy** — *"Can you tell me the return policy of BazaarHub?"* → *"a thirty-day hassle-free return policy with full buyer protection for eligible products"* → *"Would you like to know anything else about our policies or products?"*
- **Purpose:** Answer the question *and* keep the shopping conversation moving.
- **Input:** Retrieved facts from Stage 3.
- **Output / connection:** Shopper's next question loops back to Stage 2 — or the session goes idle (Stage 5) or ends (Stage 6).

### Stage 5 — Idle re-engagement

- **What happens:** After a pause in the conversation, the agent proactively checks in: *"Are you still there? I am happy to help you with any other questions you have about the MacBook Air or our other products."*
- **Purpose:** Recover a stalled session by referencing the conversation context (here, the MacBook Air just discussed).
- **Input:** Inactivity during an open session.
- **Output / connection:** Shopper response loops to Stage 2; no response leads to session end → Stage 6.

### Stage 6 — Conversation summary logged to Google Sheets

- **What happens:** Each conversation appears in a Google Sheets log as one row: a **Summary** (column B) and a **Date** (column C).
- **Rows visible in the video:**
  - *"The user inquired about the owner of BazaarHub. The agent identified Asfand as the owner. The convers[ation]…"* — `2026-10-11 21:25`
  - *"The user inquired about available headphones. The agent listed two models: Sony WH-1000XM5 and Bo[se]…"* — `2026-10-11 22:25`
- **Purpose:** Give the store a persistent, reviewable record of what shoppers ask — product interest, policy interest, and store questions — without listening to call recordings.
- **Input:** The finished conversation.
- **Processing:** Summarisation of the conversation into one paragraph, timestamped, and appended as a new row.
- **Output:** The growing Sheets log (shown at ~0:07 and again near ~0:50 in the video).

---

## Lifecycle at a glance

```
[Voice card on storefront] → [Widget opens, agent greets]
        → [Shopper asks by voice or text]
        → [RAG retrieval over product data & store content]
        → [Spoken answer + follow-up offer] ──loops──┐
        → [Idle? → re-engagement nudge] ────────────┘
        → [Conversation ends → summary + date appended to Google Sheets]
```

## Not confirmed from video — the orchestration layer

The following are **not shown anywhere in the video** and are deliberately **not** documented as fact:

- **Which component generates the conversation summaries and writes them to Google Sheets.** No automation canvas (n8n or otherwise), webhook, API call, or code is shown. The summariser could be an agent platform feature, a server tool, or a separate automation — the video does not say.
- **The LLM** powering the agent's answers.
- **RAG internals:** vector database, embedding model, chunk size/overlap, metadata filtering, and how the knowledge base is refreshed when the catalogue changes.
- **Whether answers are read from live store data** (e.g. a live catalogue API) or from a periodically synced knowledge snapshot.
- **Price and stock behaviour:** the caption names prices and availability as question types, but no price or stock answer occurs in the video.
- **Checkout/cart actions:** a Cart exists on the storefront and a "Apply coupon" suggestion chip is visible, but the agent is never shown adding items, applying a coupon, or checking out.
