# Project Architecture — BazaarHub AI Voice Shopping Assistant

## 1. Architecture overview

BazaarHub is a three-part system, of which two parts are directly visible in the demo video and one is visible only by its output:

| Part | Role | Visibility in video |
|---|---|---|
| **BazaarHub storefront** | The e-commerce page the shopper is browsing; hosts the voice entry card and widget | ✅ Fully visible |
| **ElevenLabs voice agent** | Conducts the spoken conversation: greeting, listening, answering, follow-ups, idle re-engagement | ✅ Visible through the widget UI and its on-screen transcript-style messages |
| **RAG knowledge base** | Product data & store content that grounds the agent's answers | ⚠️ Behaviour visible (store-specific answers); implementation not shown |
| **Google Sheets conversation log** | Receives one summary row (Summary \| Date) per conversation | ✅ Visible (the sheet itself); the writer component is not shown |

There is **no demonstrated automation layer** between the agent and the sheet. Any diagram or description that inserted one (e.g. n8n) would be fabrication — the project's own caption does not claim one either.

## 2. Component view

```
┌──────────────────────────────────────────────────────────────┐
│                    BazaarHub Storefront                       │
│  Categories · Search · Cart · Flash Deals · Season Sale       │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │ ElevenLabs voice card ("Shop Hands-free")                │ │
│  │  • suggestion chips  • microphone state                  │ │
│  └─────────────────────────────────────────────────────────┘ │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │ Voice widget                                             │ │
│  │  • states: Listening / Talk to interrupt                 │ │
│  │  • typed input: "Send a message…"  • mic / mute / send   │ │
│  └─────────────────────────────────────────────────────────┘ │
└───────────────────────────┬──────────────────────────────────┘
                            │ shopper question (voice/text)
                            ▼
                ┌───────────────────────┐        grounds answers
                │  BazaarHub AI Agent   │◄──────────────────────┐
                │  (ElevenLabs)         │                       │
                │  greeting · dialogue  │              ┌────────┴─────────┐
                │  follow-ups · nudge   │              │ RAG Knowledge    │
                └───────────┬───────────┘              │ Base             │
                            │                          │ Product Data &   │
                            │ conversation ends        │ Store Content    │
                            ▼                          └──────────────────┘
                ┌───────────────────────┐
                │  Summary writer       │  ← implementation NOT shown
                │  (not confirmed)      │    (see §5)
                └───────────┬───────────┘
                            ▼
                ┌───────────────────────┐
                │  Google Sheets log    │
                │  Summary | Date       │
                └───────────────────────┘
```

## 3. Data flow

| # | From → To | Data | Evidence |
|---|---|---|---|
| 1 | Shopper → Widget | Voice utterance or typed message | Widget states "Listening", "Send a message…" field |
| 2 | Widget/Agent → Knowledge base | The shopper's question (retrieval query) | Inferred from grounded answers — mechanism not shown |
| 3 | Knowledge base → Agent | Relevant product/store content (models, colours, return policy, store facts) | Answers quote store-specific facts verbatim |
| 4 | Agent → Shopper | Spoken answer + follow-up offer | On-screen agent messages throughout the demo |
| 5 | Agent → Shopper (idle) | Re-engagement nudge referencing conversation context | "Are you still there? … questions you have about the MacBook Air …" |
| 6 | Conversation → Google Sheets | One-paragraph summary + timestamp, appended as a row | Sheet with Summary \| Date columns and rows dated 2026-10-11 21:25 / 22:25 |

## 4. Integration overview

| Integration | Purpose | Confirmed? |
|---|---|---|
| ElevenLabs ↔ Storefront | Embedded voice card + widget on the store page | ✅ Yes — branding and widget visible |
| Agent ↔ RAG knowledge base | Grounding answers in product data & store content | ✅ Stated (caption + title card) and consistent with answers; ⚠️ implementation not shown |
| Conversation → Google Sheets | Append summary + date per conversation | ✅ Output visible; ⚠️ writing component not shown |
| Agent ↔ Store cart/checkout | — | ❌ Not demonstrated (cart exists on the page; no agent cart action is shown) |
| Agent ↔ Live inventory/pricing | — | ❌ Not demonstrated (no price or stock answer in the video) |

## 5. Mermaid flowchart (evidenced components only)

```mermaid
flowchart TD
    A[Shopper on BazaarHub storefront] --> B[Clicks voice button on ElevenLabs card]
    B --> C[Widget opens - agent greets]
    C --> D{Shopper asks a question}
    D -->|voice| E[Listening state - speech input]
    D -->|typed| F[Send a message input]
    E --> G[Retrieve from RAG knowledge base<br/>Product Data and Store Content]
    F --> G
    G --> H[Agent speaks grounded answer]
    H --> I[Agent offers a follow-up<br/>more detail / another attribute / policy]
    I --> D
    D -->|shopper idle| J[Re-engagement nudge<br/>references conversation context]
    J --> D
    D -->|conversation ends| K[Summary generated<br/>writer not confirmed from video]
    K --> L[(Google Sheets log<br/>Summary | Date)]

    style K stroke-dasharray: 5 5
```

Dashed node = the step whose implementation is **not confirmed from video**.

## 6. Knowledge base (as evidenced)

- **Inputs (stated):** "Product Data & Store Content" — per the title-card pipeline graphic.
- **Observable coverage:** product models (Sony WH-1000XM5, Bose QuietComfort 45), product attributes (MacBook Air 15" M3 colours), store policies (30-day returns, buyer protection), and store facts (owner's name, per the logged summary).
- **Not confirmed from video:** file formats, vector store, embedding model, chunking, retrieval ranking, and the refresh/update process when products or policies change.

## 7. Design observations (from the demo)

- **Voice-first phrasing:** numbers and model names are spoken in words ("WH-one thousand XM five"), which is how the agent's answers are rendered for speech.
- **Always a next step:** every demonstrated answer ends with an offer — more detail, the next attribute, or another topic — keeping the session alive.
- **Context-aware idle handling:** the re-engagement nudge names the product under discussion rather than using a generic prompt.
- **Observability by default:** summarising every conversation into Sheets turns the assistant into a source of voice-of-customer data, not just an answer machine.
