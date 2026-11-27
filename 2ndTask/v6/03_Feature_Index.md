*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# 3. Feature Index

This is the complete list of features. Each feature has an ID (F01, F02 and so on) and is described in detail in documents 04 to 18.

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
| F30 | Channel Connector Layer | Multi-Channel | 1 and 5 | [10](10_Features_Multi_Channel_Platform.md) |
| F31 | Channel Policy Engine | Multi-Channel | 1 and 5 | [10](10_Features_Multi_Channel_Platform.md) |
| F32 | Unified Inbox and Single Dashboard | Multi-Channel | 3 and 5 | [10](10_Features_Multi_Channel_Platform.md) |
| F33 | Cross-Channel Customer Identity | Multi-Channel | 5 | [10](10_Features_Multi_Channel_Platform.md) |
| F34 | Channel Connection and Health | Multi-Channel | 3 and 5 | [10](10_Features_Multi_Channel_Platform.md) |
| F35 | Channel Cost and Quota Control | Multi-Channel | 5 | [10](10_Features_Multi_Channel_Platform.md) |
| F36 | Google Connection and Access | Google | 1 and 3 | [17](17_Features_Google_Integration.md) |
| F37 | Google Sheets Catalog and Business Data Sync | Google | 1 | [17](17_Features_Google_Integration.md) |
| F38 | Google Sheets Write-Back (orders, leads, reports) | Google | 1 and 4 | [17](17_Features_Google_Integration.md) |
| F39 | Google Docs Knowledge Base and Style Guide Sync | Google | 2 | [17](17_Features_Google_Integration.md) |
| F40 | Google Drive Files (images, catalogs, price lists) | Google | 2 | [17](17_Features_Google_Integration.md) |
| F41 | Document Generation (invoices and quotations) | Google | 4 | [17](17_Features_Google_Integration.md) |
| F42 | Google Notifications and Reporting | Google | 4 | [17](17_Features_Google_Integration.md) |

**Note:** Some basics are built early so the system works end to end. For example, prices and stock come only from the database from Phase 1, and Phase 1 supports Cash on Delivery only. Later phases add the full versions. The core is built independent of any channel from Phase 1, with Facebook as the first channel. Instagram, WhatsApp, X and LinkedIn are added in Phase 5, and features F01 to F29 work on every channel through the connector layer (F30). Each channel is described in documents 11 to 16. The Google integration is described in documents 17 and 18.

---

[← System Overview](02_System_Overview.md) | [Index](00_README.md) | [Features: Conversation and Understanding →](04_Features_Conversation_and_Understanding.md)
