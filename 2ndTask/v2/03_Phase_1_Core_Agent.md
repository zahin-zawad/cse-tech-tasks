*AI Sales and Order Assistant for Facebook Businesses · Project Documentation*

# Phase 1: Core Agent that Does Real Work

**Goal:** Build the foundation: a complete text-based sales assistant with order taking.

**Features**
- Rust gateway that receives Facebook messages and passes them to the Python agent.
- Intent detection with Gemini (price, stock, order, delivery, other).
- Live **stock and variant** checking (for example, "Is the black one in size M available?").
- **Order taking:** collect product, size or color, quantity, name, phone and address, then confirm and save the order.
- **Delivery charge** calculated by the customer's area.
- **Owner notification** by email or Telegram for every new order.
- Human-like replies written by Gemini from database facts.

**Example**

> **Customer:** "Black polo size M ache? dam koto?"
> **Agent:** "Yes, the black polo in size M is available. The price is 850 taka. Would you like to order it?"

**Deliverables**
- Working Rust gateway and Python agent service.
- Products, customers and orders tables in PostgreSQL.
- Owner notification.

**Evidence:** system architecture running, sample chat, order saved in the database.

> [ Insert screenshots here ]

---

[← System Overview](02_System_Overview.md) | [Index](00_README.md) | [Phase 2: Multimodal and Smarter AI →](04_Phase_2_Multimodal_AI.md)
