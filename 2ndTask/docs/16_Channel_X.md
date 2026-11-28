*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# 16. Channel Profile: X

**Role:** Public customer questions, announcements and support. Customers often ask brands questions in public replies and then continue in direct messages.

**Build phase:** Phase 5C.

**Connected through:** the Universal Social Adapter (F30), with this channel's profile. Nothing on this page needs platform-specific code. It describes the **values in the profile** and the rules the Policy Engine uses.

## Profile at a Glance

| Profile section | Value for this channel |
|---|---|
| Login | OAuth with the shop's X account, plus purchased API credits |
| Verification | The method X requires, written into the profile when it is created from X's current documentation |
| Events accepted | Direct messages, replies and mentions |
| Messages sent | Direct messages and public replies |

## 16.1 What the System Does on X

| Capability | Support | Notes |
|---|---|---|
| Direct messages | Yes | Through the X API. Text and images. Voice support to be confirmed |
| Replies and mentions on posts | Yes | The agent can read replies and mentions and answer publicly |
| Buttons | No | Numbered or typed options |
| Private message from a public reply | Where allowed | The agent replies publicly, then sends a direct message only where the platform and the customer's privacy settings allow it |
| Full order process | Yes, in text | F10 to F15 using text choices |
| Human handoff | Yes | Dashboard inbox |
| Follow-ups and alerts | Limited | Only where the platform allows |

## 16.2 Setup Requirements

- An **X developer account** and an app, with permission to read and send direct messages for the shop's account.
- The shop's X account authorised with OAuth.
- **API credits** bought in advance, and a **spending limit** set in the developer console.
- A webhook or activity stream for incoming direct messages and mentions.

## 16.3 How Messages Flow

**Incoming:** X delivers direct message and mention events to the Universal Social Adapter, which verifies them with the method named in the X profile and converts them to the common message format.

**Outgoing:** The Policy Engine checks the reply. The adapter sends a direct message, or posts a public reply, using the X API and the X profile.

**Public reply to DM flow:**
1. A customer replies to a product post: "Price?"
2. The agent posts a short public reply with the price or an invitation to message the shop.
3. If a direct message is allowed, the agent sends the details privately. If not, the public reply asks the customer to message or follow the shop.

## 16.4 Rules and Limits

- Public replies must be short and follow X's automation rules and developer terms. No spam, no repeated identical replies and no mass unsolicited replies.
- Prices, stock and policy answers in public replies follow the same guardrails as everywhere (F21).
- The rules for who can be sent a direct message depend on the platform and the customer's settings. They are confirmed in X's current documentation before launch.
- Private details (phone, address, payment) are **never** asked for in public replies. The agent moves the customer to direct messages first.

## 16.5 Costs

X uses **pay-per-use** pricing with no monthly plans. Credits are bought in advance and used as requests are made. At the time of writing:

| Action | Approximate price |
|---|---|
| Read a direct message event | About 1 cent per message |
| Receive a message through the activity stream | About 1 cent per event |
| Send a direct message | About 1.5 cents per request |
| Creating a public post or reply | A few cents or less per request, and more for posts with links |

Prices can change and are shown in X's developer console. The system loads current prices as configuration (F35), tracks the cost of each conversation, and uses the spending limit as a hard safety stop.

## 16.6 Special Behaviour

- Public conversations are visible to everyone, so the agent keeps public replies short and polite, and never shares customer data.
- Complaints on X are handed to a human faster than on other channels, because they are public (F22).
- Style: short replies, because of length limits on public posts (F29).

## 16.7 Risks

| Risk | Handling |
|---|---|
| Credits run out | Budget alerts at 80%, hard stop at the limit, chats sent to humans |
| Costs grow with volume | Per-channel budget and cost per conversation in the dashboard |
| Platform rules or prices change | Rules and prices as configuration, checked before launch |
| Public complaint goes viral | Fast human handoff and a calm, factual public reply |

**Done when:** a direct message becomes an order, a public reply moves the customer to direct messages, the spending limit works, and a public complaint is handed to a human.

---

[← Channel Profile: WhatsApp](15_Channel_WhatsApp.md) | [Index](00_README.md) | [Channel Profile: LinkedIn →](17_Channel_LinkedIn.md)
