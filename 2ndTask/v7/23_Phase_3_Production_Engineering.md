*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# Phase 3: Production-Grade Engineering

**Goal:** Make the system safe, reliable and ready for real customers.

## Features Included

| ID | Feature | Scope in this phase |
|---|---|---|
| F22 | Human Handoff | Full, with Messenger handover and owner notification |
| F23 | Reliability | Duplicates, idempotency, retries, fallback, token check |
| F24 | Security and Privacy | Full |
| F25 | Shop Setup and Owner Controls | Full web panel, including a style preview (F29) |
| F32 | Unified Inbox and Single Dashboard | Inbox with one channel (Facebook) at this stage. The design already supports many channels |
| F34 | Channel Connection and Health | Connection status and token expiry for Facebook |
| F36 | Google Connection and Access | Full: Sign in with Google, file picker, health status, and disconnect |
| F27 | Logging, Cost Tracking and Monitoring | Full |

## Deliverables

- System deployed on a cloud server with Docker, HTTPS and automatic restart.
- Handoff and error workflows.
- Owner panel with login and roles.
- Monitoring, alerts, logging and backup setup.
- Automated tests for the gateway, order logic and guardrails.
- Meta app review submitted for the required messaging permissions.

## Done When

- A handoff trigger passes the chat to a human, the agent stops replying, and the owner receives the summary.
- Stopping the AI service still gives the customer a fallback reply and alerts the administrator.
- Sending the same event twice, or restarting the server during processing, never creates a duplicate order or loses a message.
- Webhook requests with a wrong signature are rejected.
- Each conversation shows its response time and AI cost in the logs.
- Alerts fire for high failure rate, queue backlog and token expiry.
- A backup can be restored in a test.
- The owner can add products, rules and policies from the panel without developer help.
- The owner can choose a style and preview sample replies from the panel before going live.
- The owner sees all Facebook conversations in the dashboard inbox, can reply from it, and sees the connection status and token expiry.
- The owner connects Google with Sign in with Google, picks the exact files, sees the sync status, and can disconnect. After disconnecting, the system no longer reads or writes those files.
- A sync failure, a held sync and a revoked Google permission each show a clear alert.

## Evidence to Collect

Handoff alert, error alert, server deployment, owner panel, logs and metrics.

> [ Insert screenshots here ]

---

[← Phase 2: Multimodal and Smarter AI](22_Phase_2_Multimodal_AI.md) | [Index](00_README.md) | [Phase 4: Sales Intelligence, Automation and Scale →](24_Phase_4_Sales_Intelligence_Scale.md)
