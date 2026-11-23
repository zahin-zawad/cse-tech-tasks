*AI Sales and Order Assistant for Facebook Businesses · Project Documentation v3.1*

# 9. End-to-End Scenarios

These four scenarios show how the features work together. Each step shows what the customer sees and what the system does behind the scenes.

---

## Scenario 1: A Customer Buys a Product

**Situation:** Rafiq messages the shop Page about a black polo.

| Step | Customer sees | Behind the scenes |
|---|---|---|
| 1 | Rafiq: "Black polo size M ache? dam koto?" | F01 verifies and queues the message. F03 finds intents stock_query and price_query |
| 2 | Agent: "Assalamu Alaikum Rafiq Vaiya! Yes, the black polo in size M is available. The price is 850 taka. Would you like to order?" | F09 reads price and stock from the database. F29 adds the greeting and the title Vaiya (the name clearly suggests a male customer, and this shop uses the casual style). The validator checks the numbers and the greeting |
| 3 | Rafiq: "Ha, nibo" | Intent order_intent. Order state starts (F10) |
| 4 | Agent: "Great! May I have your name?" | The agent asks one detail at a time |
| 5 | Rafiq gives name, phone and address | Phone and address are validated. F11 matches the zone and calculates the charge (80 taka) |
| 6 | Agent shows the summary (850 + 80 = 930 taka) with Confirm, Edit and Cancel buttons | Totals are made by code, not by AI |
| 7 | Rafiq taps **Confirm** | Order saved, stock reserved. F15 sends the owner a Telegram notification |
| 8 | Agent: "Your order is confirmed. We will call you shortly to confirm." | F13 schedules a call because Rafiq is a first-time customer |
| 9 | Rafiq's phone rings. The AI says "Assalamu Alaikum Vaiya, I am the shop's automated assistant", reads the order in short casual Bangla, and Rafiq says yes | F13 and F29 make the call: greeting, correct title and short script. F13 saves "Call Confirmed" |
| 10 | Rafiq receives a message with the courier tracking ID | F14 books the courier and saves the tracking ID |

---

## Scenario 2: A Comment Becomes a Sale

**Situation:** The shop posts a photo of a handbag. Nusrat comments "price?".

| Step | Customer sees | Behind the scenes |
|---|---|---|
| 1 | Nusrat comments "price?" | The gateway receives the comment event (F01) |
| 2 | A public reply appears: "Thank you! We have sent the details to your inbox." | F16 detects buying interest by keyword. The post is linked to the handbag product in setup |
| 3 | In Messenger: "Assalamu Alaikum Nusrat Apu! You asked about the Leather Handbag. It is 1,450 taka. Would you like to order?" | F16 sends the private reply. F09 provides the price |
| 4 | Nusrat continues in the chat | The normal order flow (Scenario 1) takes over |

---

## Scenario 3: A Lost Sale Is Won Back

**Situation:** Tania wants a pair of shoes but finds the delivery charge too high.

| Step | Customer sees | Behind the scenes |
|---|---|---|
| 1 | Tania: "Delivery charge onek beshi, 130 taka!" | F03 detects a complaint about delivery charge |
| 2 | Agent: "I understand. Would you like me to let you know if there is free delivery?" | F17 asks one gentle question |
| 3 | Tania: "Ha, janaio" | F17 saves a lost sale with the reason **delivery charge** and her consent |
| 4 | *Three days later the owner starts a free-delivery weekend for orders above 1,500 taka* | The owner creates a campaign (F25) |
| 5 | Tania receives: "Assalamu Alaikum Tania Apu! Good news! This weekend delivery is free above 1,500 taka. Your shoes are 1,800 taka. Shall I order them?" | F18 finds Tania as an eligible lost sale. All numbers come from the database |
| 6 | Tania: "Yes" | The order flow starts with the product and her saved details. Delivery charge is 0 |
| 7 | The owner dashboard shows "1 sale recovered" | F26 records the win-back result |

---

## Scenario 4: The Agent Hands Over to a Human

**Situation:** A customer receives a damaged product and writes an angry message.

| Step | Customer sees | Behind the scenes |
|---|---|---|
| 1 | Customer: "Product ta bhanga! Taka ferot chai!" | F03 detects a complaint, angry sentiment and a refund request |
| 2 | Agent: "I am very sorry about this. I am connecting you with our team now." | F22 triggers a handoff. The chat is switched to Human mode |
| 3 | No more automatic replies | The agent stops. Follow-ups for this customer are paused |
| 4 | The owner receives a Telegram message | F15 sends the customer, reason, summary, last messages and a suggested reply |
| 5 | The owner replies from the Facebook Page inbox | The human continues the chat directly |
| 6 | After solving it, the owner hands the chat back | The agent can serve the customer again |

---

## Scenario 5: Two Shops, Two Styles

**Situation:** Two shops use the same system. A customer named Karim asks each shop the same question: "Is the grey backpack available? What is the price?"

| | Shop A: casual and short | Shop B: professional and detailed |
|---|---|---|
| Settings (F29) | Casual and Short preset, Vaiya / Apu, light emoji | Professional and Detailed preset, Sir / Madam, no emoji |
| Greeting | "Assalamu Alaikum Karim Vaiya!" | "Assalamu Alaikum Karim Sir. Welcome to Shop B." |
| Reply | "Yes, it is available 😊 Price is 1,200 taka. Shall I order it for you?" | "The grey backpack is currently in stock. The price is 1,200 taka. If you wish, I can take your order now and arrange delivery to your address." |
| Facts used | Price 1,200 taka and in stock, from each shop's database | Price 1,200 taka and in stock, from each shop's database |
| Confirmation call | Short script: greeting, "Vaiya", total, "Do you confirm?" | Longer script: greeting, "Sir", full order details, "Do you confirm this order?" |

Only the wording and length differ. The facts and rules stay exactly the same.

---

[← Features: Personalization](09_Features_Personalization.md) | [Index](00_README.md) | [Phase 1: Core Agent that Does Real Work →](11_Phase_1_Core_Agent.md)
