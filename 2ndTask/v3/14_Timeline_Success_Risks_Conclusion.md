*AI Sales and Order Assistant for Facebook Businesses · Project Documentation v3*

# Timeline, Success Measures, Risks and Conclusion

## Timeline

| Phase | Focus | Estimated Time |
|---|---|---|
| Phase 1 | Core agent and order taking | [x weeks] |
| Phase 2 | Multimodal and smarter AI | [x weeks] |
| Phase 3 | Production-grade engineering | [x weeks] |
| Phase 4 | Sales intelligence, integrations and pilot | [x weeks] |

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

## Expected Impact

- Small sellers can answer customers 24/7 without hiring extra staff.
- Fewer lost customers and more orders, including recovered sales.
- Fewer mistakes, fake orders and failed deliveries, because orders are confirmed by chat and by phone.
- Owners save hours of repeated work and get clear data about their business.
- The same system can be reused for many businesses.

## Conclusion

This project goes far beyond a simple price-reply bot. It is a complete sales and order assistant that understands Bangla, voice, images and documents, takes and confirms orders, books delivery, brings back lost customers with offers and free-delivery alerts, and hands over to humans when needed. A fast and secure Rust gateway and a flexible Python AI service make it reliable enough for real businesses. Every feature is defined step by step, and building it in four phases gives a working result at every stage, proven through a pilot with real sellers.

I thank my supervisor, Md Shoaib Khan, for his guidance.

---

[← Phase 4: Sales Intelligence, Automation and Scale](13_Phase_4_Sales_Intelligence_Scale.md) | [Index](00_README.md)
