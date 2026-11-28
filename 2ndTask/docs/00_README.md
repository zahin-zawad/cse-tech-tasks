# AI Sales and Order Assistant for Social Media Businesses

**Project Documentation, Version 7 (Planning Stage)**

| | |
|---|---|
| **Organization** | CSE TECH, Daffodil International University |
| **Supervisor** | Md Shoaib Khan |
| **Submitted by** | Zahin Zawad |
| **Student ID** | 232-15-886 |
| **Date** | [DD Month YYYY] |
| **Version** | 7 |
| **Technology** | Python and Rust |

---

## About This Project

An AI-powered sales and order assistant that works on **Facebook, Instagram, WhatsApp, X and LinkedIn**, and any other social media platform, through **one Universal Social Adapter** and **one dashboard**. It works with the files owners already use in **Google Sheets, Docs and Drive**. It understands Bangla, English and Banglish, answers using real shop data, takes and confirms orders, brings back lost customers with offers and free-delivery alerts, hands over to a human when needed, and sounds like each business's own brand.

## What Is New in Version 7

**One adapter for all social media** (document 11, features F30 and F43)

In version 5, each platform had its own connector. In version 7 there is **one adapter**, and platforms are not integrated differently anymore.

- **One Universal Social Adapter (Rust).** It receives, verifies and converts events from any platform into the common message format, and sends replies back. It contains no platform-specific code.
- **Channel profiles.** Everything that differs between platforms (login, verification, event mapping, send requests, capabilities, rules, limits, costs, setup steps) is written in a short configuration file for each platform.
- **A new platform needs only a new profile.** It is checked with a test kit against a mock platform, tried in Approval mode, and then published. The adapter and the agent are not changed. A fictional example (ExampleChat) is worked through in documents 11 and 20.
- **Honest limits.** A single adapter does not remove a platform's own rules, approvals, limits or costs, and it cannot connect platforms that have no messaging API (assist mode is used instead). It moves these differences out of the code and into profiles, where they are easy to read, change and test.
- The five channel documents (13 to 17) now describe **channel profiles**, each with a "Profile at a Glance" table.
- The connection wizard, the Policy Engine rules, the costs and the standard error codes all come from the profile.

Everything from earlier versions is kept: multi-channel platform with one dashboard (F32 to F35), Google integration (F36 to F42), personalization (F29), and all core features (F01 to F28).

## Contents

| # | File | What it covers |
|---|---|---|
| 1 | [Introduction](01_Introduction.md) | Problem, solution, users, goals, scope |
| 2 | [System Overview](02_System_Overview.md) | Principles, architecture, how one message is processed, components, stack |
| 3 | [Feature Index](03_Feature_Index.md) | The complete list of 43 features with phases |
| 4 | [Conversation and Understanding](04_Features_Conversation_and_Understanding.md) | F01 to F09 |
| 5 | [Orders and Fulfilment](05_Features_Orders_and_Fulfilment.md) | F10 to F15 |
| 6 | [Sales Intelligence and Engagement](06_Features_Sales_Intelligence_and_Engagement.md) | F16 to F20 |
| 7 | [Safety, Handoff and Reliability](07_Features_Safety_Handoff_and_Reliability.md) | F21 to F24 |
| 8 | [Owner Dashboard and Analytics](08_Features_Owner_Dashboard_and_Analytics.md) | F25 to F28 |
| 9 | [Personalization](09_Features_Personalization.md) | F29 |
| 10 | [Multi-Channel Platform](10_Features_Multi_Channel_Platform.md) | F30 to F35, with the Universal Social Adapter |
| 11 | [Adapter Profile Specification](11_Adapter_Profile_Specification.md) | **New.** How one adapter connects any platform, profile format, examples, test kit, F43 |
| 12 | [Channel Overview and Comparison](12_Channel_Overview_and_Comparison.md) | Side-by-side comparison of all channels |
| 13 | [Channel Profile: Facebook](13_Channel_Facebook.md) | Messenger and Page comments |
| 14 | [Channel Profile: Instagram](14_Channel_Instagram.md) | Direct messages and comment-to-DM |
| 15 | [Channel Profile: WhatsApp](15_Channel_WhatsApp.md) | Chat, templates, costs and rules |
| 16 | [Channel Profile: X](16_Channel_X.md) | Direct messages, public replies, credits |
| 17 | [Channel Profile: LinkedIn](17_Channel_LinkedIn.md) | B2B leads, comments, messaging levels |
| 18 | [Google Integration](18_Features_Google_Integration.md) | F36 to F42: Sheets, Docs, Drive, Gmail, Chat, Looker Studio |
| 19 | [Google Sheet and Doc Templates](19_Google_Sheet_and_Doc_Templates.md) | How the owner's files should look |
| 20 | [End-to-End Scenarios](20_End_to_End_Scenarios.md) | Ten complete examples |
| 21 | [Phase 1: Core Agent](21_Phase_1_Core_Agent.md) | Features, deliverables, done-when |
| 22 | [Phase 2: Multimodal and Smarter AI](22_Phase_2_Multimodal_AI.md) | Features, deliverables, done-when |
| 23 | [Phase 3: Production-Grade Engineering](23_Phase_3_Production_Engineering.md) | Features, deliverables, done-when |
| 24 | [Phase 4: Sales Intelligence and Scale](24_Phase_4_Sales_Intelligence_Scale.md) | Features, deliverables, pilot, done-when |
| 25 | [Phase 5: Multi-Channel Platform](25_Phase_5_Multi_Channel_Platform.md) | New channel profiles, one dashboard, extended pilot |
| 26 | [Timeline, Success, Risks and Conclusion](26_Timeline_Success_Risks_Conclusion.md) | Schedule, targets, risks, impact |

## Phase Summary

| Phase | Focus | Features |
|---|---|---|
| Phase 1 | Core agent and order taking (Facebook profile first, Google Sheets for data) | F01, F02, F03, F09, F10, F11, F15, F25, F29, F30, F31, F36, F37, F38, F43 |
| Phase 2 | Multimodal and smarter AI, Google Docs and Drive | F02, F04, F05, F06, F07, F08, F21, F29, F39, F40 |
| Phase 3 | Production-grade engineering | F22, F23, F24, F25, F27, F32, F34, F36 |
| Phase 4 | Sales intelligence, integrations and pilot (Facebook) | F12, F13, F14, F16 to F20, F26, F28, F29, F38, F41, F42 |
| Phase 5 | More channel profiles on the same adapter, one dashboard | F30 to F35, F43 on all channels, F36 to F42 per shop |

## Channels at a Glance

All channels use the same adapter. Only the profile differs.

| Channel | Role | Build step |
|---|---|---|
| Facebook | Main sales channel | Phases 1 to 4 |
| Instagram | Visual products, comment-to-DM | 5A |
| WhatsApp | Orders, confirmations, order updates | 5B |
| X | Public support and direct messages | 5C |
| LinkedIn | Business leads and quotes | 5D |
| Any new platform | Added with a profile and the test kit | On demand |

---

[Start reading: Introduction →](01_Introduction.md)
