*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# 12. Channel Overview and Comparison

This document compares the five channels side by side. All five use the **same Universal Social Adapter**. The differences below are the values in each channel's profile (document 11). Each channel then has its own document (13 to 17).

**Important:** Platform rules, prices and API access change often. The facts here were checked against the platforms' documentation in November 2026. Each one must be confirmed again before that channel goes live.

## 12.1 The Role of Each Channel

| Channel | Best used for | Typical customers |
|---|---|---|
| **Facebook** | Main sales channel. Full order process | Most Bangladeshi small-business customers |
| **Instagram** | Fashion, lifestyle and visual products. Comment-to-DM | Younger customers who discover products in posts and reels |
| **WhatsApp** | Direct sales, order confirmations and order updates | Customers who prefer private chat, repeat buyers, and customers abroad |
| **X** | Public questions, announcements and customer support | Customers who ask brands questions in public |
| **LinkedIn** | Business customers: price lists, quotes and leads | Companies and professionals (B2B) |

## 12.2 What the System Can Do on Each Channel

| Capability | Facebook | Instagram | WhatsApp | X | LinkedIn |
|---|---|---|---|---|---|
| Customer sends direct messages | Yes | Yes | Yes | Yes | Yes, if the Page has messaging enabled |
| Agent replies automatically in direct messages | Yes | Yes | Yes | Yes | Only if LinkedIn approves messaging access. Otherwise assist mode |
| Voice messages | Yes | Yes | Yes | To be confirmed | No |
| Images and documents | Yes | Images and media | Yes | Images | Limited |
| Buttons and quick replies | Yes | Yes (to be confirmed per feature) | Yes (reply buttons and lists) | No, text options | No, text options |
| Public comments or replies | Yes | Yes | No | Yes (replies and mentions) | Yes (comments and mentions) |
| Private reply from a comment | Yes, one message, within 7 days | Yes, one message, within 7 days | Not applicable | Public reply with an invitation to message | Public reply with an invitation to message |
| Full order process (F10 to F15) | Yes | Yes | Yes | Yes, text only | Lead capture and quotes (see document 17) |
| Human handoff | Yes | Yes | Yes | Yes | Yes |
| Win-back, restock and reminders | Within the 24-hour window and allowed routes | Within the 24-hour window | Within the window, or with approved templates | Where the platform allows | Not available |

## 12.3 Reply Rules and Costs

| | Facebook | Instagram | WhatsApp | X | LinkedIn |
|---|---|---|---|---|---|
| Reply window | 24 hours | 24 hours | 24 hours, restarts with each customer message | See document 16 | Admin replies only after the member messages first |
| Human reply period | Up to 7 days (Human Agent tag) | Up to 7 days (Human Agent tag) | Within the 24-hour window, then templates | See document 16 | Not applicable |
| Message outside the window | Tags, one-time notification | Not for promotions | Approved templates only | Platform rules | Not possible |
| Charges | Check Meta's current pricing | Check Meta's current pricing | Per message. Service messages are charged after a free monthly amount from 1 October 2026. Rates depend on the country | Pay-per-use credits. For example, reading a message about 1 cent and sending one about 1.5 cents (prices can change) | None known for the messaging features that are available |
| Access needed | Meta app and App Review | Meta app, linked Page, Advanced Access | WhatsApp Business Platform, verified business, templates | X developer account and credits | Developer app, approval for Community Management API, messaging only by approval |
| Build phase | 1 to 4 | 5A | 5B | 5C | 5D |

## 12.4 What the Agent Does When a Channel Cannot Do Something

| If the channel cannot... | The agent... |
|---|---|
| Show buttons | Offers numbered or typed choices ("Reply 1 to confirm, 2 to edit, 3 to cancel") |
| Receive voice messages | Politely asks the customer to type the message |
| Send a private message from a comment | Replies publicly and invites the customer to message the shop |
| Send a message outside the window | Does not send. The Policy Engine blocks it, and a human is told if it is important |
| Send automatically at all (LinkedIn without approval) | Switches to assist mode: drafts for a human to send |

## 12.5 Rules That Apply to Every Channel

- All channels are connected through the same Universal Social Adapter. Only the channel profile differs.
- Every first reply starts with a greeting (F29).
- Prices, stock and totals come only from the database (F21).
- Every outgoing message passes the Channel Policy Engine (F31).
- A human is always reachable (F22).
- Consent for alerts is stored **per channel** (F33).
- Only official platform APIs are used. Scraping, browser bots and unofficial APIs are never used.

---

[← Adapter Profile Specification](11_Adapter_Profile_Specification.md) | [Index](00_README.md) | [Channel Profile: Facebook (Messenger and Page Comments) →](13_Channel_Facebook.md)
