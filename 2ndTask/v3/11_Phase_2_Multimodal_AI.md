*AI Sales and Order Assistant for Facebook Businesses · Project Documentation v3*

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

## Evidence to Collect

Banglish chat, voice message reply, image match, accuracy results, injection test results.

> [ Insert screenshots here ]

---

[← Phase 1: Core Agent that Does Real Work](10_Phase_1_Core_Agent.md) | [Index](00_README.md) | [Phase 3: Production-Grade Engineering →](12_Phase_3_Production_Engineering.md)
