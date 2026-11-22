*AI Sales and Order Assistant for Facebook Businesses · Project Documentation*

# 2. System Overview

## 2.1 Why Python and Rust

The system is built as a combination of two languages, each used where it is strongest.

| Language | Role | Reason |
|---|---|---|
| **Rust** | Webhook gateway and core services | Fast, memory-safe and reliable under heavy load. Suitable for verifying signatures, removing duplicate events, rate limiting and queueing |
| **Python** | AI agent and business logic | Strong ecosystem for AI, speech, vision, data processing and integrations. Suitable for Gemini, order logic, calling agent and analytics |

## 2.2 High-Level Architecture

```
Customer (Messenger / Page comment / Phone call)
                    ↓
        Facebook Webhook (HTTPS)
                    ↓
┌───────────────────────────────────────────┐
│  RUST: Gateway Service                    │
│  • Verify webhook signature               │
│  • Remove duplicate events                │
│  • Rate limiting                          │
│  • Put events in the queue                │
└───────────────────────────────────────────┘
                    ↓  (queue)
┌───────────────────────────────────────────┐
│  PYTHON: AI Agent Service                 │
│  • Language and intent detection          │
│  • Voice / image / document processing    │
│  • Conversation memory                    │
│  • Gemini AI + knowledge base (RAG)       │
│  • Guardrails                             │
│  • Order logic, delivery charge           │
│  • Sales intelligence                     │
└───────────────────────────────────────────┘
        ↓                ↓                ↓
   Database         Integrations     Background Workers
 (PostgreSQL)     (Courier, Payment,  (Follow-ups, alerts,
                   Voice call API,     call confirmation,
                   Telegram/Email)     token renewal)
                    ↓
     Reply to customer on Messenger
                    ↓
 Human handoff  +  Owner notification  +  Analytics dashboard
                    ↓
        Logging and monitoring
```

## 2.3 Technology Stack

| Part | Tool |
|---|---|
| Gateway service | Rust (Axum / Tokio) |
| AI agent service | Python (FastAPI) |
| AI model | Google Gemini (text, voice, image and document understanding) |
| Database | PostgreSQL (with vector search for the knowledge base) |
| Queue and cache | Redis |
| Background jobs | Python workers (scheduled and queued tasks) |
| Messaging | Facebook Messenger Platform, Meta for Developers |
| Delivery | Courier API (Pathao, Steadfast or RedX) |
| Payment | bKash / Nagad / Cash on Delivery |
| Calling agent | Voice call API with speech-to-text and text-to-speech (provider to be selected) |
| Notifications | Telegram or email |
| Dashboard | Metabase or Grafana |
| Monitoring and logs | Prometheus, Grafana and structured logs |
| Deployment | Docker on a cloud server with HTTPS |

---

[← Introduction](01_Introduction.md) | [Index](00_README.md) | [Phase 1: Core Agent →](03_Phase_1_Core_Agent.md)
