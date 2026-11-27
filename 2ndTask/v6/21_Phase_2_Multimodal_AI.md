*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# Phase 2: Multimodal and Smarter AI

**Goal:** Understand customers the way they really write, speak and share information, and make the agent safe against wrong or tricky input.

## Features Included

| ID | Feature | Scope in this phase |
|---|---|---|
| F02 | Language Understanding | Full: Banglish glossary, Bangla digits, reply language rules |
| F04 | Conversation Memory and Returning Customers | Full |
| F05 | Voice Message Understanding | Full |
| F06 | Image Understanding | Product photo matching |
| F07 | Document and Screenshot Understanding | Payment receipts, address screenshots |
| F08 | Shop Knowledge Base | Full |
| F21 | Guardrails | Full: reply validator, injection protection, limits |
| F29 | Personalization | Address titles (Vaiya, Apu, Sir, Madam) in chat, sample replies, and style checks in the validator |
| F39 | Google Docs Knowledge Base and Style Guide Sync | Policy, FAQ, product details and style guide documents |
| F40 | Google Drive Files | Product images, and PDF catalogs and price lists with owner review |

## Deliverables

- Multi-language prompts and tests.
- Voice, image and document processing modules.
- Knowledge base with the pilot shop's FAQs and policies.
- Reply validator and guardrail rules.
- A test set of **50 to 100 realistic messages** (including misspellings and Banglish) with measured accuracy.

## Done When

- Intent accuracy on the test set reaches the agreed target (proposed: 90% or higher).
- A voice message in Bangla is understood, and order-critical details are repeated for confirmation.
- A product photo is matched to the correct catalog item in the test cases, or correctly reported as not found.
- A payment screenshot is read, and TrxID and amount are extracted correctly in the test cases.
- A policy question is answered from the knowledge base, and an unknown question is handed over instead of guessed.
- All prompt injection and fake discount test cases fail to change any price or order.
- Zero price or stock errors in the test set.
- Address titles are correct on the test names, unclear names get neutral wording, and a customer's correction is applied immediately.
- Replies stay within the chosen length limit and match the chosen style in the test cases.
- Editing a policy in a Google Doc changes the agent's answer after the next sync, and text under a "Private" heading is never used.
- A style guide Doc is turned into a style profile that the owner can preview and approve.
- Product images from a Drive folder are matched to products, and the agent can send a product photo where the channel allows it.
- A PDF price list becomes a draft catalog that is applied only after the owner approves it.

## Evidence to Collect

Banglish chat, voice message reply, image match, accuracy results, injection test results.

> [ Insert screenshots here ]

---

[← Phase 1: Core Agent that Does Real Work](20_Phase_1_Core_Agent.md) | [Index](00_README.md) | [Phase 3: Production-Grade Engineering →](22_Phase_3_Production_Engineering.md)
