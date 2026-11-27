# AI Sales and Order Assistant for Social Media Businesses

**Project Documentation, Version 5 (Planning Stage)**

| | |
|---|---|
| **Organization** | CSE TECH, Daffodil International University |
| **Supervisor** | Md Shoaib Khan |
| **Submitted by** | Zahin Zawad |
| **Student ID** | 232-15-886 |
| **Date** | [DD Month YYYY] |
| **Version** | 5 |
| **Technology** | Python and Rust |

---

## About This Project

An AI-powered sales and order assistant that works on **Facebook, Instagram, WhatsApp, X and LinkedIn** from **one platform and one dashboard**. It works with the files owners already use in **Google Sheets, Docs and Drive**. It understands Bangla, English and Banglish, answers using real shop data, takes and confirms orders, brings back lost customers with offers and free-delivery alerts, hands over to a human when needed, and sounds like each business's own brand.

## What Is New in Version 5

**1. Multi-channel platform** (documents 10 to 16)
- One agent for all channels, through a connector for each platform (F30).
- A Policy Engine that checks every outgoing message against the platform's rules (F31).
- One dashboard and inbox, one customer across channels, a connection wizard with health status, and cost control per channel (F32 to F35).
- A document for each channel: Facebook, Instagram, WhatsApp, X and LinkedIn.

**2. Google integration** (documents 17 and 18)
- **Google Sheets:** products, prices, stock, offers and delivery rules are read from the owner's sheet, checked, and copied to the database. Orders, leads, lost sales and reports are written back (F37, F38).
- **Google Docs:** policies, FAQs, product details and a style guide are read as the agent's knowledge base and brand voice (F39).
- **Google Drive:** product images, and PDF catalogs and price lists with owner review (F40).
- **Invoices and quotations** made from Google Docs templates (F41).
- **Gmail, Google Chat and Looker Studio** for notifications and reports (F42).
- **Safe connection:** by sharing files with the system, or Sign in with Google and a file picker, with the least access needed (F36).
- **A sync engine** keeps the agent fast and safe: it never waits for Google while answering a customer, uses the last good copy if the sheet has errors, and holds a sync if too much changes at once.
- Exact sheet and document layouts for owners (document 18).

Related parts of other documents were updated, and there are new scenarios (6 to 9).

## Earlier Versions

Version 3.1 defined every feature step by step (F01 to F29), including personalization: reply style, address titles (Vaiya, Apu, Sir, Madam) and the greeting rule. Those features remain and now work on every channel and with Google files.

## Contents

| # | File | What it covers |
|---|---|---|
| 1 | [Introduction](01_Introduction.md) | Problem, solution, users, goals, scope |
| 2 | [System Overview](02_System_Overview.md) | Principles, architecture, how one message is processed, components, stack |
| 3 | [Feature Index](03_Feature_Index.md) | The complete list of 42 features with phases |
| 4 | [Conversation and Understanding](04_Features_Conversation_and_Understanding.md) | F01 to F09 |
| 5 | [Orders and Fulfilment](05_Features_Orders_and_Fulfilment.md) | F10 to F15 |
| 6 | [Sales Intelligence and Engagement](06_Features_Sales_Intelligence_and_Engagement.md) | F16 to F20 |
| 7 | [Safety, Handoff and Reliability](07_Features_Safety_Handoff_and_Reliability.md) | F21 to F24 |
| 8 | [Owner Dashboard and Analytics](08_Features_Owner_Dashboard_and_Analytics.md) | F25 to F28 |
| 9 | [Personalization](09_Features_Personalization.md) | F29 |
| 10 | [Multi-Channel Platform](10_Features_Multi_Channel_Platform.md) | F30 to F35 |
| 11 | [Channel Overview and Comparison](11_Channel_Overview_and_Comparison.md) | Side-by-side comparison of all channels |
| 12 | [Channel: Facebook](12_Channel_Facebook.md) | Messenger and Page comments |
| 13 | [Channel: Instagram](13_Channel_Instagram.md) | Direct messages and comment-to-DM |
| 14 | [Channel: WhatsApp](14_Channel_WhatsApp.md) | Chat, templates, costs and rules |
| 15 | [Channel: X](15_Channel_X.md) | Direct messages, public replies, credits |
| 16 | [Channel: LinkedIn](16_Channel_LinkedIn.md) | B2B leads, comments, messaging levels |
| 17 | [Google Integration](17_Features_Google_Integration.md) | F36 to F42: Sheets, Docs, Drive, Gmail, Chat, Looker Studio |
| 18 | [Google Sheet and Doc Templates](18_Google_Sheet_and_Doc_Templates.md) | How the owner's files should look |
| 19 | [End-to-End Scenarios](19_End_to_End_Scenarios.md) | Nine complete examples |
| 20 | [Phase 1: Core Agent](20_Phase_1_Core_Agent.md) | Features, deliverables, done-when |
| 21 | [Phase 2: Multimodal and Smarter AI](21_Phase_2_Multimodal_AI.md) | Features, deliverables, done-when |
| 22 | [Phase 3: Production-Grade Engineering](22_Phase_3_Production_Engineering.md) | Features, deliverables, done-when |
| 23 | [Phase 4: Sales Intelligence and Scale](23_Phase_4_Sales_Intelligence_Scale.md) | Features, deliverables, pilot, done-when |
| 24 | [Phase 5: Multi-Channel Platform](24_Phase_5_Multi_Channel_Platform.md) | New channels, one dashboard, extended pilot |
| 25 | [Timeline, Success, Risks and Conclusion](25_Timeline_Success_Risks_Conclusion.md) | Schedule, targets, risks, impact |

## Phase Summary

| Phase | Focus | Features |
|---|---|---|
| Phase 1 | Core agent and order taking (Facebook first, Google Sheets for data) | F01, F02, F03, F09, F10, F11, F15, F25, F29, F30, F31, F36, F37, F38 |
| Phase 2 | Multimodal and smarter AI, Google Docs and Drive | F02, F04, F05, F06, F07, F08, F21, F29, F39, F40 |
| Phase 3 | Production-grade engineering | F22, F23, F24, F25, F27, F32, F34, F36 |
| Phase 4 | Sales intelligence, integrations and pilot (Facebook) | F12, F13, F14, F16 to F20, F26, F28, F29, F38, F41, F42 |
| Phase 5 | Multi-channel platform: Instagram, WhatsApp, X, LinkedIn, one dashboard | F30 to F35 on all channels, F36 to F42 per shop |

## Channels at a Glance

| Channel | Role | Build step |
|---|---|---|
| Facebook | Main sales channel | Phases 1 to 4 |
| Instagram | Visual products, comment-to-DM | 5A |
| WhatsApp | Orders, confirmations, order updates | 5B |
| X | Public support and direct messages | 5C |
| LinkedIn | Business leads and quotes | 5D |

## Google at a Glance

| Google tool | Used for | Features |
|---|---|---|
| Sheets | Product, stock, offer and delivery data in. Orders, leads and reports out | F37, F38 |
| Docs | Policies, FAQs, style guide, invoice and quotation templates | F39, F41 |
| Drive | Product images, PDF catalogs and price lists | F40 |
| Gmail, Google Chat, Looker Studio | Notifications and reports | F42 |
| Sign in with Google | Connecting files and logging in | F36 |

---

[Start reading: Introduction →](01_Introduction.md)
