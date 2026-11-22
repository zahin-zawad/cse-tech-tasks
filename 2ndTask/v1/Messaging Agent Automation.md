# AI Sales and Order Assistant for Facebook Businesses

**Project Documentation (Planning Stage)**

| | |
|---|---|
| **Organization** | CSE TECH, Daffodil International University |
| **Supervisor** | Md Shoaib Khan |
| **Submitted by** | Zahin Zawad |
| **Student ID** | 232-15-886 |
| **Date** | 21/11/26 |
| **Technology** | Python and Rust |

---

## 1. Introduction

### 1.1 Problem Statement

Many small businesses in Bangladesh sell through Facebook Pages and Messenger, a model commonly called F-commerce. Page owners face the same challenges every day:

- Customers ask the same questions repeatedly, such as price, stock, delivery and payment.
- Replies are slow or missing outside working hours, so customers move to other sellers.
- Orders are collected manually in chats and notebooks, which leads to mistakes.
- Customers who leave because of a high price or delivery charge are never followed up.
- Owners have no clear data about what customers ask, what sells, and why sales are lost.

### 1.2 Proposed Solution

This project proposes an AI-powered sales and order assistant for Facebook Pages. It acts as a digital sales representative that works around the clock and uses the shop's real data. The system will provide the following capabilities.

**Understanding customers**
- Understands messages in **Bangla, English and Banglish**.
- Interprets **voice messages, images and documents** sent by customers.

**Answering with real data**
- Answers questions about **price, stock and shop policies** using the shop's own database, never AI guesswork.

**Handling the full order process**
- Takes complete orders and **calculates the delivery charge**.
- **Books delivery** through a courier service and **confirms payment**.
- **Confirms the order by phone call** using an AI calling agent.
- **Notifies the shop owner** by email or Telegram.

**Engagement and growth**
- **Comment-to-inbox automation:** when someone comments "price?" on a post, the agent replies to the comment and moves the conversation to Messenger.
- **Proactive follow-ups** for abandoned orders and restock alerts, within platform rules.
- **Sales intelligence:** the agent learns why a customer did not buy and brings them back at the right time.
  - If the customer left because of a **high product price**, the agent alerts them when an **offer or discount** becomes available.
  - If the customer left because of a **high delivery charge**, the agent alerts them when **free delivery** is available.

**Safety and visibility**
- **Hands the chat to a human** whenever the situation is unclear or something goes wrong.
- Gives the owner **analytics** about sales and customer questions.
- **Logs everything** and tracks response time, AI cost per conversation and failure rate.

### 1.3 Goals

| Goal | Description |
|---|---|
| Fast replies | Reply to customers within seconds, 24/7 |
| Accurate data | Prices and stock always come from the database, never from AI guessing |
| More orders | Capture orders automatically from chats, calls and comments |
| Recover lost sales | Bring back customers who left because of price or delivery charge |
| Reliability | Handle errors, duplicates and failures safely |
| Real impact | Test with real sellers and measure the results |

---

## 2. System Overview

### 2.1 Why Python and Rust

The system is built as a combination of two languages, each used where it is strongest.

| Language | Role | Reason |
|---|---|---|
| **Rust** | Webhook gateway and core services | Fast, memory-safe and reliable under heavy load. Suitable for verifying signatures, removing duplicate events, rate limiting and queueing |
| **Python** | AI agent and business logic | Strong ecosystem for AI, speech, vision, data processing and integrations. Suitable for Gemini, order logic, calling agent and analytics |

### 2.2 High-Level Architecture

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

### 2.3 Technology Stack

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

### 2.4 Main Database Tables

| Table | Main Fields |
|---|---|
| products | id, name, variants (size/color), price, stock |
| offers | id, product, discount, free delivery flag, start/end date |
| customers | id, Facebook ID, name, phone, address |
| orders | id, customer, items, total, delivery charge, status, payment status |
| conversations | id, customer, messages, state, handoff flag |
| lost_sales | id, customer, product, reason (price / delivery charge / other), notified flag |
| follow_ups | id, customer, type (abandoned order / restock / offer), scheduled time, status |
| call_logs | id, order, call status, customer response, timestamp |
| faq | question, answer (shop policies) |
| usage_logs | id, conversation, response time, AI cost, errors |

---

## 3. Project Phases

The project is divided into **four phases**. Each phase produces a working system, so progress is safe even if time runs short.

---

### Phase 1: Core Agent that Does Real Work

**Goal:** Build the foundation: a complete text-based sales assistant with order taking.

**Features**
- Rust gateway that receives Facebook messages and passes them to the Python agent.
- Intent detection with Gemini (price, stock, order, delivery, other).
- Live **stock and variant** checking (for example, "Is the black one in size M available?").
- **Order taking:** collect product, size or color, quantity, name, phone and address, then confirm and save the order.
- **Delivery charge** calculated by the customer's area.
- **Owner notification** by email or Telegram for every new order.
- Human-like replies written by Gemini from database facts.

**Example**

> **Customer:** "Black polo size M ache? dam koto?"
> **Agent:** "Yes, the black polo in size M is available. The price is 850 taka. Would you like to order it?"

**Deliverables**
- Working Rust gateway and Python agent service.
- Products, customers and orders tables in PostgreSQL.
- Owner notification.

**Evidence:** system architecture running, sample chat, order saved in the database.

> [ Insert screenshots here ]

---

### Phase 2: Multimodal and Smarter AI

**Goal:** Understand customers the way they really write, speak and share information.

**Features**
- **Bangla, English and Banglish** support (for example, "dam koto?" and "price ki?").
- **Voice messages:** transcribe and understand, then reply.
- **Images:** match a product photo sent by the customer to the closest catalog item.
- **Documents:** read files and screenshots sent by customers, such as payment receipts or product details.
- **Knowledge base (RAG):** answer return policy, delivery time, warranty and shop FAQs from the shop's own information.
- **Conversation memory:** remember earlier messages in the same chat.
- **Guardrails:**
  - Prices and stock come **only** from the database.
  - Protection against prompt injection and fake discount tricks.
  - The agent never promises anything outside shop policy.

**Deliverables**
- Multi-language prompts and tests.
- Voice, image and document processing modules.
- FAQ knowledge base.
- A test set of **50 to 100 realistic messages** (including misspellings and Banglish) with measured accuracy.

**Evidence:** Banglish chat, voice message reply, image match, accuracy results.

> [ Insert screenshots here ]

---

### Phase 3: Production-Grade Engineering

**Goal:** Make the system safe, reliable and ready for real customers.

| Area | What will be done |
|---|---|
| Security | Verify Facebook webhook signatures in the Rust gateway; keep tokens and secrets in environment variables or a secrets manager |
| Reliability | Retry logic and an error workflow with alerts, so failures are never silent |
| Duplicates | Detect duplicate message events (Facebook can send the same event twice) |
| Human handoff | If the customer is angry, confused or asks something unusual, or if something goes wrong, the agent stops and notifies a human with the full conversation |
| Platform rules | Follow Messenger messaging policies, including the standard 24-hour reply window |
| Logging | Log everything and track **response time, AI cost per conversation and failure rate** |
| Deployment | Docker, HTTPS, automatic restart and regular backups |
| Token handling | Renew Facebook access tokens before they expire |
| Testing | Automated tests for the gateway, order logic and guardrails |

**Deliverables**
- System deployed on a cloud server.
- Error and handoff workflows.
- Monitoring, logging and backup setup.

**Evidence:** handoff alert, error alert, server deployment, logs and metrics.

> [ Insert screenshots here ]

---

### Phase 4: Sales Intelligence, Automation and Scale

**Goal:** Turn the assistant into a real sales product that increases revenue, and prove it with a pilot.

**Order completion**
- **Courier booking** through a courier API, with a tracking message sent to the customer.
- **Payment flow** (bKash, Nagad or Cash on Delivery) with automatic payment status updates.
- **Calling agent:** an AI voice agent calls the customer to confirm the order, and the result is saved to the order.

**Sales intelligence**
- The agent records **why a customer did not buy** (high price, high delivery charge, or other reason).
- When a matching **offer** is created, the agent alerts customers who left because of price.
- When **free delivery** becomes available, the agent alerts customers who left because of the delivery charge.
- **Proactive follow-ups** for abandoned orders and restock alerts, within platform rules and with customer consent.

**Engagement**
- **Comment-to-inbox automation:** when someone comments "price?" on a post, the agent replies and moves the conversation to Messenger.

**Analytics dashboard**
- Top products
- Questions the agent could not answer
- Orders captured automatically
- Lost sales by reason
- Response time, AI cost per conversation and failure rate
- Conversion rate (chats that became orders)

**Scale**
- **Multi-shop design:** many shops can use the same system by changing only their catalog and settings.
- More channels later: Instagram and WhatsApp.

**Real-world pilot**

Run the system for one or two real small sellers for about two weeks, then measure:

| Metric | Before | After |
|---|---|---|
| Average reply time | [measure] | [measure] |
| Orders captured automatically | [measure] | [measure] |
| Lost customers recovered by follow-up | [measure] | [measure] |
| Questions needing a human | [measure] | [measure] |
| Customer satisfaction | [measure] | [measure] |

**Deliverables**
- Courier, payment and calling agent integration.
- Sales intelligence and follow-up engine.
- Analytics dashboard.
- Pilot results and final report.

**Evidence:** courier booking, confirmation call log, follow-up message, dashboard, pilot results.

> [ Insert screenshots here ]

---

## 4. Timeline

| Phase | Focus | Estimated Time |
|---|---|---|
| Phase 1 | Core agent and order taking | [x weeks] |
| Phase 2 | Multimodal and smarter AI | [x weeks] |
| Phase 3 | Production-grade engineering | [x weeks] |
| Phase 4 | Sales intelligence, integrations and pilot | [x weeks] |

*(Time to be decided with the supervisor.)*

---

## 5. Success Measures

| Area | Target |
|---|---|
| Response time | Under 10 seconds |
| Intent understanding accuracy | Measured on the 50 to 100 message test set |
| Price and stock errors | Zero (data always from the database) |
| Handoff | All unclear or angry chats passed to a human |
| Order confirmation | Share of orders confirmed by the calling agent |
| Recovered sales | Customers who bought after a price or free-delivery alert |
| System reliability | Failures detected and alerted |
| Business impact | Faster replies and more orders in the pilot |

---

## 6. Risks and Solutions

| Risk | Solution |
|---|---|
| AI gives a wrong price | Prices only from the database; AI never decides numbers |
| Unusual Bangla or Banglish | Test set with real examples; improve prompts |
| Customer tries to trick the AI | Guardrails and strict rules in the prompt |
| Voice or image misunderstood | Ask the customer to confirm before placing the order |
| Facebook token expires or webhook fails | Token renewal, retries and error alerts |
| Duplicate messages create duplicate orders | Duplicate event detection in the Rust gateway |
| Agent cannot handle a request | Hand over to a human |
| Follow-up messages break platform rules | Send only within Meta's messaging policies and with customer consent |
| Calling customers without permission | Call only customers who gave their phone number for the order and agreed to confirmation |
| AI cost grows | Track cost per conversation and set usage limits |
| Customer data privacy | Store only required data, secure the database, and restrict access |

---

## 7. Expected Impact

- Small sellers can answer customers 24/7 without hiring extra staff.
- Fewer lost customers and more orders, including recovered sales.
- Fewer mistakes in order taking, because orders are confirmed by chat and by phone.
- Owners save hours of repeated work and get clear data about their business.
- The same system can be reused for many businesses.

---

## 8. Conclusion

This project goes far beyond a simple price-reply bot. It is a complete sales and order assistant that understands Bangla, voice, images and documents, takes and confirms orders, books delivery, brings back lost customers with offers and free delivery alerts, and hands over to humans when needed. By combining a fast and secure Rust gateway with a flexible Python AI service, the system is designed to be reliable enough for real businesses. Building it in four phases gives a working result at every stage and moves the project step by step toward a real product, which will be proven through a pilot with real sellers.

I thank my supervisor, Md Shoaib Khan, for his guidance.