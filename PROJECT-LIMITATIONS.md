# Project Limitations & Documentation Disclosure — BazaarHub AI Voice Shopping Assistant

This file states, plainly, what this repository is and is not, so that no reader (employer, client, or collaborator) is misled.

## 1. Basis of this documentation

- This documentation was reconstructed **solely from a ~53-second demonstration video** (a screen recording of the BazaarHub storefront, the ElevenLabs widget, and a Google Sheets log) plus the project's published LinkedIn caption.
- **No source code** is included in this repository, because none was available.
- **No workflow/automation export** is included, because none was available — and none is shown in the video.
- **No live deployment link** is provided; the only public link is the demo video on LinkedIn: https://lnkd.in/p/da7wR5GJ

## 2. Not confirmed from video

The following are explicitly **not confirmed from video** and are never presented as fact in this repository:

| Item | Why it's unconfirmed |
|---|---|
| Orchestration layer | No automation canvas (n8n or any other), webhook, or code appears in the video. The caption does not claim n8n, and neither does this documentation. |
| Summary-writing component | The Google Sheets log is visible, but what generates summaries and appends rows is never shown. |
| RAG internals | Vector store, embedding model, chunking strategy, retrieval ranking, and knowledge-base refresh process are not shown. |
| LLM / model | The model behind the agent's answers is not named in the video or caption. |
| Live data vs. snapshot | Whether answers read live store data or a synced knowledge snapshot is not shown. |
| Price & stock answers | The caption lists prices/availability as question types, but **no price or stock figure is quoted anywhere in the video**. |
| Cart/checkout actions by the agent | A cart and an "Apply coupon" suggestion chip exist on the page, but the agent is never shown adding items, applying coupons, or checking out. |
| Sheet column A | The visible sheet shows columns B (Summary) and C (Date); column A's header is cut off in the frame. |

## 3. Marketing material vs. demonstrated behaviour

- The video's **title card** ("Smart Shopping · Instant Answers · AI-Powered RAG", tiles for Voice Shopping / Smart Recommendations, and the pipeline graphic) is the project's own **promotional positioning**. This documentation reports those claims as *stated*, and separately lists what the live demo actually *demonstrates*.
- "Smart Recommendations" and "Instant Responses" are stated claims; the demo shows grounded Q&A, not a recommendation engine or measured latency.
- The storefront itself is a demo store (Season Sale, Flash Deals countdown, coupon WELCOME20); product facts quoted by the agent (two headphone models, MacBook colours, 30-day returns) are the demo knowledge base's contents, not a real retail catalogue.

## 4. Data notes

- The Google Sheets rows visible in the video are dated **2026-10-11** and describe demo conversations (a store-owner question and a headphones question). They evidence the logging behaviour, not production usage volume.
- The agent's statement identifying "Asfand" as BazaarHub's owner comes from a summary row visible in the video; it is reported as knowledge-base content, not independently verified.

## 5. What this repository is for

A faithful, evidence-based portfolio record of the BazaarHub demo: what the system does on screen, how its parts connect at the level the video supports, and an honest map of what remains undocumented. If the original project's code, agent configuration, or knowledge-base design becomes available, this documentation should be updated to replace every "Not confirmed from video" item with verified detail.
