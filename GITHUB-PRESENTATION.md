# GitHub Presentation — BazaarHub AI Voice Shopping Assistant

## Repository name

`bazaarhub-ai-voice-shopping-assistant`

## One-line description (repo "About")

> AI voice shopping assistant for an e-commerce store — ElevenLabs voice agent with a RAG knowledge base that answers product and policy questions and logs conversation summaries to Google Sheets.

## Topics / tags

```
elevenlabs
voice-ai
conversational-ai
rag
retrieval-augmented-generation
ai-agent
ecommerce
voice-shopping
google-sheets
ai-assistant
knowledge-base
portfolio
```

> Deliberately **not** tagged `n8n` — no automation canvas is shown in the demo and the project does not claim one.

## Website field (repo "About")

Set the website to the demo video: https://lnkd.in/p/da7wR5GJ

## Suggested commit messages

| Commit | Message |
|---|---|
| Initial docs | `docs: add README with evidenced feature set and architecture overview` |
| Workflow doc | `docs: document request lifecycle stages from demo video` |
| Architecture | `docs: add architecture, data flow and Mermaid diagram` |
| Setup | `docs: add setup guide with verified vs reconstructed steps` |
| Limitations | `docs: add limitations and not-confirmed-from-video disclosure` |
| Assets | `assets: add demo frames (title card, storefront widget, Sheets log)` |
| Demo link | `docs: add LinkedIn demo video link` |

## Upload instructions

### Option A — GitHub web UI
1. Create a new public repository named `bazaarhub-ai-voice-shopping-assistant` (no README/gitignore initialisation).
2. Upload all files from this folder, preserving structure:
   - Root: `README.md`, `WORKFLOW-EXPLANATION.md`, `PROJECT-ARCHITECTURE.md`, `SETUP-GUIDE.md`, `PROJECT-LIMITATIONS.md`, `GITHUB-PRESENTATION.md`
   - `assets/` → the three PNG frames
   - `demo/` → `VIDEO-LINK.md`
3. Set the About description and topics from this file; set the website field to the LinkedIn demo link.
4. Pin the repository to your profile if it is among your strongest voice-AI pieces.

### Option B — Git CLI
```bash
cd bazaarhub-ai-voice-shopping-assistant
git init
git add .
git commit -m "docs: BazaarHub AI voice shopping assistant — full documentation pack"
git branch -M main
git remote add origin https://github.com/AiwithFahad/bazaarhub-ai-voice-shopping-assistant.git
git push -u origin main
```

## Presentation advice

- **Lead with the demo link.** The video shows the whole system in under a minute; put the LinkedIn link at the top of the README (already done) and in the repo's website field.
- **Keep the honesty visible.** The "Demonstrated vs. Stated" split and the limitations file are strengths, not weaknesses — they show engineering judgement. Recruiters and clients trust documentation that separates evidence from claims.
- **Use the Sheets log as the differentiator.** Many voice-agent demos stop at Q&A; the automatic conversation summaries are the detail that shows product thinking — mention it in the repo description (already done).
- **Don't over-tag.** Only tag technologies the project actually evidences (ElevenLabs, RAG, Google Sheets). Adding unproven stack tags (e.g. n8n) invites questions the demo can't answer.
- **Next upgrade that would strengthen this repo:** publish a short architecture note (or redacted agent settings) covering the RAG stack and the summary-writing component, replacing the "Not confirmed from video" items with verified detail.
