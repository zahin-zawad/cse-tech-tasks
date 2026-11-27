*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# Phase 4: Sales Intelligence, Automation and Scale

**Goal:** Turn the assistant into a real sales product that increases revenue, and prove it with a pilot.

## Features Included

| ID | Feature | Scope in this phase |
|---|---|---|
| F12 | Payment Handling | COD, bKash and Nagad, advance payment, verification |
| F13 | AI Calling Agent | Order confirmation calls in Bangla |
| F14 | Courier Booking and Tracking | One courier first |
| F16 | Comment-to-Inbox | Full on Facebook (other channels in Phase 5) |
| F17 | Lost-Sale Tracking | Price and delivery charge reasons |
| F18 | Offer and Free-Delivery Win-Back Alerts | Full |
| F19 | Abandoned Order Follow-up | Two reminders |
| F20 | Restock Alerts | Full |
| F26 | Analytics Dashboard | Full |
| F28 | Multi-Shop Support | Full |
| F29 | Personalization | The calling agent uses the greeting, correct title and shop style |
| F38 | Google Sheets Write-Back | Extended: Lost Sales, Unanswered Questions and Daily Summary tabs |
| F41 | Document Generation | Invoices and order slips from Google Docs templates |
| F42 | Google Notifications and Reporting | Gmail and Google Chat notifications, and Looker Studio reports |

In this phase, all features run on Facebook. [Phase 5](24_Phase_5_Multi_Channel_Platform.md) extends them to the other channels.

## Real-World Pilot

Run the system with one or two real small sellers for about two weeks, then measure:

| Metric | Before | After |
|---|---|---|
| Average reply time | [measure] | [measure] |
| Orders captured automatically | [measure] | [measure] |
| Lost customers recovered by win-back alerts | [measure] | [measure] |
| Orders confirmed by calling agent | [measure] | [measure] |
| Failed deliveries or returns | [measure] | [measure] |
| Questions needing a human | [measure] | [measure] |
| Customer satisfaction | [measure] | [measure] |

For the first pilot days, the agent can run in **approval mode**, where the owner approves each reply before it is sent. After accuracy is proven, it switches to automatic replies.

## Deliverables

- Payment, calling agent and courier integrations.
- Sales intelligence and follow-up engine.
- Analytics dashboard.
- Multi-shop configuration.
- Pilot results and final report.

## Done When

- A customer can complete the full journey: comment, chat, order, payment, confirmation call and courier tracking message.
- A lost sale with reason "price" receives an alert when an offer starts, and one with reason "delivery charge" receives an alert when free delivery starts. Customers who did not agree, or said stop, receive nothing.
- Abandoned orders get at most 2 reminders and none during quiet hours.
- The dashboard shows all metrics listed in F26 with real pilot data.
- A second shop can be added by configuration only.
- The confirmation call starts with a greeting, uses the right title (Vaiya, Apu, Sir or Madam) or neutral wording when the title is unknown, and follows the shop's style.
- An invoice PDF is created from the Google Docs template with the correct order data, and a quotation is never sent without the owner's approval.
- The owner receives notifications by Gmail or Google Chat, and a Looker Studio report shows orders and daily results from the sheets.
- Pilot results are collected and compared with the "before" values.

## Evidence to Collect

Courier booking, confirmation call log, follow-up and win-back messages, dashboard, pilot results.

> [ Insert screenshots here ]

---

[← Phase 3: Production-Grade Engineering](22_Phase_3_Production_Engineering.md) | [Index](00_README.md) | [Phase 5: Multi-Channel Platform →](24_Phase_5_Multi_Channel_Platform.md)
