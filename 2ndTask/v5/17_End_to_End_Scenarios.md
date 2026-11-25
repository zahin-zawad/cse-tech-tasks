*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# 9. End-to-End Scenarios

These scenarios show how the features work together. Each step shows what the customer sees and what the system does behind the scenes.

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

## Scenario 6: An Order on WhatsApp

**Situation:** Sabbir messages the shop's WhatsApp number to buy a backpack.

| Step | Customer sees | Behind the scenes |
|---|---|---|
| 1 | Sabbir: "Hi, grey backpack ache?" | The WhatsApp connector (F30) receives the event. The Rust gateway checks the Meta signature. The message opens the 24-hour window |
| 2 | Agent: "Assalamu Alaikum! Yes, the grey backpack is available. The price is 1,200 taka. Would you like to order?" | Same agent logic as on Facebook. The Policy Engine (F31) confirms the window is open and sends a normal (non-template) message |
| 3 | Sabbir agrees. The agent asks for his name and address, and only confirms his phone number | F10 already knows the number from WhatsApp, so it is only confirmed |
| 4 | The summary appears with reply buttons Confirm, Edit and Cancel | The WhatsApp connector turns the choices into reply buttons |
| 5 | Sabbir taps Confirm | The order is saved. The owner is notified, with "Channel: WhatsApp" |
| 6 | Later, Sabbir receives an order update with the tracking ID | The window has closed, so the Policy Engine sends an **approved utility template** instead of a normal message. The WhatsApp charge is recorded (F35) |

---

## Scenario 7: Instagram Comment, then the Same Customer on WhatsApp

**Situation:** Mim comments "price?" on an Instagram reel. A week later she messages the shop on WhatsApp.

| Step | Customer sees | Behind the scenes |
|---|---|---|
| 1 | Mim comments "price?" on a reel | The Instagram connector receives the comment. F16 detects buying interest |
| 2 | A public reply: "Thank you! Please check your messages." | The agent sends one public reply |
| 3 | In her Instagram inbox (or Requests folder): "Assalamu Alaikum Mim Apu! The Linen Kurti is 1,850 taka. Would you like to order?" | One private reply, within the allowed 7 days. The chat continues only if Mim replies |
| 4 | Mim replies "dam ektu beshi" and leaves the chat | F17 records a lost sale on Instagram with the reason **price**. She did not agree to be notified, so no alert will be sent |
| 5 | A week later Mim messages the shop's WhatsApp number: "Hi, I asked about the Linen Kurti on Instagram." The agent asks: "Are you the Mim who asked on Instagram?" She answers yes | F33 links her WhatsApp and Instagram identities **only after her confirmation**. A matching name alone is never enough |
| 6 | Agent: "Welcome back, Mim Apu! The Linen Kurti is still 1,850 taka. Would you like to order it?" | The earlier conversation and saved details are now shared across both channels, and the order continues on WhatsApp |

---

## Scenario 8: One Dashboard for Every Channel

**Situation:** The shop owner uses the dashboard at the end of a busy day.

| Step | Owner sees | Behind the scenes |
|---|---|---|
| 1 | Home screen: today's conversations, orders and response time, split by channel | F26 combines data from all connected channels |
| 2 | Channel health: Facebook Connected, Instagram Connected, WhatsApp Connected, X Warning (token expires in 5 days), LinkedIn Connected (comments only) | F34 checks every channel daily. The owner also received an alert about the X token |
| 3 | The handoff queue shows two chats: an angry customer on X and a bulk price enquiry on LinkedIn | F22 handed both over. Each shows the channel, a summary and a suggested reply |
| 4 | The owner replies to the X customer from the unified inbox | F32 sends the reply through the X connector. The Policy Engine checks it first |
| 5 | The LinkedIn enquiry shows "Assist mode: copy this reply and send it in LinkedIn" | LinkedIn messaging access is not approved, so the agent only drafts (see document 16) |
| 6 | Cost screen: WhatsApp message charges, X API credits and AI cost, with a warning at 80% of the X budget | F35 tracks cost per channel |

---

[← Channel: LinkedIn](16_Channel_LinkedIn.md) | [Index](00_README.md) | [Phase 1: Core Agent that Does Real Work →](18_Phase_1_Core_Agent.md)
