*AI Sales and Order Assistant for Facebook Businesses · Project Documentation*

# Risks, Impact and Conclusion

## Risks and Solutions

| Risk | Solution |
|---|---|
| AI gives a wrong price | Prices only from the database; AI never decides numbers |
| Unusual Bangla or Banglish | Test set with real examples; improve prompts |
| Customer tries to trick the AI | Guardrails and strict rules in the prompt |
| Voice or image misunderstood | Ask the customer to confirm before placing the order |
| Facebook token expires or webhook fails | Token renewal, retries and error alerts |
| Duplicate messages create duplicate orders | Duplicate event detection in the Rust gateway |
| Agent cannot handle a request | Hand over to a human |
| Follow-up messages break platform rules | Send only within Meta's messaging policies and with customer consent |
| Calling customers without permission | Call only customers who gave their phone number for the order and agreed to confirmation |
| AI cost grows | Track cost per conversation and set usage limits |
| Customer data privacy | Store only required data, secure the database, and restrict access |

## Expected Impact

- Small sellers can answer customers 24/7 without hiring extra staff.
- Fewer lost customers and more orders, including recovered sales.
- Fewer mistakes in order taking, because orders are confirmed by chat and by phone.
- Owners save hours of repeated work and get clear data about their business.
- The same system can be reused for many businesses.

## Conclusion

This project goes far beyond a simple price-reply bot. It is a complete sales and order assistant that understands Bangla, voice, images and documents, takes and confirms orders, books delivery, brings back lost customers with offers and free delivery alerts, and hands over to humans when needed. By combining a fast and secure Rust gateway with a flexible Python AI service, the system is designed to be reliable enough for real businesses. Building it in four phases gives a working result at every stage and moves the project step by step toward a real product, which will be proven through a pilot with real sellers.

I thank my supervisor, Md Shoaib Khan, for his guidance.

---

[← Timeline and Success Measures](07_Timeline_and_Success_Measures.md) | [Index](00_README.md)
