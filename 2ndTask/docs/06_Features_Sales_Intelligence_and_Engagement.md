*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# 6. Features: Sales Intelligence and Engagement

These features bring in new customers and recover customers who would otherwise be lost. All messages here follow Meta's messaging policies and the customer's consent (see the rules in each feature).

---

## F16 Comment-to-Inbox (Phase 4)

**Purpose:** Turn public comments such as "price?" into private sales conversations.

**Trigger:** A customer comments on a Page post and the comment shows buying interest.

**How it works**
1. The gateway receives the comment event (F01).
2. The system decides whether the comment shows buying interest. It first checks keywords (price, dam, koto, details, inbox, ডাম) and uses Gemini for unclear comments.
3. The owner can link each post to a product in setup (F25). If a link exists, the agent already knows which product the customer means.
4. The agent posts a short public reply, for example: "Thank you! We have sent you the details in your inbox."
5. The agent sends a private message using Messenger's private reply feature, starting the normal conversation. For example: "Hello! You asked about the Black Polo. The price is 850 taka. Would you like to order?"
6. From here the chat follows the normal pipeline.

**Rules**
- One public reply and one private reply per comment.
- The system ignores the Page's own comments, spam and abusive comments, and comments from people already in an active chat (it continues the chat instead).
- The private reply must be sent within the time window allowed by Meta.
- Price can be shown publicly only if the owner turns this on. The default is to share price in the inbox.

**How comment-to-inbox works on each channel**

| Channel | What the agent does |
|---|---|
| Facebook | Public reply, then one private reply to start the chat (within Meta's allowed time, currently 7 days) |
| Instagram | Public reply, then one private reply. It arrives in the customer's inbox, or in the Requests folder if they do not follow the account. The chat continues only if the customer replies |
| X | Public reply to the post, with an invitation to send a direct message. A direct message is sent only where the platform and the customer's settings allow it |
| LinkedIn | Public reply to the comment, with an invitation to message the Page or to use another channel. No automatic private message is sent |
| WhatsApp | No public comments. Instead, posts, ads and the website use a click-to-chat link or QR code that opens the WhatsApp chat |

**If something goes wrong:** If the private reply cannot be sent, the public reply asks the customer to message the Page, and the event is logged.

---

## F17 Lost-Sale Tracking (Phase 4)

**Purpose:** Record **why** a customer did not buy, so the system can bring them back when the reason is removed.

**Trigger:** A conversation about a product ends without an order. This is detected when the customer says they will not buy, stops replying after seeing a price or delivery charge, or cancels.

**How it works**
1. **Detect the reason.** Gemini analyses the last messages and classifies the reason:

| Reason | Examples of what the customer says |
|---|---|
| Price too high | "dam onek beshi", "too expensive", asks for discount, stops after price |
| Delivery charge too high | "delivery charge beshi", stops after delivery charge |
| Out of stock | Wanted size or color not available |
| Other | "I will think", "later", no reason |

2. If the reason is clear, it is saved with a confidence score.
3. If the customer hesitates but gives no reason, the agent may ask one gentle question: "Is the price or delivery charge the concern? I can notify you if there is an offer." The agent asks **once** and never pushes.
4. The system saves: customer, product, reason, the price and delivery charge shown at that time, and whether the customer agreed to be notified. The record is also added to the Lost Sales tab of the owner's Google Sheet, if connected (F38).

**Rules**
- A lost sale is only used for alerts if the customer **agreed to be notified**, or if the alert can be sent within Meta's rules for that conversation.
- The agent does not guess the reason when confidence is low. It saves "Other".
- The customer can say "stop" at any time and will not be contacted again.

---

## F18 Offer and Free-Delivery Win-Back Alerts (Phase 4)

**Purpose:** Win back customers when the reason they left disappears.

**Trigger:** The owner creates or starts an **offer** (price drop or discount on a product) or a **free-delivery campaign** (for an area, minimum order, or date range).

**How it works**
1. The owner creates the offer or campaign in setup (F25) or in the Offers tab of the shop's Google Sheet (F37), with start and end dates.
2. The system searches for matching lost sales:
   - **Offer on a product:** customers who left that product because of **price**.
   - **Free delivery:** customers who left because of **delivery charge**, in the matching area or order amount.
3. Eligibility is checked for each customer:
   - They agreed to be notified, and have not said "stop".
   - The new price or delivery charge is a real improvement. The owner sets a minimum discount that triggers an alert (default: 5%).
   - They have not received an alert in the last 7 days (default).
   - The offer is still active and the product is in stock.
4. Gemini writes a short personal message using verified facts. Example: "Good news! The Black Polo you liked is now 700 taka, down from 850. Reply YES and I will place your order."
5. If the customer replies "YES", the order flow (F10) starts with the product and details already known.
6. The result (alert sent, replied, ordered) is saved.

**Rules**
- Maximum 2 alerts per lost sale. After that the record is closed.
- The message states the real offer price from the database. The agent never invents a discount.
- Whether a message may be sent outside the reply window is decided per channel by the Channel Policy Engine (F31). On WhatsApp an approved message template and customer opt-in are required. On Instagram, one-time notifications and sponsored messages are not available. On Messenger, only the allowed message types are used. On X and LinkedIn, only paths the platform allows are used. The exact methods are confirmed against each platform's current policy during implementation.
- Customer consent is **per channel**. Consent given on one channel does not allow messages on another (see F33).
- The owner can turn alerts off per product or campaign.

**Example**
> A customer left because the delivery charge was 130 taka. The owner starts free delivery over 1500 taka this weekend. The agent messages: "This weekend delivery is free for orders above 1500 taka. Your Black Polo and the matching cap total 1650 taka. Shall I order them for you?"

---

## F19 Abandoned Order Follow-up (Phase 4)

**Purpose:** Remind customers who started an order but did not finish it.

**Trigger:** The order has passed the product-chosen step, but there has been no customer reply for 30 minutes (default).

**How it works**
1. A background worker finds stalled orders.
2. **Reminder 1** (after 30 minutes): a friendly nudge that mentions the product and the next missing step. Example: "You are one step away from your order. May I have your delivery address?"
3. **Reminder 2** (after about 20 hours, before the 24-hour window closes): a final reminder.
4. If the customer replies, the order continues from where it stopped.
5. If there is no reply after reminder 2, the order is closed as abandoned and the reservation is released.

**Rules**
- Maximum 2 reminders per order, sent only while the channel's reply window is open (24 hours on Messenger, Instagram and WhatsApp, see F31).
- No reminders at night (default quiet hours: 10:00 PM to 8:00 AM). They are sent after quiet hours end, if still inside the window.
- Reminders stop immediately if the customer says stop or cancels.

---

## F20 Restock Alerts (Phase 4)

**Purpose:** Tell customers when a product they wanted is available again.

**Trigger:** A customer asks for an out-of-stock product, and later stock becomes available.

**How it works**
1. When the product is out of stock, the agent offers: "It is out of stock now. Shall I notify you when it is back?"
2. If the customer agrees, the request is saved with the product, size and color.
3. When the owner updates stock and the item is available again, the system finds waiting customers.
4. Each customer receives one message: "The Black Polo (M) you asked for is back in stock. Would you like to order it?"
5. If the customer replies, the order flow starts.

**Rules**
- Only customers who agreed are notified, once per restock.
- Stock is not held for waiting customers. It is first come, first served.
- Delivery of the alert follows the same channel rules as F18 (F31).

---

[← Features: Orders and Fulfilment](05_Features_Orders_and_Fulfilment.md) | [Index](00_README.md) | [Features: Safety, Handoff and Reliability →](07_Features_Safety_Handoff_and_Reliability.md)
