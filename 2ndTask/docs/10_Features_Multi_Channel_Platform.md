*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# 10. Features: Multi-Channel Platform

These features turn the assistant into **one platform for all social media channels**. The agent logic described in documents 04 to 09 works the same way on every channel. These six features handle what is different between channels, and give the owner one place to control everything.

Supported channels: **Facebook, Instagram, WhatsApp, X and LinkedIn.** Each channel is described in documents 13 to 17. The adapter and the channel profile format are defined in document 11 (F43).

---

## F30 Universal Social Adapter (Phase 1 with Facebook, Phase 5 for the others)

**Purpose:** Connect **every** social media platform through **one adapter**. A new platform is added by writing a channel profile, not new code.

**Trigger:** Every incoming event from a platform, and every outgoing message to a platform.

**How it works**
1. **One adapter, two halves.** Both halves are in the same Rust service:
   - **Inbound half** (the gateway, F01): receives the platform's webhook, verifies it, removes duplicates, converts the event to the **common message format**, and puts it in the queue.
   - **Outbound half:** takes the agent's channel-neutral reply, turns it into the platform's request, sends it, handles retries and rate limits, refreshes tokens, and reports the result.
2. **Channel profiles.** Everything that is specific to a platform is written in a **channel profile** (F43, document 11): how to log in, how to verify events, how to read each event, how to send each kind of message, what the channel supports, its rules, limits, costs, health check and setup steps. The adapter reads the profile and behaves accordingly.
3. **The common message format** has the same fields for every channel:

```json
{
  "shop_id": "shop_01",
  "channel": "whatsapp",
  "conversation_id": "c_48213",
  "customer_channel_id": "8801XXXXXXXXX",
  "message_type": "voice",
  "content": { "audio_ref": "media_9921" },
  "reply_context": null,
  "received_at": "2026-11-25T10:42:11Z"
}
```

   Message types: text, voice, image, document, comment, button_click.
   `reply_context` holds the post or comment ID for comments.

4. **Capabilities** come from the profile. The agent asks the adapter instead of assuming:

| Capability | Example question the agent asks |
|---|---|
| Buttons and quick replies | Can I show Confirm / Edit / Cancel as buttons? |
| Voice, images, documents | Can the customer send these, and can I download them? |
| Public comments | Can I reply to comments on this channel? |
| Private reply | Can I start a private chat from a comment? |
| Reply window | How long can I reply after the customer's last message? |
| Maximum reply length | How long can a message be? |

5. **The agent writes a channel-neutral reply** (text, optional choices, optional attachments). The adapter turns it into buttons where the profile says they are supported, or into a numbered text list where they are not.
6. **Standard error codes.** Each platform's errors are mapped by the profile to the same codes (`rate_limited`, `auth_expired`, `window_closed`, `invalid_recipient`, `content_rejected`, `platform_down`, `unknown`). The Policy Engine and the reliability features (F23) react the same way on every channel.

**Rules**
- There is **no platform-specific code** in the adapter, the agent, the order engine or the knowledge base. It all lives in profiles.
- Adding a platform means adding a profile and passing the test kit (F43). The adapter and the agent do not change.
- The adapter supports a fixed set of **login methods** (such as OAuth 2, long-lived token, API key and signed request) and **verification methods** (such as signature check, challenge reply and shared secret). A profile chooses from them. If a new platform needs a method the adapter does not have, the method is added **once**, and every later profile can use it.
- If a platform has an unusual data shape, a profile may use a small **transform hook**: a short, reviewed, sandboxed function that only converts data.
- A profile can point to an approved messaging provider instead of the platform's own API, if that is easier or safer. The adapter and the agent do not change.
- A single adapter does **not** remove a platform's own rules, approvals, limits or costs. It moves them into profiles where they are easy to read, change and test (see document 11).

**If something goes wrong:** If the adapter cannot convert an event, the event is stored as "unsupported", the customer gets a polite message asking for text, and the owner is notified. If the adapter service stops, it restarts automatically and queued messages are not lost (F23).

---

## F31 Channel Policy Engine (Phase 1 with Facebook rules, Phase 5 for the others)

**Purpose:** Make sure no message breaks a platform's rules. Breaking rules can lead to blocked accounts, so this check cannot be left to the AI.

**Trigger:** Before **every** outgoing message: replies, follow-ups, alerts, human replies and call-back messages.

**How it works**
1. The sender (the agent, a background job or a human) asks to send a message and says which **channel**, **customer** and **kind of message** it is (reply, follow-up, alert, human reply).
2. The engine loads the channel's rules from its channel profile (F43), so they can be updated without changing code.
3. It checks, in this order:
   1. Is the customer's **reply window open**?
   2. If not, is there an **allowed route** (approved template, message tag, one-time notification, or the human-agent period)?
   3. Did the customer **agree** to this kind of message **on this channel**?
   4. Is the content allowed for that route (for example, non-promotional for tagged messages)?
   5. Are **quiet hours** and **frequency limits** respected?
   6. Is **budget** left for this channel (F35)?
   7. Does the first message include the **automation notice** where the platform requires it?
   8. Has the customer said **stop**?
4. The engine returns one decision:

| Decision | Meaning |
|---|---|
| **Send** | Send now as a normal message |
| **Send with route** | Send using a template, tag or other allowed route |
| **Delay** | Wait (for example until quiet hours end) |
| **Block** | Do not send. The reason is logged. Important blocked messages are shown to the owner |

**Summary of the main rules** (confirmed from the platforms' documentation at the time of writing, and re-checked before launch)

| Channel | Normal reply window | Outside the window |
|---|---|---|
| Facebook Messenger | 24 hours from the customer's last action | Message tags for approved uses, the Human Agent tag (humans only, up to 7 days), one-time notification with the customer's opt-in |
| Instagram | 24 hours | The Human Agent tag (humans only, up to 7 days). One-time notification and sponsored messages are not available |
| WhatsApp | 24 hours, restarting with each customer message | Only approved message templates. Starting a chat needs a template and opt-in |
| X | Rules are checked in the current X documentation (see document 16) | Only what the platform allows |
| LinkedIn | An admin can message a member only after the member has messaged the Page | Not possible |

**Automation notice:** Where a platform requires it, the first reply tells the customer they are chatting with the shop's AI assistant and how to reach a person. This comes together with the greeting (F29).

**Rules**
- The engine decides. If it says Block, nothing can override it, not the AI and not a background job.
- Platform rules change, so each channel's rules are reviewed before launch and regularly after that.

**Example:** A win-back alert (F18) for an Instagram customer whose window closed 3 days ago. The engine sees the window is closed, no allowed route exists on Instagram for promotional content, and returns **Block**. The owner sees "Not sent: outside Instagram's reply window".

---

## F32 Unified Inbox and Single Dashboard (Phase 3 for Facebook, Phase 5 for all channels)

**Purpose:** Let the owner and staff handle all channels in one place, with one login.

**Trigger:** The owner or staff open the dashboard, or a conversation needs a human.

**How it works**
1. **One inbox** lists conversations from all channels. Each row shows the channel icon, customer name, last message, status and waiting time.
2. **Filters and search:** by channel, status (agent handling, waiting for human, resolved), assigned staff member, unread, order stage, and customer name or phone.
3. **Opening a conversation** shows the whole chat, the customer profile (including linked channels, F33), the order progress, and for handoffs a summary with a suggested reply.
4. **Replying:** the human types a reply. It goes through the Channel Policy Engine (F31) and the Universal Social Adapter (F30), which sends it on the right channel. The screen shows how much reply time is left (for example "WhatsApp window: 3 hours left").
5. **Three modes for each conversation:**

| Mode | What happens |
|---|---|
| Auto | The agent replies by itself |
| Approval | The agent writes a draft and a human approves it before sending. Used in the first pilot days |
| Human | The agent is silent and a person replies |

6. **Assignment and notes:** staff can take a conversation, assign it, and add internal notes the customer never sees.
7. **Assist mode** (used where a channel does not allow automated sending, see LinkedIn in document 17): the agent writes a draft and the human copies it into the platform.
8. **One home screen:** today's conversations, orders, response time, channel health and unanswered questions, with a split by channel.

**Rules**
- One login, with roles: owner (everything) and staff (inbox, orders and customers only).
- A conversation in Human mode cannot be answered by the agent until it is handed back (F22).
- All actions are recorded in the audit log (F24).

---

## F33 Cross-Channel Customer Identity (Phase 5)

**Purpose:** Recognise the same person across channels, so history, saved details and context are not lost.

**Trigger:** A customer contacts the shop on a new channel, or gives details (phone number) in a conversation.

**How it works**
1. Each channel gives a different ID: Facebook sender ID, Instagram ID, WhatsApp number, X ID, LinkedIn ID. Each ID belongs to one **customer profile**.
2. A new ID starts as a new profile.
3. Profiles are **linked** in one of three ways:

| Way | When |
|---|---|
| **Automatic** | Strong evidence only: the same **verified phone number** (for example, the WhatsApp number matches the phone number given in an order on another channel) |
| **Asked** | Weaker signs (similar name, mention of another channel). The agent asks the customer: "Are you the same Mim who asked on Instagram?" The link is made **only after the customer confirms** |
| **By the owner** | The owner links or unlinks profiles in the dashboard |

4. After linking, the profiles share: order history, saved name, address and title (F04, F29), return risk notes, and lost-sale records (F17).

**Rules**
- A matching name alone **never** links profiles.
- **Consent is per channel.** If a customer agreed to alerts on Instagram, that does not allow messages on WhatsApp. Alerts are sent only on the channel where consent was given and where the platform's rules allow it (F31).
- Any link can be undone. If the customer says they are a different person, the link is removed.
- Linking a profile does not expose one channel's private messages to another channel's customer.
- Deleting a customer's data (F24) removes all linked profiles.

---

## F34 Channel Connection and Health (Phase 3 for Facebook, Phase 5 for all channels)

**Purpose:** Make connecting a channel simple for the owner, and detect problems before customers notice.

**Trigger:** The owner adds a channel, or the daily health check runs.

**How it works**
1. **Connection wizard** in the dashboard. The steps for each channel come from its channel profile (F43), so a new channel gets its own wizard without any new screens. Steps for the current channels:

| Channel | Main steps in the wizard |
|---|---|
| Facebook | Log in with Facebook, choose the Page, allow the permissions, subscribe to messages and comments |
| Instagram | Connect the professional account that is linked to the Facebook Page, and allow the permissions |
| WhatsApp | Choose or register the business number, add the Meta Business details, create and submit message templates, and set opt-in wording |
| X | Authorise the shop's X account, buy API credits, and set a spending limit |
| LinkedIn | Authorise as a Page admin. Shows which level of access is approved: comments only, or messaging too |

2. **Health status** for each channel:

| Status | Meaning |
|---|---|
| **Connected** | Everything works |
| **Warning** | Token expires soon, error rate rising, or budget nearly used |
| **Disconnected** | Access removed or expired. The agent cannot answer on this channel |

3. The health check shows the last event received, token expiry date, recent error rate, permissions granted, and has a **Send test message** button.
4. **Alerts** go to the owner and administrator by Telegram or email. A Warning alert is sent 7 days before a token expires.
5. **Reconnecting** keeps all history and settings.

**Rules**
- A broken channel never stops the others (F23).
- The owner can switch the agent off for one channel without affecting the rest.
- The wizard checks each requirement (for example, "WhatsApp templates approved: 3 of 4") before the channel can be set to live.

---

## F35 Channel Cost and Quota Control (Phase 5)

**Purpose:** Keep running costs under control, because some channels charge for messages or API use.

**Trigger:** Every message sent or received, and the daily and monthly budget checks.

**What is tracked**

| Cost | Details |
|---|---|
| AI cost | Gemini use for each conversation, on every channel |
| WhatsApp | Charges per message by message type. Rules and prices differ by country and change over time (Meta changed WhatsApp pricing in 2025 and again from 1 October 2026), so the current rate card for Bangladesh is loaded into the system |
| X | API credits. The pay-per-use model charges per request or per received event, for example reading a message and sending a message have separate prices |
| Messenger, Instagram, LinkedIn | Charges are tracked if the platform introduces them, and rate limits are tracked |

**How it works**
1. Every sent and received message is recorded with its channel, type and cost.
2. The owner sets a **monthly budget for each channel** (for example, a WhatsApp budget and an X credit limit).
3. At **80% of the budget**, the owner gets a warning.
4. At **100%**, the system acts in this order:
   1. Pauses **proactive** messages first (follow-ups, win-back alerts, restock alerts).
   2. Keeps answering customers' own messages as long as the platform allows and the hard limit has not been reached.
   3. At the hard limit, pauses the channel's automatic replies, sends the chats to humans (F22), and alerts the owner.
5. The dashboard shows **cost per channel, per conversation and per order**.

**Rules**
- The budget check is part of the Policy Engine (F31), so a message over budget is blocked or delayed according to the rules above.
- Replies to customers always have priority over proactive messages.
- Rate cards and prices are part of each channel profile (F43), not code, so changes do not need a new release.

---

[← Features: Personalization](09_Features_Personalization.md) | [Index](00_README.md) | [Adapter Profile Specification →](11_Adapter_Profile_Specification.md)
