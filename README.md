# Merchant Risk Intelligence System

Interactive prototype for a Product Manager assessment — a decision-support system for risk analysts at Payment Aggregators.

**Live demo →** https://YOUR-USERNAME.github.io/merchant-risk-demo/

---

## What this is

Payment Aggregators in India earn a few paise per ₹100 of MDR margin, while a single fraudulent merchant can cost lakhs in chargebacks and trigger regulatory action. This prototype demonstrates a system that helps risk analysts make merchant-onboarding decisions faster, with full evidence and complete audit trails.

The system ingests seven inputs about a merchant, runs them through an enrichment pipeline (deterministic rules + AI), and produces one of three verdicts:

- **Legitimate** — auto-approve at standard MDR
- **Needs Review** — route to analyst with full evidence brief
- **Suspicious** — reject and flag for AML review

---

## The architectural thesis

> **The AI's job is to produce evidence, not verdicts.**

Rules answer auditable questions ("is this GSTIN active?"). AI answers semantic questions ("does this website match the declared business?"). The deterministic decision engine combines both — the LLM is never on the verdict path.

This separation is what makes the system safe to deploy in a regulated context. Every decision is fully reproducible: input hash, model versions, prompt versions, and the deterministic logic applied are all captured in the audit log.

---

## Walkthrough (~90 seconds)

1. **Queue** — 5 pre-populated merchant applications across all three verdicts
2. **Submit** — enter the seven brief-required inputs (or use a preset for quick demo)
3. **Processing** — staggered enrichment animation across deterministic and AI tracks
4. **Decision** — verdict hero, analyst brief, side-by-side supporting vs concerning evidence
5. **All enrichments** — full dossier of every signal, organized by category
6. **Source drawer** — click any source to see the raw response data
7. **Audit log** — terminal-style decision record, replay-ready

---

## Try this

Submit a new merchant using one of the three presets:

| Preset | Expected verdict | Why |
|---|---|---|
| **Clean retail merchant** | Legitimate | Apparel store, clean signals across the board |
| **Nutraceutical brand** | Needs Review | High-risk MCC category — manual verification required |
| **Suspected transaction laundering** | Suspicious | Education claim + gambling-adjacent terms + suspicious TLD + cluster address |

Same inputs always produce the same verdict — the demo is fully reproducible.

---

## What's deterministic vs AI in this system

| Decision question | Handler |
|---|---|
| Is the GSTIN active? | Rule (API call) |
| Is the entity on a sanctions list? | Rule (list match) |
| Is the domain less than 30 days old? | Rule (WHOIS date math) |
| Does the website match the declared industry? | **AI** |
| Is this a gambling front disguised as something else? | **AI** |
| Is this entity linked to past flagged merchants? | Rule (graph DB) |
| Final verdict | Rule engine consuming both |

The discipline: if a deterministic rule could plausibly answer a question, AI shouldn't.

---

## Tech

- Single HTML file — React via CDN, no build step
- ~3,000 lines, ~110 KB
- All state in React (no backend required)
- Verdicts deterministic from input hash (same inputs → same verdict)

---

## Companion document

The full system design — risk strategy, AI-native reasoning, end-to-end pipeline, worked example — is documented in the accompanying PDF (submitted alongside this prototype).

---

Built as a take-home assessment for **Modus AI** (modussecure.com).
