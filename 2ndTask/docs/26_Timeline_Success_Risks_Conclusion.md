*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# Timeline, Success Measures, Risks and Conclusion

## Timeline

| Phase | Focus | Estimated Time |
|---|---|---|
| Phase 1 | Core agent and order taking | [x weeks] |
| Phase 2 | Multimodal and smarter AI | [x weeks] |
| Phase 3 | Production-grade engineering | [x weeks] |
| Phase 4 | Sales intelligence, integrations and pilot | [x weeks] |
| Phase 5 | Multi-channel platform: Instagram, WhatsApp, X, LinkedIn and one dashboard | [x weeks] |

*(Time to be decided with the supervisor.)*

## Success Measures

The targets below are proposals to be agreed with the supervisor.

| Area | Target |
|---|---|
| Response time | Under 10 seconds for 95% of messages |
| Intent understanding accuracy | 90% or higher on the 50 to 100 message test set |
| Price and stock errors | Zero (data always from the database) |
| Handoff | All unclear, angry or refund chats passed to a human |
| Order confirmation | Share of orders confirmed by the calling agent |
| Recovered sales | Customers who bought after a price or free-delivery alert |
| System reliability | No lost messages. Failures detected and alerted |
| Business impact | Faster replies and more orders in the pilot |
| Channel coverage | The same accuracy and safety on every connected channel, tested with each channel's own test messages |
| Channel compliance | No message sent outside a platform's rules. The Policy Engine blocks them |
| Cost control | Cost per channel tracked, and budget alerts working |
| Google sync | A change in a Google Sheet or Doc reaches the agent within the agreed time (default: 5 minutes), and invalid data never reaches customers |
| One adapter | A new platform can be added with a profile and the test kit only, shown with a mock platform |

## Risks and Solutions

| Risk | Solution |
|---|---|
| AI gives a wrong price | Prices only from the database. The reply validator blocks mismatched numbers |
| Unusual Bangla or Banglish | Test set with real examples. Improve prompts and the glossary |
| Customer tries to trick the AI | Guardrails. Customer text is treated as data only |
| Voice or image misunderstood | The agent repeats key details and asks the customer to confirm |
| Facebook token expires or webhook fails | Token checks, retries and error alerts |
| Duplicate messages create duplicate orders | Duplicate detection in the gateway and unique order keys |
| Agent cannot handle a request | Hand over to a human with a summary |
| Follow-up messages break platform rules | Send only within Meta's policies and with customer consent |
| Meta permissions are not approved in time | Start the app review early. Test with Page admins and testers meanwhile |
| Calling customers without permission | Call only the number given for the order, tell the customer first, and identify as automated |
| Bangla speech recognition errors on calls | Closed questions only, and fall back to a Messenger confirmation |
| Payment fraud | Verification by the owner, TrxID reuse checks, and optional advance payment. Never ask for PIN or OTP |
| AI cost grows | Track cost per conversation, use at most 2 AI calls per message, set daily limits |
| Customer data privacy | Encryption, access control, retention limits and deletion on request |
| Wrong address title (Vaiya, Apu, Sir or Madam) | Use a title only when confident, ask the customer, accept corrections instantly, and use neutral wording when unsure |
| Replies do not match the shop's style | Sample replies, validator checks, shop templates, and an owner preview before going live |
| Platform rules or prices change | Rules are kept as configuration in the Policy Engine and reviewed regularly. Each channel is checked against the platform's current documentation before launch |
| A platform does not grant API access (especially LinkedIn messaging) | LinkedIn is built in levels: comments first, messaging only if approved, and an assist mode as a fallback. The other channels do not depend on it |
| WhatsApp or X costs grow with volume | Per-channel budgets, alerts at 80%, and proactive messages paused first when a limit is reached (F35) |
| The same person is wrongly merged across channels | Merge automatically only on strong evidence such as a matching verified phone number, otherwise ask the customer, and allow the owner to unlink |
| Customer receives a message on a channel they did not agree to | Consent is stored per channel. The Policy Engine blocks messages without it |
| One channel breaks (expired token, outage) | Channel health monitoring, alerts, queueing, and other channels continue to work |
| Meta, X or LinkedIn app reviews take longer than planned | Start every approval early, in the order of the build plan, and test with admin and tester accounts meanwhile |
| A platform changes its API, rules or prices | Only that platform's profile changes. The test kit re-runs, the new version is rolled out to one shop first, and it can be rolled back (F43) |
| A new platform needs something the adapter cannot do | Add the missing login or verification method to the adapter once, or use assist mode if the platform has no messaging API |
| One adapter is a single point of failure | Run more than one copy, health checks, automatic restart, and a queue that holds messages (F23) |
| Owners break their Google Sheet (deleted columns, wrong formats, accidental deletes) | Column mapping, cleaning, row checks, a hold when too much changes at once, and the last good copy kept in the database |
| Google's request limits are reached | The agent reads from the database, not from Google. Sync requests are batched, cached and retried |
| Google's approval for broader access takes time or is refused | Use narrow per-file access and sharing with the system's service account. Do not ask for access to all of a user's Drive |
| Owner and assistant both change stock | Two stock modes, check-before-write, and an alert when the sheet changed in between |
| Customer data in Google files is shared by mistake | Files stay in the owner's own Google account, the system only writes to its own tabs, and phone numbers can be masked |

## Expected Impact

- Small sellers can answer customers 24/7 without hiring extra staff.
- Fewer lost customers and more orders, including recovered sales.
- Fewer mistakes, fake orders and failed deliveries, because orders are confirmed by chat and by phone.
- Owners save hours of repeated work and get clear data about their business.
- The same system can be reused for many businesses.

## Conclusion

This project goes far beyond a simple price-reply bot. It is a complete sales and order assistant that works on Facebook, Instagram, WhatsApp, X and LinkedIn as one platform through a single Universal Social Adapter, controlled from one dashboard, and works with the files owners already keep in Google Sheets, Docs and Drive. It understands Bangla, voice, images and documents, takes and confirms orders, books delivery, brings back lost customers with offers and free-delivery alerts, and hands over to humans when needed. A fast and secure Rust gateway and a flexible Python AI service make it reliable enough for real businesses. Every feature is defined step by step, and building it in five phases gives a working result at every stage, proven through a pilot with real sellers.

I thank my supervisor, Md Shoaib Khan, for his guidance.

---

[← Phase 5: Multi-Channel Platform](25_Phase_5_Multi_Channel_Platform.md) | [Index](00_README.md)
