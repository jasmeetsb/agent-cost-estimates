# Conversational Chatbot

**Use case:** Customer-support Q&A chatbot  ·  **Model:** gemini-2.5-flash  ·  **Pattern:** Single agent + light tools + Memory Bank
**Measured over 120 interactions** (2–5-turn (varying) conversation, ~8 model calls each).

## Architecture

```mermaid
graph TB
    User([User]) <--> Coord
    subgraph Engine["Agent Engine — conversational_chatbot"]
        direction TB
        Coord["chatbot_agent (Gemini 2.5 Flash)"]
        Coord -->|tool| FAQ[faq_lookup]
        Coord -->|tool| KB[kb_search]
        Coord -->|tool| PM[load_memory]
    end
    subgraph Core["Always-on Agent Platform SKUs"]
        direction LR
        Gemini[("Gemini 2.5 Flash<br/>per-token")]
        Runtime[("Agent Runtime<br/>vCPU + memory-sec")]
        Sess[("Sessions<br/>per event appended")]
        MB[("Memory Bank<br/>per memory + gen tokens")]
    end
    Engine -.-> Core
```

Single user-facing support agent (archetype: Conversational Chatbot, Moderate). Light tool use — `faq_lookup` + `kb_search` (stand-ins for a BigQuery/KB lookup) — and `load_memory` for returning-user personalization. Volume-driven archetype: cheap model, short turns. Measured ~7.5 model calls / ~15 session events per interaction.

## SKU usage per interaction

| SKU dimension | Per-interaction (avg) |
|---|---|
| Gemini input tokens | 6,369 |
| Gemini output tokens (incl. thinking) | 693 |
| — coordinator / sub split | 100% coordinator (single-agent) |
| Model calls | 7.5 |
| Agent Runtime — vCPU-seconds | 20.9 |
| Agent Runtime — memory GiB-seconds | 39 |
| Sessions — events appended | 15.0 |
| Memory Bank — generation tokens | 2,486 |
| Firestore — document writes / reads | 0.03 / 0.00 |
| Vertex AI Search (RAG) — queries | 2.15 |

## Derived cost per interaction

| Component | $ / interaction |
|---|---|
| Gemini tokens | 0.003600 |
| Agent Runtime (vCPU + memory) | 0.001853 |
| Memory Bank + Sessions | 0.004517 |
| Firestore | 0.000000 |
| Vertex AI Search (RAG) | 0.003225 |
| Model Armor (derived: all tokens scanned) | 0.000706 |
| **Total** | **$0.0139** |

## How to read these numbers

- **Usage quantities are the primary output**; the dollar column is a secondary, derived estimate.
- **$ = Cloud Billing Catalog list price**, not actual billed spend (no committed-use or contract discounts).
- **Agent Runtime** (vCPU / GiB-seconds) is amortized allocation time — an **upper bound**, not actual billed instance-time.
- **1 interaction = a 2–5-turn (varying) conversation.**
