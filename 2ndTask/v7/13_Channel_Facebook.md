*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# 13. Channel Profile: Facebook (Messenger and Page Comments)

**Role:** The main channel. It is built first (Phases 1 to 4) and has the full feature set.

**Connected through:** the Universal Social Adapter (F30), with this channel's profile. Nothing on this page needs platform-specific code. It describes the **values in the profile** and the rules the Policy Engine uses.

## Profile at a Glance

| Profile section | Value for this channel |
|---|---|
| Login | Page access token from Facebook login |
| Verification | HMAC signature, `X-Hub-Signature-256` header, Meta app secret |
| Events accepted | Messages, button clicks, comments |
| Messages sent | Text, buttons and quick replies, images, public comment reply, private reply |

## 13.1 What the System Does on Facebook

| Capability | Support | Notes |
|---|---|---|
| Messenger chat | Yes | Text, images, voice messages and documents |
| Buttons and quick replies | Yes | Confirm / Edit / Cancel and product choices |
| Comments on Page posts | Yes | The agent reads and replies to comments |
| Private reply from a comment | Yes | One message per comment, within the time allowed by Meta (currently 7 days) |
| Human handoff | Yes | Dashboard inbox and Messenger's handover feature (thread control) |
| Full order process | Yes | F10 to F15 |
| Follow-ups and alerts | Yes | Within the 24-hour window or allowed routes (F31) |

## 13.2 Setup Requirements

- A Meta app in Meta for Developers, and a Facebook Page where the owner is an admin.
- Messaging and comment permissions, approved through Meta's App Review before real customers use it. Before approval, only people with a role on the app (admins and testers) can use it.
- A webhook subscribed to messages, button clicks and Page feed (comments).
- A Page access token, saved encrypted and checked daily (F34).

## 13.3 How Messages Flow

**Incoming:** Facebook sends the event to the Universal Social Adapter. Using the Facebook profile, the adapter checks the `X-Hub-Signature-256` header, removes duplicates, converts the event to the common message format and queues it (F30).

**Outgoing:** The agent's reply passes the Policy Engine (F31). The adapter, using the Facebook profile, sends it with the Messenger Send API. Public comment replies use the Page comment endpoint. Private replies use the comment ID.

## 13.4 Rules and Limits

- The reply window is 24 hours from the customer's last action. Opening actions include a message and a comment.
- Outside the window: message tags for approved non-promotional uses, the Human Agent tag for human replies (up to 7 days), and one-time notification when the customer opts in.
- Meta requires an escalation path to a human for automated experiences. F22 provides it.
- Meta has a responsiveness policy for bots registered as automated, so replies must stay fast. The target of under 10 seconds supports this.

## 13.5 Costs

Check Meta's current pricing for Messenger before launch. If any charges apply, they are tracked in F35.

## 13.6 Special Behaviour

- **Post-to-product links:** the owner links a Page post to a product so comment-to-inbox knows which product the customer means (F16, F25).
- **Handover:** while a human is replying, Messenger thread control makes sure the agent does not also reply.

## 13.7 Risks

| Risk | Handling |
|---|---|
| App Review delays | Apply early. Test with admin and tester accounts meanwhile |
| Token expires | Daily check and alert (F34) |
| Policy changes | Rules kept as configuration (F31) |

**Phase:** 1 to 4.

---

[← Channel Overview and Comparison](12_Channel_Overview_and_Comparison.md) | [Index](00_README.md) | [Channel Profile: Instagram →](14_Channel_Instagram.md)
