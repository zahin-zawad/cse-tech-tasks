*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# 11. Adapter Profile Specification

This document explains **how one adapter connects every social media platform**. The idea is simple:

> **One adapter. Many channel profiles. No new code for a new platform.**

The adapter (F30) is the only part of the system that talks to social media platforms. It does not contain any platform-specific code. Everything that is different about a platform is written in a **channel profile**, a short configuration file. To connect a new platform, we write a new profile and run it through the test kit (F43).

---

## 11.1 The Idea in One Picture

```
 Facebook   Instagram   WhatsApp    X     LinkedIn    any new platform
     │          │          │        │        │              │
     └──────────┴──────────┴────┬───┴────────┴──────────────┘
                                │
             ┌──────────────────┴──────────────────┐
             │   UNIVERSAL SOCIAL ADAPTER (Rust)   │
             │                                     │
             │   Inbound half     Outbound half    │
             │   verify           render reply     │
             │   map to common    send request     │
             │   message format   retry, refresh   │
             │   deduplicate      rate limit       │
             └──────────────────┬──────────────────┘
                                │      ▲
                 reads one      │      │  platform differences
                 profile  ──────┘      └─ live only in profiles
                                │
                  Common message format (same for all)
                                │
                        Agent, order engine,
                       knowledge base (Python)
```

## 11.2 What the Adapter Does and Does Not Do

| The adapter does | The adapter does not |
|---|---|
| Receive events from any platform, verify them and convert them to the common message format | Contain code that is specific to Facebook, WhatsApp or any other platform |
| Turn the agent's channel-neutral reply into the platform's request, and send it | Decide what to say. That is the agent's job |
| Log in to the platform, refresh tokens and check their expiry | Decide whether a message is allowed. That is the Policy Engine's job (F31), using rules from the profile |
| Retry, rate limit and report errors in one standard way | Remove the platform's own rules, approvals, limits or costs |
| Download and upload media within the platform's limits | Work with platforms that have no API (assist mode is used instead) |

**An honest note:** a single adapter does not make platforms the same. Their rules, approvals, limits and costs still exist. The adapter moves these differences **out of the code and into profiles**, where they are easy to read, change, test and roll back.

## 11.3 What a Channel Profile Contains

| Section | What it holds | Used by |
|---|---|---|
| **Identity** | Platform name, profile version | Dashboard, logs |
| **Login** | Login method (OAuth 2, long-lived token, API key, signed request), which tokens are needed, how to refresh them, how to check expiry. Secrets are **not** stored here. They are kept in the secrets store and only referenced | Adapter, F34 |
| **Inbound** | How the platform's events are verified (the method, header and secret reference), the one-time webhook handshake, which events to accept, which to ignore (own messages, receipts), and the **mapping rules** that turn each event into the common message format | F01, F30 |
| **Outbound** | For every action (send text, send buttons, send image, send document, public reply, private reply, send template), the request to make: method, address, headers, body, and how to read the message ID from the response | F30 |
| **Media** | How to download a file the customer sent, how to upload one, size and type limits | F05, F06, F07, F40 |
| **Capabilities** | What the channel supports: buttons, voice, images, documents, public comments, private reply, templates, maximum message length | Agent, F30 |
| **Policy** | The reply window, routes outside the window (templates, tags, human period), opt-in rules, required notices, human escalation rules | Policy Engine (F31) |
| **Limits** | Request rate limits and concurrency | Adapter |
| **Costs** | The price model (per message, per request, free amounts) and a reference to the current rate card | F35 |
| **Errors** | How the platform's error replies map to the standard error codes (11.6) | Adapter, F23 |
| **Health** | How to test the connection and when a token expires | F34 |
| **Wizard** | The steps the owner follows to connect this platform, shown automatically in the dashboard | F34 |

## 11.4 Example Profile: WhatsApp (illustrative)

The exact format is fixed in Phase 1. Field names and values below are illustrative, and each value is confirmed against the platform's current documentation when the profile is written.

```yaml
id: whatsapp
name: WhatsApp Business Platform
version: 1

login:
  type: bearer_token
  token_ref: secret://shop/whatsapp_token     # kept in the secrets store, never here
  expiry_check: daily

inbound:
  verify:
    method: hmac_sha256
    header: X-Hub-Signature-256
    secret_ref: secret://app/meta_app_secret
  handshake: challenge_echo                    # one-time webhook setup
  events:
    - match: "entry[].changes[].value.messages[]"
      type: message
      map:
        customer_channel_id: "from"
        message_id: "id"
        message_type: "type"                   # text, audio, image, document
        text: "text.body"
        media_ref: "audio.id | image.id | document.id"
        received_at: "timestamp"
  ignore: ["statuses[]"]                       # delivery receipts
  dedupe_key: "id"

outbound:
  send_text:
    request: POST https://graph.facebook.com/{version}/{phone_number_id}/messages
    headers: { Authorization: "Bearer {token}" }
    body:
      messaging_product: whatsapp
      to: "{customer_channel_id}"
      type: text
      text: { body: "{text}" }
    response_id: "messages[0].id"
  send_buttons: { ... }                        # reply buttons for Confirm / Edit / Cancel
  send_template: { ... }                       # approved templates outside the window

capabilities:
  buttons: true
  voice: true
  images: true
  documents: true
  public_comments: false
  private_reply: false
  templates: true

policy:
  reply_window: { hours: 24, resets_on: customer_message }
  outside_window: [approved_template]
  opt_in_required_for_business_initiated: true
  human_escalation_required: true

costs:
  model: per_message
  rate_card_ref: rates://whatsapp/bangladesh

errors:
  window_closed: "<the platform's error for a closed reply window>"
  rate_limited: "<the platform's rate limit error>"
  auth_expired: "<the platform's expired token error>"

health:
  check: "GET {version}/{phone_number_id}"
  expect: 200

wizard:
  - Register the business number
  - Add the Meta Business details
  - Submit message templates for approval
  - Set the opt-in wording
```

## 11.5 Example: Adding a New Platform With Only a Profile

To show that no code is needed, imagine a fictional platform called **ExampleChat** with a simple API key, a signed webhook and text messages only.

```yaml
id: examplechat
name: ExampleChat
version: 1

login:
  type: api_key
  key_ref: secret://shop/examplechat_key

inbound:
  verify: { method: shared_secret, header: X-ExampleChat-Token, secret_ref: secret://shop/examplechat_secret }
  events:
    - match: "messages[]"
      type: message
      map:
        customer_channel_id: "user.id"
        message_id: "id"
        message_type: "kind"
        text: "body"
        received_at: "sent_at"
  dedupe_key: "id"

outbound:
  send_text:
    request: POST https://api.examplechat.example/v1/send
    headers: { X-Api-Key: "{key}" }
    body: { to: "{customer_channel_id}", body: "{text}" }
    response_id: "id"

capabilities: { buttons: false, voice: false, images: false, documents: false,
                public_comments: false, private_reply: false, templates: false, max_text: 2000 }

policy:
  reply_window: { hours: none }
  human_escalation_required: false

costs: { model: free }

wizard:
  - Create an API key in ExampleChat
  - Paste the key and the webhook secret
```

With this profile, the agent answers on ExampleChat. Because the profile says `buttons: false`, the order summary appears as "Reply 1 to confirm, 2 to edit, 3 to cancel". The adapter and the agent are not changed. In Phase 5, this same example is used as a **proof** that a new platform can be added with a profile only.

## 11.6 Standard Error Codes

Every platform reports errors differently. The profile maps them to the same standard codes, so the rest of the system reacts in the same way on every channel.

| Standard code | Meaning | What the system does |
|---|---|---|
| `rate_limited` | Too many requests | Wait and retry with increasing waiting time. Customer replies go before follow-ups |
| `auth_expired` | Token no longer valid | Channel shows Warning or Disconnected, owner is alerted (F34) |
| `window_closed` | The reply window is closed | The Policy Engine finds an allowed route, or blocks the message |
| `invalid_recipient` | The customer cannot be reached | Stop sending, log it, tell a human if important |
| `content_rejected` | The platform refused the content | Do not retry the same text. Hand to a human |
| `platform_down` | The platform is not working | Queue and retry. Other channels continue (F23) |
| `unknown` | Anything else | Log it in detail and alert the administrator |

## 11.7 F43 Adapter Profile Library and Test Kit (Phase 1 and Phase 5)

**Purpose:** Keep all channel profiles in one place, and make sure every profile works correctly and safely before it is used with real customers.

**Trigger:** A profile is created, changed or updated for a new platform rule.

**How it works**
1. **Profile library:** all profiles are stored together, with version numbers and a history of changes.
2. **Test kit:** a **mock platform** pretends to be the real platform. It sends fake events and checks the requests the adapter makes. A profile must pass all these tests:

| Test | What it checks |
|---|---|
| Verification | A correct signature is accepted, a wrong one is rejected |
| Receive | Each event type (text, voice, image, document, comment, button click) becomes the correct common message |
| Ignore | The shop's own messages and receipts are ignored |
| Duplicates | The same event sent twice is processed once |
| Send | Each action makes the correct request |
| Errors | Each error from the mock maps to the right standard code and the right reaction |
| Media | Files are downloaded and uploaded within the limits |
| Fallback | If buttons are not supported, numbered text options are used |
| Policy | Messages inside and outside the reply window are handled as the profile says |
| Health | The connection check works and a token expiry alert is raised |
| Secrets | The profile contains no secrets (checked automatically) |
| Load | The adapter keeps up at the expected message volume |

3. **Review:** a person checks every new or changed profile against the platform's current documentation.
4. **Rollout:** a new profile version is first used by one test shop or in **Approval mode** (F32), then by all shops.
5. **Rollback:** a bad version can be replaced by the previous one without a new release.

**Rules**
- Profiles never contain secrets. They only point to the secrets store (F24).
- Shop-specific values (account IDs, tokens, budgets, style overrides) are kept separate from the profile.
- When a platform changes a rule or a price, only the profile changes. It is re-tested and rolled out without a new release of the adapter.

**If something goes wrong:** If a new profile version fails in use (more errors, blocked messages), it is rolled back automatically or by the administrator, and an alert is sent.

## 11.8 How to Add a New Platform

| Step | What happens |
|---|---|
| 1 | Read the platform's developer documentation: login, webhooks, sending, limits, rules and costs |
| 2 | Create the developer account or app, and apply for any approvals. Start early, because approval time is outside our control |
| 3 | Copy the closest existing profile, or the empty template |
| 4 | Fill in every section of the profile (11.3) |
| 5 | If the platform has an unusual data shape, add a small **transform hook** to the profile. If it needs a login or verification method the adapter does not have yet, add that method to the adapter **once**. Every later profile can use it |
| 6 | Run the test kit against the mock platform until every test passes |
| 7 | Connect a real test account and run real messages in Approval mode |
| 8 | Check the Policy Engine rules for the platform with real examples |
| 9 | Publish the profile. The connection wizard for the new platform appears in the dashboard automatically |
| 10 | Watch channel health, errors and costs (F34, F35) |

## 11.9 What a Profile Can and Cannot Do

**A profile can:** describe any platform that has a normal API with logins, webhooks or polling, and message sending, including different message types, limits, rules and costs.

**A profile cannot:**
- Change a platform's rules, approvals or limits.
- Add features the platform does not offer.
- Run general code. Transform hooks are small, reviewed and sandboxed. They can only convert data. They cannot make network calls or read other data.
- Connect a platform that has **no API** for messaging. For such platforms, the system uses **assist mode** (F32): the agent drafts a reply and a human sends it. Unofficial access, such as scraping or browser bots, is never used.

**Optional transport:** if it is easier or safer to reach a platform through an approved messaging provider, the profile can point to that provider's API instead of the platform's own. The adapter and the agent do not change.

---

[← Features: Multi-Channel Platform](10_Features_Multi_Channel_Platform.md) | [Index](00_README.md) | [Channel Overview and Comparison →](12_Channel_Overview_and_Comparison.md)
