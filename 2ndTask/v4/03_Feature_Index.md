*AI Sales and Order Assistant for Facebook Businesses · Project Documentation v3.1*

# 3. Feature Index

This is the complete list of features. Each feature has an ID (F01, F02 and so on) and is described in detail in documents 04 to 09.

**Every feature description answers the same questions:**
- **Purpose:** why the feature exists
- **Trigger:** what starts it
- **How it works:** the exact steps
- **Rules:** limits and decisions
- **Example:** a real-looking case
- **If something goes wrong:** how failures are handled

| ID | Feature | Group | Phase | Detailed in |
|---|---|---|---|---|
| F01 | Message Gateway | Conversation | 1 | [04](04_Features_Conversation_and_Understanding.md) |
| F02 | Language Understanding (Bangla, English, Banglish) | Conversation | 1 and 2 | [04](04_Features_Conversation_and_Understanding.md) |
| F03 | Intent and Detail Extraction | Conversation | 1 | [04](04_Features_Conversation_and_Understanding.md) |
| F04 | Conversation Memory and Returning Customers | Conversation | 2 | [04](04_Features_Conversation_and_Understanding.md) |
| F05 | Voice Message Understanding | Conversation | 2 | [04](04_Features_Conversation_and_Understanding.md) |
| F06 | Image Understanding | Conversation | 2 | [04](04_Features_Conversation_and_Understanding.md) |
| F07 | Document and Screenshot Understanding | Conversation | 2 | [04](04_Features_Conversation_and_Understanding.md) |
| F08 | Shop Knowledge Base (policies and FAQs) | Conversation | 2 | [04](04_Features_Conversation_and_Understanding.md) |
| F09 | Product, Price and Stock Answers | Conversation | 1 | [04](04_Features_Conversation_and_Understanding.md) |
| F10 | Order Taking | Orders | 1 | [05](05_Features_Orders_and_Fulfilment.md) |
| F11 | Delivery Charge Calculation | Orders | 1 | [05](05_Features_Orders_and_Fulfilment.md) |
| F12 | Payment Handling (COD, bKash, Nagad) | Orders | 4 | [05](05_Features_Orders_and_Fulfilment.md) |
| F13 | AI Calling Agent (order confirmation call) | Orders | 4 | [05](05_Features_Orders_and_Fulfilment.md) |
| F14 | Courier Booking and Tracking | Orders | 4 | [05](05_Features_Orders_and_Fulfilment.md) |
| F15 | Owner Notifications | Orders | 1 | [05](05_Features_Orders_and_Fulfilment.md) |
| F16 | Comment-to-Inbox | Sales and Engagement | 4 | [06](06_Features_Sales_Intelligence_and_Engagement.md) |
| F17 | Lost-Sale Tracking | Sales and Engagement | 4 | [06](06_Features_Sales_Intelligence_and_Engagement.md) |
| F18 | Offer and Free-Delivery Win-Back Alerts | Sales and Engagement | 4 | [06](06_Features_Sales_Intelligence_and_Engagement.md) |
| F19 | Abandoned Order Follow-up | Sales and Engagement | 4 | [06](06_Features_Sales_Intelligence_and_Engagement.md) |
| F20 | Restock Alerts | Sales and Engagement | 4 | [06](06_Features_Sales_Intelligence_and_Engagement.md) |
| F21 | Guardrails | Safety | 2 | [07](07_Features_Safety_Handoff_and_Reliability.md) |
| F22 | Human Handoff | Safety | 3 | [07](07_Features_Safety_Handoff_and_Reliability.md) |
| F23 | Reliability (duplicates, retries, fallback) | Safety | 3 | [07](07_Features_Safety_Handoff_and_Reliability.md) |
| F24 | Security and Privacy | Safety | 3 | [07](07_Features_Safety_Handoff_and_Reliability.md) |
| F25 | Shop Setup and Owner Controls | Owner | 1 and 3 | [08](08_Features_Owner_Dashboard_and_Analytics.md) |
| F26 | Analytics Dashboard | Owner | 4 | [08](08_Features_Owner_Dashboard_and_Analytics.md) |
| F27 | Logging, Cost Tracking and Monitoring | Owner | 3 | [08](08_Features_Owner_Dashboard_and_Analytics.md) |
| F28 | Multi-Shop Support | Owner | 4 | [08](08_Features_Owner_Dashboard_and_Analytics.md) |
| F29 | Personalization (reply style, address titles, greetings) | Personalization | 1, 2 and 4 | [09](09_Features_Personalization.md) |

**Note:** Some basics are built early so the system works end to end. For example, prices and stock come only from the database from Phase 1, and Phase 1 supports Cash on Delivery only. Later phases add the full versions.

---

[← System Overview](02_System_Overview.md) | [Index](00_README.md) | [Features: Conversation and Understanding →](04_Features_Conversation_and_Understanding.md)
