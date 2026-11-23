*AI Sales and Order Assistant for Facebook Businesses · Project Documentation v3*

# 5. Features: Orders and Fulfilment

These features take an order from the first "I want it" to a delivered parcel.

---

## F10 Order Taking (Phase 1)

**Purpose:** Collect a complete and correct order in chat without any manual work.

**Trigger:** The intent is `order_intent`, or the customer answers "yes" to the agent's offer to order.

**How it works**

The order follows a fixed set of steps controlled by code, not by AI. Gemini only understands what the customer says and writes the messages. The agent asks **one question at a time**.

| Step | State | What the agent does |
|---|---|---|
| 1 | Product chosen | Confirms the product (from F09) |
| 2 | Variant | Asks size or color if the product has variants and it is not yet known |
| 3 | Quantity | Asks how many (default 1 if the customer says "one" or does not mention it) |
| 4 | More items | Asks if the customer wants to add another product. Several products can go in one order |
| 5 | Customer details | Collects name, phone and full address (area, district) one by one |
| 6 | Summary | Shows items, prices, delivery charge (F11) and total, with buttons **Confirm / Edit / Cancel** |
| 7 | Confirmed | Customer taps Confirm. The order is saved and stock is reserved |
| 8 | After order | Payment (F12), confirmation call (F13), courier (F14) and owner notification (F15) follow |

**Order summary shown to the customer**
```
Your order
1 x Black Polo (M)   850 taka
Delivery charge      80 taka
Total                930 taka
Payment: Cash on Delivery
Address: House 12, Road 5, Mirpur, Dhaka
[ Confirm ]  [ Edit ]  [ Cancel ]
```

**Rules**
- **Phone check:** a Bangladeshi mobile number must have 11 digits and start with 01. Otherwise the agent asks again.
- **Address check:** the address must contain an area and a district so the delivery charge and courier can be decided.
- **Editing:** before confirmation, the customer can change anything. The summary is rebuilt.
- **Cancel:** the customer can cancel before confirmation without any effect. After confirmation, cancellation is handled as in F22.
- **Stock:** stock is reserved when the order is confirmed. It is released if the order is cancelled.
- **Duplicate order check:** if the same customer orders the same product within 10 minutes, the agent asks: "Is this a new order or the same one?"
- **Returning customers:** saved details are offered for reuse (F04).
- Orders can only be confirmed through the Confirm button or a clear "yes". Never through a free-text guess.

**If something goes wrong:** If the product goes out of stock before confirmation, the agent tells the customer and suggests alternatives. If saving the order fails, the customer is told the shop team will confirm shortly and the owner is alerted (F23).

---

## F11 Delivery Charge Calculation (Phase 1)

**Purpose:** Calculate the correct delivery charge automatically from the customer's address.

**Trigger:** The address is collected during order taking, or the customer asks about delivery charge.

**How it works**
1. The owner defines delivery zones and charges in setup (F25). Example:

| Zone | Charge |
|---|---|
| Inside Dhaka | 80 taka |
| Outside Dhaka | 130 taka |

2. Code reads the district and area from the address and matches them to a zone. If the area is unclear, the agent asks for the district and area.
3. Code checks free-delivery rules: a minimum order amount (for example, free delivery above 2000 taka) or an active free-delivery campaign set by the owner (F18).
4. The final charge is calculated by code and shown in the order summary.

**Rules**
- The delivery charge is never decided by AI.
- If the customer asks "delivery charge koto?" before giving an address, the agent shows the zone prices and asks for their area.
- If a free-delivery rule applies, the summary clearly shows "Free delivery".

---

## F12 Payment Handling (Phase 4; Phase 1 supports Cash on Delivery only)

**Purpose:** Handle payment methods and keep the payment status of every order up to date.

**Trigger:** The order is confirmed and the customer chooses a payment method.

**Payment methods**
- **Cash on Delivery (COD):** the courier collects the total.
- **bKash or Nagad:** the customer sends money to the shop's merchant number.
- **Advance payment (optional):** the owner can require advance payment of the delivery charge or part of the total for high-value or high-risk orders. This reduces fake orders.

**How it works for bKash or Nagad**
1. The agent shows the shop's merchant number and the exact amount.
2. The customer pays and sends the transaction ID (TrxID) as text or a screenshot (read by F07).
3. The status becomes **Claimed**.
4. Code checks that the amount matches and that the TrxID has not been used before.
5. The owner or staff verifies the payment against the shop's own records with one tap in the dashboard or Telegram notification. The status becomes **Verified** or **Failed**.
6. Where the payment provider offers a merchant API or notification service, verification can be automated later.

**Payment statuses:** Pending, Claimed, Verified, Failed, COD.

**Rules**
- The agent **never asks for a PIN, OTP or password**, and tells customers never to share them.
- An order with an unverified advance payment is not sent to the courier.
- If advance payment is not verified within the owner's time limit (default: 2 hours), the agent reminds the customer once, then the order is cancelled and stock is released.

---

## F13 AI Calling Agent (Phase 4)

**Purpose:** Confirm orders by phone call to reduce fake orders and failed deliveries.

**Trigger:** A confirmed order that matches the owner's call rule. Default rule: calls for all Cash on Delivery orders of first-time customers and orders above a set amount. The owner can change the rule.

**How it works**
1. The agent tells the customer in chat: "We will call you shortly to confirm your order."
2. A background worker schedules the call during calling hours (default: 9:00 AM to 9:00 PM).
3. The voice provider places the call. The AI introduces itself as the shop's automated assistant and states the shop name.
4. It reads a short summary: items, total and delivery address, then asks: "Do you confirm this order?"
5. Speech-to-text understands the answer in Bangla and the system classifies it.

| Customer answer | Result |
|---|---|
| Confirms | Order marked **Call Confirmed**. Courier booking can continue |
| Cancels | Order cancelled, stock released, owner notified |
| Wants a change (address, quantity) | A message is sent in Messenger to collect the change, and the owner is notified |
| No answer | Retry up to 2 more times later. Then a Messenger confirmation message is sent |
| Unclear answer | Handed to a human (F22) |

6. The call result and a short transcript are saved to the order.

**Rules**
- Only closed questions are used (yes, no, change), which keeps the call short and reduces errors.
- The call is made only to the number the customer gave for the order.
- The AI always identifies itself as automated.
- If the customer asks to speak to a person, the call ends and a human is notified.

**If something goes wrong:** If the call provider fails, the system falls back to a Messenger confirmation message with Confirm and Cancel buttons.

---

## F14 Courier Booking and Tracking (Phase 4)

**Purpose:** Book delivery automatically and keep the customer informed.

**Trigger:** An order is ready to ship (confirmed, payment status acceptable, call confirmed if required).

**How it works**
1. The system sends the parcel details to the courier API (customer name, phone, address, items, and the amount to collect if COD).
2. The courier returns a tracking ID. It is saved to the order.
3. The customer receives a Messenger message with the tracking ID and expected delivery time.
4. Status updates from the courier (picked up, in transit, delivered, returned) are received and saved.
5. The customer is notified at key changes, and the owner sees the status in the dashboard.

**Rules**
- The first version supports **one courier** chosen with the pilot shop. Others are added later.
- The owner can choose automatic booking, or booking only after the owner approves each order.
- If a parcel is returned, the owner is alerted and the customer's profile is marked for future risk checks.

**If something goes wrong:** If booking fails, it is retried 3 times. After that the owner is alerted with the reason and can book manually.

---

## F15 Owner Notifications (Phase 1)

**Purpose:** Keep the owner informed instantly without checking the system all day.

**Trigger:** Events listed below.

**How it works:** Notifications are sent through Telegram or email, chosen by the owner. Each message is short and tells the owner what to do.

| Event | Message contains | Phase |
|---|---|---|
| New order | Order ID, items, total, address, payment method | 1 |
| System failure | What failed and which customer was affected | 1 |
| Handoff request | Customer, reason, last messages, suggested reply | 3 |
| Payment claimed | Order ID, amount, TrxID, Verify / Reject buttons | 4 |
| Call result | Confirmed, cancelled, changed or no answer | 4 |
| Courier problem | Booking failed or parcel returned | 4 |
| Daily summary | Orders, sales, unanswered questions, lost sales | 4 |

**Rules**
- Each event is sent once, with retry if delivery fails.
- Notifications have no customer private data beyond what the owner needs for the order.

---

[← Features: Conversation and Understanding](04_Features_Conversation_and_Understanding.md) | [Index](00_README.md) | [Features: Sales Intelligence and Engagement →](06_Features_Sales_Intelligence_and_Engagement.md)
