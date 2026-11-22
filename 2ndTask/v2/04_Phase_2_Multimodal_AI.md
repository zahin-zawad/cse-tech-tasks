*AI Sales and Order Assistant for Facebook Businesses · Project Documentation*

# Phase 2: Multimodal and Smarter AI

**Goal:** Understand customers the way they really write, speak and share information.

**Features**
- **Bangla, English and Banglish** support (for example, "dam koto?" and "price ki?").
- **Voice messages:** transcribe and understand, then reply.
- **Images:** match a product photo sent by the customer to the closest catalog item.
- **Documents:** read files and screenshots sent by customers, such as payment receipts or product details.
- **Knowledge base (RAG):** answer return policy, delivery time, warranty and shop FAQs from the shop's own information.
- **Conversation memory:** remember earlier messages in the same chat.
- **Guardrails:**
  - Prices and stock come **only** from the database.
  - Protection against prompt injection and fake discount tricks.
  - The agent never promises anything outside shop policy.

**Deliverables**
- Multi-language prompts and tests.
- Voice, image and document processing modules.
- FAQ knowledge base.
- A test set of **50 to 100 realistic messages** (including misspellings and Banglish) with measured accuracy.

**Evidence:** Banglish chat, voice message reply, image match, accuracy results.

> [ Insert screenshots here ]

---

[← Phase 1: Core Agent](03_Phase_1_Core_Agent.md) | [Index](00_README.md) | [Phase 3: Production-Grade Engineering →](05_Phase_3_Production_Engineering.md)
