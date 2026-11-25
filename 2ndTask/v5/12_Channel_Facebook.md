*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# 12. Channel: Facebook (Messenger and Page Comments)

**Role:** The main channel. It is built first (Phases 1 to 4) and has the full feature set.

## 12.1 What the System Does on Facebook

| Capability | Support | Notes |
|---|---|---|
| Messenger chat | Yes | Text, images, voice messages and documents |
| Buttons and quick replies | Yes | Confirm / Edit / Cancel and product choices |
| Comments on Page posts | Yes | The agent reads and replies to comments |
| Private reply from a comment | Yes | One message per comment, within the time allowed by Meta (currently 7 days) |
| Human handoff | Yes | Dashboard inbox and Messenger's handover feature (thread control) |
| Full order process | Yes | F10 to F15 |
| Follow-ups and alerts | Yes | Within the 24-hour window or allowed routes (F31) |

## 12.2 Setup Requirements

- A Meta app in Meta for Developers, and a Facebook Page where the owner is an admin.
- Messaging and comment permissions, approved through Meta's App Review before real customers use it. Before approval, only people with a role on the app (admins and testers) can use it.
- A webhook subscribed to messages, button clicks and Page feed (comments).
- A Page access token, saved encrypted and checked daily (F34).

## 12.3 How Messages Flow

**Incoming:** Facebook sends the event to the Rust gateway. The gateway checks the `X-Hub-Signature-256` header, removes duplicates and queues the event. The Facebook connector converts it to the common message format (F30).

**Outgoing:** The agent's reply passes the Policy Engine (F31). The connector sends it with the Messenger Send API. Public comment replies use the Page comment endpoint. Private replies use the comment ID.

## 12.4 Rules and Limits

- The reply window is 24 hours from the customer's last action. Opening actions include a message and a comment.
- Outside the window: message tags for approved non-promotional uses, the Human Agent tag for human replies (up to 7 days), and one-time notification when the customer opts in.
- Meta requires an escalation path to a human for automated experiences. F22 provides it.
- Meta has a responsiveness policy for bots registered as automated, so replies must stay fast. The target of under 10 seconds supports this.

## 12.5 Costs

Check Meta's current pricing for Messenger before launch. If any charges apply, they are tracked in F35.

## 12.6 Special Behaviour

- **Post-to-product links:** the owner links a Page post to a product so comment-to-inbox knows which product the customer means (F16, F25).
- **Handover:** while a human is replying, Messenger thread control makes sure the agent does not also reply.

## 12.7 Risks

| Risk | Handling |
|---|---|
| App Review delays | Apply early. Test with admin and tester accounts meanwhile |
| Token expires | Daily check and alert (F34) |
| Policy changes | Rules kept as configuration (F31) |

**Phase:** 1 to 4.

---

[← Channel Overview and Comparison](11_Channel_Overview_and_Comparison.md) | [Index](00_README.md) | [Channel: Instagram →](13_Channel_Instagram.md)
