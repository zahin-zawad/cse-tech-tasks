*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# 2. System Overview

## 2.1 Design Principles

1. **Facts come from code and the database. Language comes from AI.** Gemini understands and writes messages, but every price, stock figure, delivery charge and total is produced by code from the database.
2. **No message is lost.** Every incoming message is accepted immediately, queued and processed, even during failures.
3. **A human is always reachable.** If the agent is unsure or something fails, the chat goes to a person.
4. **Everything is logged.** Every message, decision, action and cost can be traced.
5. **The owner stays in control.** The owner sets prices, discounts, rules and limits. The agent never goes outside them.
6. **One brain, many channels.** The details of each platform live in a connector. The agent logic is the same on every channel.
7. **Every platform's rules are obeyed.** A Policy Engine checks each outgoing message against the platform's rules before it is sent.

## 2.2 Why Python and Rust

| Language | Role | Reason |
|---|---|---|
| **Rust** | Webhook gateway and core services | Fast, memory-safe and reliable under heavy load. Suitable for verifying signatures, removing duplicate events, rate limiting and queueing |
| **Python** | AI agent, order logic and integrations | Strong ecosystem for AI, speech, vision, data processing and APIs. Suitable for Gemini, order handling, the calling agent and analytics |

## 2.3 Architecture

```
Customers on:  Facebook · Instagram · WhatsApp · X · LinkedIn
               (messages, comments, voice, images, documents)
                              ↓
              Platform webhooks (HTTPS)
                              ↓
┌─────────────────────────────────────────────────┐
│  RUST: Gateway Service                          │
│  • Verify each platform's signature             │
│  • Remove duplicate events                      │
│  • Tag channel and shop, rate limiting          │
│  • Reply 200 OK, put event in the queue         │
└─────────────────────────────────────────────────┘
                              ↓  (queue)
┌─────────────────────────────────────────────────┐
│  PYTHON: Agent Service                          │
│  1. Channel connector: convert to common format │
│  2. Load customer profile and conversation      │
│  3. Convert voice / image / document            │
│  4. Gemini call 1: understand message           │
│  5. Run the right handler (code)                │
│  6. Gemini call 2: write the reply (shop style) │
│  7. Validate the reply against facts            │
│  8. Channel Policy Engine: is sending allowed?  │
│  9. Channel connector: send, save state, log    │
└─────────────────────────────────────────────────┘
        ↓                ↓                ↓
   Database         Integrations     Background Workers
 (PostgreSQL)     (Courier, Payment,  (Follow-ups, alerts,
                   Voice call API,     call confirmation,
                   Telegram/Email)     token checks)
                              ↓
   ONE DASHBOARD: unified inbox · human handoff · owner notifications
                  channel health · analytics · costs per channel
                              ↓
                  Logging and monitoring
```

## 2.4 How One Message Is Processed

This is the path every customer message follows.

| Step | Where | What happens |
|---|---|---|
| 1 | Rust gateway | The platform (Facebook, Instagram, WhatsApp, X or LinkedIn) sends the event to the webhook. The gateway checks it with that platform's verification method. Invalid requests are rejected |
| 2 | Rust gateway | The gateway ignores the shop's own messages, removes duplicates by message ID, tags the event with its channel and shop, and applies rate limiting |
| 3 | Rust gateway | It replies "200 OK" to the platform immediately and puts the event in the queue |
| 4 | Python agent | A worker takes the event from the queue. The channel connector converts it to the common message format (F30). The worker then loads the customer's conversation state (current product, order progress, recent messages, summary) |
| 5 | Python agent | If the message is a voice message, image or document, it is converted to text or structured data first (features F05, F06, F07) |
| 6 | Python agent | **Gemini call 1 (Understand):** returns the language, intent, details such as product, size and quantity, sentiment, and a confidence score, as structured data |
| 7 | Python agent | **Code handler:** based on the intent, normal code looks up the database, runs the order steps, calculates charges or searches the knowledge base. This produces a set of verified facts |
| 8 | Python agent | **Gemini call 2 (Reply):** writes a natural, human-like reply in the customer's language using only the verified facts, following the shop's style profile (length, tone, address title and greeting, see F29) |
| 9 | Python agent | **Validator:** checks that every number in the reply (price, stock, charge, total) matches the verified facts. It also checks the greeting, length limit and style rules (F29). If a check fails, it regenerates once, then falls back to a safe template reply |
| 10 | Python agent | The Channel Policy Engine checks that the reply is allowed (reply window, message type, opt-in, budget). The channel connector then sends it through that platform's API (F31) |
| 11 | Python agent | The conversation state is saved, and the message, decision, time taken and AI cost are logged |
| 12 | Background | Follow-ups, notifications, calls and courier actions run as background jobs if needed |

A message uses at most **two Gemini calls**. Everything between them is normal code. This keeps the system accurate, fast and cheap.

## 2.5 Components and Responsibilities

| Component | Language | Responsibility |
|---|---|---|
| Gateway | Rust | Receive webhooks, verify, deduplicate, rate limit, queue |
| Queue and cache | Redis | Hold events, duplicate keys, short-term session data |
| Agent worker | Python | Understand, decide, reply (the pipeline above) |
| Order engine | Python | Controlled order steps, stock reservation, totals |
| Knowledge base | Python and PostgreSQL | Store and search shop policies and FAQs |
| Integration adapters | Python | Courier, payment, voice call, Telegram and email |
| Channel connectors | Python (verification in Rust) | One per platform. Receive events, convert them to the common message format, send replies, and report what the channel supports (F30) |
| Channel Policy Engine | Python | Checks every outgoing message against the platform's rules (F31) |
| Unified inbox | Python (FastAPI) | One inbox for all channels in the dashboard (F32) |
| Background workers | Python | Follow-ups, alerts, confirmation calls, token checks, reports |
| Admin API and panel | Python (FastAPI) | Owner setup, order and chat views, handoff queue |
| Analytics and monitoring | Metabase / Grafana / Prometheus | Dashboards, metrics and alerts |

## 2.6 Technology Stack

| Part | Tool |
|---|---|
| Gateway service | Rust (Axum / Tokio) |
| Agent service | Python (FastAPI) |
| AI model | Google Gemini (text, voice, image and document understanding) |
| Database | PostgreSQL (with vector search for the knowledge base) |
| Queue and cache | Redis |
| Messaging channels | Facebook Messenger and Instagram Messaging (Meta for Developers), WhatsApp Business Platform, X API, LinkedIn APIs |
| Delivery | Courier API (Pathao, Steadfast or RedX) |
| Payment | bKash / Nagad / Cash on Delivery |
| Calling agent | Voice call API with Bangla speech-to-text and text-to-speech (provider to be selected) |
| Notifications | Telegram or email |
| Dashboard | Metabase or Grafana |
| Monitoring and logs | Prometheus, Grafana and structured logs |
| Deployment | Docker on a cloud server with HTTPS |

---

[← Introduction](01_Introduction.md) | [Index](00_README.md) | [Feature Index →](03_Feature_Index.md)
