*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# 16. Channel: LinkedIn

**Role:** A channel for **business customers** (B2B): price lists, quotations, bulk orders and partnership enquiries. It is different from the other channels, because LinkedIn limits automated messaging. This document is honest about that and plans in levels.

**Build phase:** Phase 5D. It is built last, because it depends on LinkedIn's approval.

## 16.1 What LinkedIn Allows

- **Comments and mentions:** LinkedIn's Community Management API supports Page posts, comments and mentions. Access needs an application and review, in stages (a development tier first, then a standard tier).
- **Page messaging:** LinkedIn Pages can receive messages from members. An admin can message a member only after the member has messaged the Page first.
- **No general messaging API:** at the time of writing, LinkedIn's documented marketing APIs do not provide generic automated direct messaging. Some partner platforms offer LinkedIn Page messaging, so access may exist through LinkedIn's approval process. This must be confirmed with LinkedIn.
- **Unofficial tools** (scraping, browser bots and unofficial APIs) break LinkedIn's terms and can get the Page restricted. They are **never used** in this project.

## 16.2 Plan in Three Levels

| Level | What it does | Depends on |
|---|---|---|
| **Level A: Comments and mentions** (always built) | Reads comments and @mentions on Page posts. Answers product and price questions publicly. Invites the person to message the Page or to use another channel | Community Management API approval |
| **Level B: Page messaging** (built only if approved) | The full agent answers messages sent to the Page, with human handoff | LinkedIn's approval of messaging access |
| **Level C: Assist mode** (fallback) | The dashboard helps a human. The person pastes or opens the message, the agent drafts the reply with real shop data, and the human sends it in LinkedIn | Nothing. No automated sending |

The dashboard shows which level is active for the shop (F34).

## 16.3 What the System Does at Each Level

| Capability | Level A | Level B | Level C |
|---|---|---|---|
| Reads comments and mentions | Yes | Yes | No |
| Public reply to comments | Yes | Yes | Draft only |
| Reads Page messages | No | Yes | Manual |
| Replies to Page messages | No | Yes | Draft only |
| Price and quote answers | Public, short | Yes | Draft |
| Lead capture | Yes | Yes | Yes |
| Human handoff | Yes | Yes | Always human |
| Voice | No | No | No |

## 16.4 How LinkedIn Is Used for Business Customers

The consumer order flow (F10) is simplified for LinkedIn into **lead capture and quotes**:

1. The agent answers questions about products, prices and minimum order quantities, using the shop's real data.
2. For a bulk or custom request, it collects the company name, contact person, phone or email, product and quantity.
3. It saves this as a **lead** and notifies the owner or sales staff (F15) with a summary.
4. A human prepares the quotation and continues the conversation. The agent never promises custom prices (F21).

## 16.5 How Messages Flow

**Incoming:** LinkedIn sends notifications of comments and mentions to the gateway, which verifies them. If Level B is approved, Page messages are received in the same way. The LinkedIn connector converts the events to the common message format.

**Outgoing:** The Policy Engine checks every reply. At Levels A and B the connector sends through the official API. At Level C nothing is sent automatically.

## 16.6 Rules and Limits

- Replies go only to members who started the conversation. No unsolicited messages.
- No proactive follow-ups, win-back alerts or restock alerts on LinkedIn.
- Style: professional and detailed, with "Sir / Madam" or name only (F29).
- LinkedIn access tokens expire. They are checked daily and renewed (F34).
- The Community Management API must be the only product on its developer application, so LinkedIn needs a **separate developer app**.

## 16.7 Costs

No per-message charges are known for the available features. Approval work and time are the main costs. Any charges are tracked in F35.

## 16.8 Risks

| Risk | Handling |
|---|---|
| LinkedIn does not approve messaging access | The project does not depend on it. Level A and Level C still work |
| Approval takes a long time | Apply early in Phase 5, build Level A first |
| Rules change | Keep rules as configuration, review regularly |
| Someone asks for unofficial automation | Not allowed. Explain the risk to the owner |

**Done when:** comments and mentions on a Page post are answered with correct shop data, a bulk enquiry becomes a lead with an owner notification, assist mode produces a correct draft, and Level B works if LinkedIn approves it.

---

[← Channel: X](15_Channel_X.md) | [Index](00_README.md) | [End-to-End Scenarios →](17_End_to_End_Scenarios.md)
