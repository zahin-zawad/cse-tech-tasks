*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# 14. Channel: WhatsApp

**Role:** Private, trusted chat for direct sales, order confirmations and order updates. It is also the best channel for notifications, because customers expect messages there.

**Build phase:** Phase 5B.

## 14.1 What the System Does on WhatsApp

| Capability | Support | Notes |
|---|---|---|
| Customer messages | Yes | Text, voice notes, images, documents and location |
| Reply buttons and lists | Yes | Confirm / Edit / Cancel and product choices |
| Comments | Not applicable | WhatsApp has no public comments. Customers arrive through click-to-chat links, QR codes, website buttons and click-to-WhatsApp ads |
| Customer phone number | Known | Already known from the chat. Only confirmed for the order |
| Full order process | Yes | F10 to F15 |
| Order updates outside the window | Yes, with templates | Order confirmation and tracking use approved utility templates |
| Follow-ups and win-back alerts | Yes, with templates and opt-in | Must follow the template and opt-in rules |
| Human handoff | Yes | Dashboard inbox. The platform's policy requires a clear escalation path to a human |

## 14.2 Setup Requirements

- Access to the **WhatsApp Business Platform** (Cloud API) through Meta, with the shop's business phone number registered on it.
- Meta Business verification, so the number can send at higher limits.
- A set of **message templates**, approved by Meta, before they can be used. Approval can take from hours to a day or more, so templates are submitted early.
- A webhook subscribed to messages and message status updates.
- Written **opt-in** wording, so customers agree to receive business-initiated messages.

## 14.3 How Messages Flow

**Incoming:** The event goes to the Rust gateway, which checks the Meta signature header. The WhatsApp connector converts it. A customer message opens or restarts the 24-hour window.

**Outgoing:** The Policy Engine (F31) chooses:
- **Inside the window:** a normal (non-template) message.
- **Outside the window:** only an approved **template** is possible, and the customer must have agreed.

## 14.4 Rules and Limits

- **Reply window:** 24 hours, restarting with each customer message. Normal messages can only be sent inside it.
- **Starting a chat:** only with an approved template.
- **Automation is allowed**, but there must be a clear and quick way to reach a human (in-chat transfer, phone number or email). F22 provides this.
- **Purpose-built AI only.** Since 15 January 2026, WhatsApp's terms do not allow general-purpose AI assistants on the platform. A shop assistant for customer service and sales is allowed. The agent must **stay on shop topics** (F21 "scope control") and must not become an open-ended chatbot.
- **Templates and categories:**

| Template category | Used for | Example |
|---|---|---|
| Utility | Order updates | "Your order #1042 is confirmed. Tracking ID: ..." |
| Marketing | Offers and win-back | "The Black Polo is now 700 taka. Would you like to order?" |
| Authentication | Not used | Not used in this project |

- Customers can block or report the number. Poor quality lowers the sending limit, so messages must be useful and wanted.
- The agent never shares one customer's chat with another customer.

## 14.5 Costs

- WhatsApp charges **per message**, by category and country.
- Pricing changed in July 2025 (template messages charged per delivered message) and again from 1 October 2026 (service messages charged per message after a free monthly amount, and utility replies inside the window charged).
- Rates for Bangladesh must be loaded from Meta's current rate card (F35).
- Chats that start from a click-to-WhatsApp ad have an extended free window (72 hours at the time of writing).
- The cost of each conversation and each order on WhatsApp is shown in the dashboard.

## 14.6 Special Behaviour

- **Call confirmation (F13):** the number is known, so the agent asks the customer to confirm that the call may be made to this number.
- **Notification channel:** the owner can choose WhatsApp templates for order and tracking updates even to customers who first ordered on another channel, **only** if the customer gave consent on WhatsApp (F33).

## 14.7 Risks

| Risk | Handling |
|---|---|
| Template rejected or slow to approve | Submit early, write simple and clear templates |
| Messages cost more than expected | Budgets and alerts per channel (F35), proactive messages paused first |
| Number quality drops | Send only wanted messages, respect opt-out |
| Agent drifts into general chat | Scope control and guardrails (F21) |

**Done when:** a customer orders on WhatsApp, receives an order update by template after the window closes, a human takes over from the dashboard, and the cost per order is shown.

---

[← Channel: Instagram](13_Channel_Instagram.md) | [Index](00_README.md) | [Channel: X →](15_Channel_X.md)
