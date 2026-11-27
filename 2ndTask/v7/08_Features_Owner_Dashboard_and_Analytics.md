*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# 8. Features: Owner Dashboard and Analytics

These features let the owner control the system and see its results.

---

## F25 Shop Setup and Owner Controls (Phase 1 simple setup, Phase 3 full panel)

**Purpose:** Let the owner set up and manage the shop without a developer.

**Phase 1 (simple):** Setup is done with a spreadsheet import (CSV or Excel) and a configuration file prepared with the owner.
**Phase 3 (full panel):** A login-protected web panel.

**What the owner can set up**

| Setting | Details |
|---|---|
| Channel connections | Connect Facebook, Instagram, WhatsApp, X and LinkedIn from one place, choose which channels the agent answers on, and check each connection's status (F34) |
| Google connection | Connect Google, choose the Sheets, Docs and Drive folders, map columns, and see the sync status (F36 to F40) |
| Products | Name, other names (Bangla, English, Banglish), category, price, images, description, sizes and colors with stock. Add one by one or import from a file |
| Delivery rules | Zones, charges and free-delivery conditions |
| Payment methods | COD, bKash and Nagad numbers, advance payment rules |
| Offers and campaigns | Price offers and free-delivery campaigns with dates and minimum discount for alerts (F18) |
| Policies and FAQs | Text or documents for the knowledge base (F08) |
| Post-to-product links | Link a Facebook post to a product for comment-to-inbox (F16) |
| Business hours | Working hours, handoff messages, calling hours |
| Agent style and personalization | Style preset (casual and short, balanced, professional and detailed), reply length, emoji use, greeting phrase, address style (Vaiya / Apu, Sir / Madam, or name only), sample replies, words to use or avoid, call voice, and how Banglish is answered (see F29) |
| Rules and limits | Maximum discount, call rule, order approval mode, high-value order limit |
| Notifications | Telegram or email, and which events to receive |
| Staff | Add or remove staff with roles |

**Daily work from the panel**
- See all orders with status (new, confirmed, payment pending, shipped, delivered, cancelled, returned) and filter them.
- Approve or cancel orders, and verify payments.
- Open any conversation and see the handoff queue.
- Update stock, which also triggers restock alerts (F20).

**Rules:** Every change to prices, offers and rules is recorded in the audit log (F24).

---

## F26 Analytics Dashboard (Phase 4)

**Purpose:** Show the owner what customers ask, what sells and where sales are lost.

**How it works:** Data from orders, conversations and logs is shown on a dashboard (Metabase or Grafana) that refreshes automatically.

| Metric | Meaning |
|---|---|
| Top products | Most asked about and most ordered |
| Orders captured automatically | Orders completed by the agent without human work |
| Conversion rate | Orders divided by conversations that showed product interest |
| Lost sales by reason | Price, delivery charge, out of stock or other |
| Win-back results | Alerts sent, replies and orders from alerts |
| Unanswered questions | Questions the agent could not answer, with a note to add them to the knowledge base |
| Response time | Average and slowest time to reply |
| Handoffs | Number and reasons |
| Channel breakdown | Conversations, orders, response time and conversion for each channel |
| Cross-channel customers | Customers who contacted the shop on more than one channel |
| Call confirmations | Confirmed, cancelled, no answer |
| Order outcomes | Delivered, returned, cancelled |

The owner also receives a **daily summary** by Telegram or email (F15). Reports can also be viewed in Looker Studio from the Daily Summary sheet (F42).

---

## F27 Logging, Cost Tracking and Monitoring (Phase 3)

**Purpose:** Know how the system performs, what it costs, and when it fails.

**What is logged for every message**
- Time received, time replied and total response time.
- Intent, language and confidence.
- Which handler ran and what facts were used.
- Number of AI calls, AI tokens used and **AI cost** of the conversation.
- Errors and retries.

**What is tracked**

| Metric | Use |
|---|---|
| Response time | Check that replies stay fast |
| AI cost per conversation | Keep running costs under control |
| Cost per channel | AI cost plus any platform charges (for example WhatsApp messages and X API credits) for each channel (F35) |
| Failure rate | Detect problems early |
| Queue size and waiting time | Detect overload |
| Handoff rate | Measure how often the agent needs help |

**Alerts** are sent to the administrator when failure rate, delay or queue size passes a limit, when a service stops, or when the AI cost per day passes a set limit.

**Rules:** Logs do not contain secrets, and personal data in logs is masked.

---

## F28 Multi-Shop Support (Phase 4)

**Purpose:** Let many shops use the same system, which turns the project into a product.

**How it works**
1. Each shop has its own channel connections (Facebook Page, Instagram account, WhatsApp number, X account and LinkedIn Page), catalog, rules, policies, offers and owner account. Each shop also connects its own Google files (F36).
2. Incoming events are matched to the right shop by the channel account ID (Page ID, Instagram account ID, WhatsApp number ID, X account ID or LinkedIn Page ID).
3. Shops cannot see each other's data.
4. Settings, prompts and limits are loaded per shop at the start of each message.
5. Costs and metrics are tracked per shop.

**Rules:** Adding a new shop needs only configuration and catalog data, not code changes.

---

[← Features: Safety, Handoff and Reliability](07_Features_Safety_Handoff_and_Reliability.md) | [Index](00_README.md) | [Features: Personalization →](09_Features_Personalization.md)
