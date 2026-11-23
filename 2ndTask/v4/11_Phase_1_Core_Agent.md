*AI Sales and Order Assistant for Facebook Businesses · Project Documentation v3.1*

# Phase 1: Core Agent that Does Real Work

**Goal:** Build the foundation: a working text-based sales assistant that takes orders from message to saved order.

## Features Included

| ID | Feature | Scope in this phase |
|---|---|---|
| F01 | Message Gateway | Full |
| F02 | Language Understanding | Basic Bangla, English and Banglish through Gemini |
| F03 | Intent and Detail Extraction | Full |
| F09 | Product, Price and Stock Answers | Full |
| F10 | Order Taking | Full, with Cash on Delivery only |
| F11 | Delivery Charge Calculation | Zone-based and free-delivery threshold |
| F15 | Owner Notifications | New order and system failure alerts |
| F25 | Shop Setup | Simple: spreadsheet import and configuration file |
| F29 | Personalization | Style presets (casual and short, balanced, professional and detailed) and the greeting rule, set in the configuration file |

Prices and stock come only from the database from the first day.

## Not in This Phase

Voice, images, documents, knowledge base, payment methods other than COD, calling agent, courier booking, follow-ups, and the owner panel.

## Deliverables

- Rust gateway and Python agent service running together.
- Product, customer and order data stored in PostgreSQL.
- Complete flow from the first message to a saved order.
- Owner notification for every new order.

## Done When

- A test customer can ask about price and stock, place an order with the Confirm button, and the order appears in the database.
- 20 out of 20 test price and stock questions are answered with the correct database values.
- Duplicate Facebook events never create duplicate orders.
- The owner receives a notification within 10 seconds of a confirmed order.
- Invalid phone numbers and incomplete addresses are rejected and asked again.
- Every new chat starts with a greeting, and replies follow the chosen style preset (short and casual, or long and professional).

## Evidence to Collect

System running, sample chat, order saved in the database, owner notification.

> [ Insert screenshots here ]

---

[← End-to-End Scenarios](10_End_to_End_Scenarios.md) | [Index](00_README.md) | [Phase 2: Multimodal and Smarter AI →](12_Phase_2_Multimodal_AI.md)
