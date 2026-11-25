# AI Sales and Order Assistant for Social Media Businesses

**Project Documentation, Version 5 (Planning Stage)**

| | |
|---|---|
| **Organization** | CSE TECH, Daffodil International University |
| **Supervisor** | Md Shoaib Khan |
| **Submitted by** | Zahin Zawad |
| **Student ID** | 232-15-886 |
| **Date** | [DD Month YYYY] |
| **Version** | 5.0 |
| **Technology** | Python and Rust |

---

## About This Project

An AI-powered sales and order assistant that works on **Facebook, Instagram, WhatsApp, X and LinkedIn** from **one platform and one dashboard**. It understands Bangla, English and Banglish, answers using real shop data, takes and confirms orders, brings back lost customers with offers and free-delivery alerts, hands over to a human when needed, and sounds like each business's own brand.

## What Is New in Version 5

**Multi-channel platform.** Everything built for Facebook now works on the other channels in the same way.

- **One agent, many channels.** A connector for each platform converts messages to one common format, so the agent logic is written once (F30, document 10).
- **A Policy Engine** checks every outgoing message against the platform's rules (reply windows, templates, consent, budget) before it is sent (F31).
- **One dashboard and one inbox** for all channels, with Auto, Approval and Human modes (F32).
- **One customer across channels.** The same person is recognised on several channels, safely, with consent kept per channel (F33).
- **Connection wizard and health monitor** for every channel (F34).
- **Cost control per channel** for WhatsApp message charges and X API credits (F35).
- **A document for each channel** (12 to 16) covering what the system can do there, setup, rules, costs and risks, based on each platform's current rules. LinkedIn is planned in levels because it limits automated messaging.
- **Phase 5** adds the channels in a clear build order.
- Related parts were updated: scope, architecture, message pipeline, comment-to-inbox, follow-ups, handoff, reliability, security, owner settings, analytics, personalization, scenarios (new Scenarios 6 to 8), timeline and risks.

## Earlier Versions

Version 3.1 defined every feature step by step (F01 to F29), including personalization: reply style, address titles (Vaiya, Apu, Sir, Madam) and the greeting rule. Those features remain unchanged and now work on every channel.

## Contents

| # | File | What it covers |
|---|---|---|
| 1 | [Introduction](01_Introduction.md) | Problem, solution, users, goals, scope |
| 2 | [System Overview](02_System_Overview.md) | Principles, architecture, how one message is processed, components, stack |
| 3 | [Feature Index](03_Feature_Index.md) | The complete list of 35 features with phases |
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
| 17 | [End-to-End Scenarios](17_End_to_End_Scenarios.md) | Eight complete examples |
| 18 | [Phase 1: Core Agent](18_Phase_1_Core_Agent.md) | Features, deliverables, done-when |
| 19 | [Phase 2: Multimodal and Smarter AI](19_Phase_2_Multimodal_AI.md) | Features, deliverables, done-when |
| 20 | [Phase 3: Production-Grade Engineering](20_Phase_3_Production_Engineering.md) | Features, deliverables, done-when |
| 21 | [Phase 4: Sales Intelligence and Scale](21_Phase_4_Sales_Intelligence_Scale.md) | Features, deliverables, pilot, done-when |
| 22 | [Phase 5: Multi-Channel Platform](22_Phase_5_Multi_Channel_Platform.md) | New channels, one dashboard, extended pilot |
| 23 | [Timeline, Success, Risks and Conclusion](23_Timeline_Success_Risks_Conclusion.md) | Schedule, targets, risks, impact |

## Phase Summary

| Phase | Focus | Features |
|---|---|---|
| Phase 1 | Core agent and order taking (Facebook first) | F01, F02, F03, F09, F10, F11, F15, F25, F29, F30, F31 |
| Phase 2 | Multimodal and smarter AI | F02, F04, F05, F06, F07, F08, F21, F29 |
| Phase 3 | Production-grade engineering | F22, F23, F24, F25, F27, F32, F34 |
| Phase 4 | Sales intelligence, integrations and pilot (Facebook) | F12, F13, F14, F16 to F20, F26, F28, F29 |
| Phase 5 | Multi-channel platform: Instagram, WhatsApp, X, LinkedIn, one dashboard | F30 to F35 on all channels |

## Channels at a Glance

| Channel | Role | Build step |
|---|---|---|
| Facebook | Main sales channel | Phases 1 to 4 |
| Instagram | Visual products, comment-to-DM | 5A |
| WhatsApp | Orders, confirmations, order updates | 5B |
| X | Public support and direct messages | 5C |
| LinkedIn | Business leads and quotes | 5D |

---

[Start reading: Introduction →](01_Introduction.md)
