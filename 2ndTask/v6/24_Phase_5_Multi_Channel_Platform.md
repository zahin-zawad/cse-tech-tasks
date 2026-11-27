*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# Phase 5: Multi-Channel Platform

**Goal:** Extend the system from Facebook to Instagram, WhatsApp, X and LinkedIn, all controlled from **one platform and one dashboard**.

Because the core is built channel-independent from Phase 1 (F30, F31), adding a channel means building a connector and its rules. The agent logic is not rewritten.

## Features Included

| ID | Feature | Scope in this phase |
|---|---|---|
| F30 | Channel Connector Layer | Connectors for Instagram, WhatsApp, X and LinkedIn |
| F31 | Channel Policy Engine | Rules for all five channels |
| F32 | Unified Inbox and Single Dashboard | All channels in one inbox, assignment, modes, assist mode |
| F33 | Cross-Channel Customer Identity | Linking with strong evidence or customer confirmation |
| F34 | Channel Connection and Health | Connection wizard and health status for every channel |
| F35 | Channel Cost and Quota Control | Budgets, alerts and cost per channel |
| F36 to F42 | Google Integration | Each shop's own Google connection, a Leads sheet, and quotations for business enquiries from any channel |
| F01 to F29 | All earlier features | Extended to every channel as each one is added |

## Build Order

| Step | Channel | Why in this order |
|---|---|---|
| 5A | Instagram | Uses the same Meta platform, signature check and Page link as Facebook. The fastest to add |
| 5B | WhatsApp | High value for orders and order updates. Needs templates, opt-in and cost tracking |
| 5C | X | Needs credits and spending control. Mostly public support and direct messages |
| 5D | LinkedIn | Depends on LinkedIn's approval. Level A first, Level B only if approved, assist mode as the fallback |
| 5E | One platform | Cross-channel identity, unified analytics, cost dashboard and the extended pilot |

Applications for each platform's approvals are submitted **at the start of Phase 5** (or earlier), because approval time is outside our control.

## Deliverables

- Instagram, WhatsApp, X and LinkedIn connectors.
- The Policy Engine rules and tests for every channel.
- Unified inbox with filters, assignment, modes (Auto, Approval, Human) and assist mode.
- Customer identity linking with consent-per-channel rules.
- Connection wizard and health monitor for all channels.
- Budget, alerts and cost reports per channel.
- Updated analytics with a channel breakdown.
- Extended pilot results.

## Done When

**For every channel**
- A customer message is received through the connector, answered by the same agent, and logged with its channel.
- The connector passes the common test set: send, receive, duplicate event, rate limit and failure.
- A message that the platform's rules do not allow is blocked by the Policy Engine.
- A human can take over from the unified inbox and reply through the right channel.
- The channel's health status and token expiry are visible in the dashboard.

**Per channel**
- **Instagram:** a comment becomes one private reply, and an order is completed in Instagram DM.
- **WhatsApp:** an order is completed, an order update is sent by approved template after the window closes, and the cost per order is shown.
- **X:** a direct message becomes an order, a public reply moves the customer to direct messages, and the spending limit works.
- **LinkedIn:** comments and mentions are answered with correct data, a bulk enquiry becomes a lead, and assist mode produces a correct draft. Messaging is tested only if LinkedIn approves it.

**One platform**
- The same customer on two channels is linked after confirmation or a matching verified phone number, and is never linked by name alone.
- Consent given on one channel does not allow messages on another.
- Turning off one channel, or losing its token, does not affect the others.
- The dashboard shows conversations, orders, response time, conversion and cost for each channel.
- One owner login controls all channels.

## Extended Pilot

Run the pilot with Facebook and at least **one more channel** (WhatsApp or Instagram recommended). Measure:

| Metric | Facebook | Second channel | Total |
|---|---|---|---|
| Conversations | [measure] | [measure] | [measure] |
| Average reply time | [measure] | [measure] | [measure] |
| Orders captured automatically | [measure] | [measure] | [measure] |
| Questions needing a human | [measure] | [measure] | [measure] |
| Cost per order (AI and platform) | [measure] | [measure] | [measure] |
| Customers on more than one channel | | | [measure] |

## Evidence to Collect

Each channel connected, the same question answered on several channels, the unified inbox, a channel health screen, a blocked message with the reason, a WhatsApp template message, the cost dashboard, and pilot results.

> [ Insert screenshots here ]

---

[← Phase 4: Sales Intelligence, Automation and Scale](23_Phase_4_Sales_Intelligence_Scale.md) | [Index](00_README.md) | [Timeline, Success Measures, Risks and Conclusion →](25_Timeline_Success_Risks_Conclusion.md)
