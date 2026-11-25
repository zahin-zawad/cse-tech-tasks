*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# 13. Channel: Instagram

**Role:** A channel for visual products such as fashion and lifestyle. Customers often start from a post, reel or story, so comment-to-DM is the key feature.

**Build phase:** Phase 5A. It is built first among the new channels because it uses the same Meta platform, gateway verification and Page link as Facebook.

## 13.1 What the System Does on Instagram

| Capability | Support | Notes |
|---|---|---|
| Direct messages | Yes | Text, images and media. Messages can arrive in the Inbox or the Requests folder |
| Buttons and quick replies | Yes | Confirmed per feature during implementation. Text options are the fallback |
| Voice messages | Yes | Handled as in F05 |
| Comments on posts and reels | Yes | The agent reads and replies to comments |
| Private reply from a comment | Yes | Only one message per comment, within 7 days |
| Story replies and mentions | Yes | Treated as a message that opens the window |
| Human handoff | Yes | Dashboard inbox. The Human Agent tag allows human replies for up to 7 days |
| Full order process | Yes | F10 to F15 |

## 13.2 Setup Requirements

- An Instagram **professional account** (business or creator) linked to the shop's Facebook Page.
- The Meta app with Instagram messaging and comment permissions. **Advanced Access** is needed for people who have no role on the app, so App Review is required for real customers.
- The Human Agent feature enabled, because Meta requires an escalation path to a human.
- A webhook subscribed to messages and comments.

## 13.3 How Messages Flow

**Incoming:** Instagram sends events to the same Rust gateway. It uses the Meta signature check. The Instagram connector converts the event to the common message format.

**Outgoing:** The Policy Engine checks the reply. The connector sends it through the Instagram Messaging API.

**Comment-to-DM (F16):**
1. A comment arrives, and the agent decides if it shows buying interest.
2. The agent posts a short public reply.
3. The agent sends **one** private reply using the comment ID.
4. The private reply goes to the customer's Inbox if they follow the account, or to the Requests folder if not.
5. The conversation continues **only if the customer replies**. Then the 24-hour window opens.

## 13.4 Rules and Limits

- Reply window: 24 hours after the customer's message.
- One private reply per comment, within 7 days. For Instagram Live comments, only while the live broadcast is on.
- One-time notification and sponsored messages are **not available** on Instagram. So win-back alerts (F18) and restock alerts (F20) can only be sent while the window is open or by a human within the Human Agent period. The Policy Engine blocks anything else.
- Meta asks automated experiences to inform users that they are chatting with an automation. The agent adds a short AI-assistant notice to its first reply (F31).
- Customer messages and data must be handled under Meta's platform terms, including deleting data when Meta sends a deletion notice.

## 13.5 Costs

Check Meta's current pricing for Instagram messaging before launch. Charges, if any, are tracked in F35.

## 13.6 Special Behaviour

- Many Instagram customers do not give a phone number early. The agent collects it only at the order step, as on Facebook.
- Style: the owner can use a short and casual style on Instagram (F29).

## 13.7 Risks

| Risk | Handling |
|---|---|
| Customer never replies to the private reply | Record as lost sale. No further messages are possible, so rely on posts and a human follow-up |
| Messages land in Requests and are missed by the customer | Keep the first message short and clear |
| App Review delays | Apply early and test with admin and tester accounts |

**Done when:** an Instagram comment becomes a private reply, a customer completes an order in Instagram DM, and a human takes over from the dashboard.

---

[← Channel: Facebook (Messenger and Page Comments)](12_Channel_Facebook.md) | [Index](00_README.md) | [Channel: WhatsApp →](14_Channel_WhatsApp.md)
