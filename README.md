# Agent cost estimates

Curated **per-interaction usage results** for representative GCP agent architectures, to seed a cost calculator. Each agent below links to its architecture and measured SKU usage. Start here, plug the per-interaction numbers into the calculator as defaults, and open an agent's page to see what architecture produced them.

## Archetypes

| Archetype | Interactions | Turns | Input tok | Output tok | Model calls | Runtime vCPU-s | Sessions | Mem-gen tok | Mem retr | Firestore W/R | RAG | Grounding | $/intxn |
|---|--:|--:|--:|--:|--:|--:|--:|--:|--:|:--:|--:|--:|--:|
| [Conversational Chatbot](archetypes/conversational-chatbot.md) | 120 | 432 | 6,369 | 693 | 7.5 | 20.9 | 15.0 | 2,486 | 0.00 | 0.03/0.00 | 2.15 | 0.00 | 0.0139 |
| [Workflow Operator](archetypes/workflow-operator.md) | 118 | 425 | 20,107 | 1,485 | 14.0 | 25.3 | 27.9 | 2,549 | 0.67 | 1.42/1.23 | 0.00 | 0.00 | 0.0232 |
| [Autonomous Researcher](archetypes/autonomous-researcher.md) | 79 | 253 | 32,585 | 10,739 | 7.8 | 171.2 | 15.6 | 7,999 | 0.38 | 1.34/2.06 | 1.18 | 1.62 | 0.0822 |
| [Multi-Agent Orchestrator](archetypes/multi-agent-orchestrator.md) | 120 | 432 | 149,080 | 6,080 | 18.9 | 90.6 | 37.9 | 2,793 | 0.20 | 0.29/0.63 | 0.42 | 0.00 | 0.0932 |

## How these were measured

- Each agent was built on Google's **Agent Development Kit (ADK)**, deployed to **Vertex AI Agent Engine**, and run for its stated number of interactions (~80–120 each, multi-turn).
- **Model: gemini-2.5-flash** for all usage and cost numbers.
- Token usage from the model response (`usage_metadata`); runtime + Memory Bank from Cloud Monitoring; RAG / grounding / Firestore counted from the agent's tool calls; Imagen from Cloud Monitoring.
- **Dollars are Cloud Billing Catalog list-price estimates**, not billed spend. Usage quantities are the primary output; cost is a secondary derived view.
- The **coordinator vs sub-agent token split %** (where shown) comes from a separate two-model measurement (coordinator on gemini-3.5-flash, sub-agents on gemini-3.1-flash-lite).

_Numbers are estimates for planning, not a billing guarantee._
