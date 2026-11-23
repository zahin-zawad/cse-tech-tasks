# AI Sales and Order Assistant for Facebook Businesses

**Project Documentation, Version 3.1 (Planning Stage)**

| | |
|---|---|
| **Organization** | CSE TECH, Daffodil International University |
| **Supervisor** | Md Shoaib Khan |
| **Submitted by** | Zahin Zawad |
| **Student ID** | 232-15-886 |
| **Date** | [DD Month YYYY] |
| **Version** | 3.1 |
| **Technology** | Python and Rust |

---

## About This Project

An AI-powered sales and order assistant for Facebook Pages. It understands Bangla, English and Banglish, answers using real shop data, takes and confirms orders, brings back lost customers with offers and free-delivery alerts, and hands over to a human when needed. Each business can personalize how the agent talks.

## What Is New in Version 3.1

**Personalization feature (F29)**, described in [09_Features_Personalization.md](09_Features_Personalization.md):
- **Reply style:** each business chooses long and professional replies, or short, casual, human-like replies, with three presets and custom settings.
- **Address titles:** the agent and the calling agent address customers as Vaiya or Apu, or Sir or Madam, according to gender, and use neutral wording when unsure.
- **Greeting rule:** the agent always starts a conversation, and every call, with a greeting.

Related parts were updated: the message pipeline, order taking, the calling agent, guardrails, owner settings, the scenarios (including a new Scenario 5), the phases and the risks.

## What Was New in Version 3

Every feature (F01 to F29) is described with the same structure: purpose, trigger, how it works step by step, rules, example, and what happens if something goes wrong. The path of one message, the order process, the sales features and four end-to-end scenarios are all defined clearly. Each phase lists its features and a "Done When" checklist.

## Contents

| # | File | What it covers |
|---|---|---|
| 1 | [Introduction](01_Introduction.md) | Problem, solution, users, goals, scope |
| 2 | [System Overview](02_System_Overview.md) | Principles, architecture, how one message is processed, components, stack |
| 3 | [Feature Index](03_Feature_Index.md) | The complete list of 29 features with phases |
| 4 | [Conversation and Understanding](04_Features_Conversation_and_Understanding.md) | F01 to F09 |
| 5 | [Orders and Fulfilment](05_Features_Orders_and_Fulfilment.md) | F10 to F15 |
| 6 | [Sales Intelligence and Engagement](06_Features_Sales_Intelligence_and_Engagement.md) | F16 to F20 |
| 7 | [Safety, Handoff and Reliability](07_Features_Safety_Handoff_and_Reliability.md) | F21 to F24 |
| 8 | [Owner Dashboard and Analytics](08_Features_Owner_Dashboard_and_Analytics.md) | F25 to F28 |
| 9 | [Personalization](09_Features_Personalization.md) | F29: reply style, address titles, greetings |
| 10 | [End-to-End Scenarios](10_End_to_End_Scenarios.md) | Five complete examples |
| 11 | [Phase 1: Core Agent](11_Phase_1_Core_Agent.md) | Features, deliverables, done-when |
| 12 | [Phase 2: Multimodal and Smarter AI](12_Phase_2_Multimodal_AI.md) | Features, deliverables, done-when |
| 13 | [Phase 3: Production-Grade Engineering](13_Phase_3_Production_Engineering.md) | Features, deliverables, done-when |
| 14 | [Phase 4: Sales Intelligence and Scale](14_Phase_4_Sales_Intelligence_Scale.md) | Features, deliverables, pilot, done-when |
| 15 | [Timeline, Success, Risks and Conclusion](15_Timeline_Success_Risks_Conclusion.md) | Schedule, targets, risks, impact |

## Phase Summary

| Phase | Focus | Features |
|---|---|---|
| Phase 1 | Core agent and order taking | F01, F02, F03, F09, F10, F11, F15, F25, F29 |
| Phase 2 | Multimodal and smarter AI | F02, F04, F05, F06, F07, F08, F21, F29 |
| Phase 3 | Production-grade engineering | F22, F23, F24, F25, F27 |
| Phase 4 | Sales intelligence, integrations and pilot | F12, F13, F14, F16 to F20, F26, F28, F29 |

---

[Start reading: Introduction →](01_Introduction.md)
