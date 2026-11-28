*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

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
| 1 | Nusrat comments "price?" | The adapter's inbound half receives the comment event (F01) |
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
| 1 | Sabbir: "Hi, grey backpack ache?" | The Universal Social Adapter (F30) receives the event and checks the Meta signature named in the WhatsApp profile. The message opens the 24-hour window |
| 2 | Agent: "Assalamu Alaikum! Yes, the grey backpack is available. The price is 1,200 taka. Would you like to order?" | Same agent logic as on Facebook. The Policy Engine (F31) confirms the window is open and sends a normal (non-template) message |
| 3 | Sabbir agrees. The agent asks for his name and address, and only confirms his phone number | F10 already knows the number from WhatsApp, so it is only confirmed |
| 4 | The summary appears with reply buttons Confirm, Edit and Cancel | The adapter turns the choices into reply buttons, as the WhatsApp profile says |
| 5 | Sabbir taps Confirm | The order is saved. The owner is notified, with "Channel: WhatsApp" |
| 6 | Later, Sabbir receives an order update with the tracking ID | The window has closed, so the Policy Engine sends an **approved utility template** instead of a normal message. The WhatsApp charge is recorded (F35) |

---

## Scenario 7: Instagram Comment, then the Same Customer on WhatsApp

**Situation:** Mim comments "price?" on an Instagram reel. A week later she messages the shop on WhatsApp.

| Step | Customer sees | Behind the scenes |
|---|---|---|
| 1 | Mim comments "price?" on a reel | The adapter receives the comment (Instagram profile). F16 detects buying interest |
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
| 4 | The owner replies to the X customer from the unified inbox | F32 sends the reply through the adapter, using the X profile. The Policy Engine checks it first |
| 5 | The LinkedIn enquiry shows "Assist mode: copy this reply and send it in LinkedIn" | LinkedIn messaging access is not approved, so the agent only drafts (see document 17) |
| 6 | Cost screen: WhatsApp message charges, X API credits and AI cost, with a warning at 80% of the X budget | F35 tracks cost per channel |

---

## Scenario 9: A Shop That Runs on Google Sheets and Docs

**Situation:** Farhana runs a small clothing shop. She keeps her products in a Google Sheet, her policies in a Google Doc and her photos in a Drive folder. She connects them to the platform.

| Step | What happens | Behind the scenes |
|---|---|---|
| 1 | Farhana shares her three files with the system and gives each a role (Catalog Sheet, Policy Doc, Images Folder). The dashboard shows "Connected" | F36 tests access. The sync engine reads the sheet and shows a column mapping, with her Bangla headings matched to the system's fields. She confirms it once |
| 2 | The dashboard says "148 products. 3 rows skipped." The report lists the 3 rows with missing prices | F37 cleaned prices like "৳850" and skipped invalid rows. They never reach customers |
| 3 | A customer asks about a kurti. The agent answers with the price and sends the product photo | F09 reads the price from the database copy of her sheet. F40 uses the cached image from her Drive folder. Google is not called during the chat |
| 4 | Farhana changes the kurti price from 1,850 to 1,650 in her sheet | Google notifies the sync engine that the file changed (or the 5-minute schedule picks it up). It re-reads the tab and finds one changed price |
| 5 | Two minutes later, another customer is told 1,650 taka | The database now holds the new price. The reply validator uses it |
| 6 | A customer orders the kurti. A new row appears in the Orders tab of Farhana's sheet | F38 adds the row in a batch with the key ORD-1042, and the Stock "Sold by assistant" column goes up by one |
| 7 | The order is shipped. The same row changes to "Shipped" with the tracking ID | F38 finds the row by its key and updates it. The sheet can be sorted without breaking this |
| 8 | Farhana adds a new section "Gift wrapping" to her Policy Doc | F39 notices the change, re-indexes only that section, and the dashboard says "Knowledge base updated. 1 section changed" |
| 9 | A customer asks about gift wrapping and gets the new answer. A question about a "Private: supplier prices" section gets no answer | F08 answers from the new section. Text under a "Private" heading is never used |
| 10 | A business customer asks for 200 kurtis. The agent captures the lead. Farhana receives a quotation PDF draft by Gmail for approval | F38 adds the lead to the Leads tab. F41 fills her Docs template and makes the PDF. The quotation is sent to her first, never straight to the customer |
| 11 | By mistake, Farhana deletes most of her product rows | The safety check finds that more than 30% of products would disappear. The sync is **held**. The agent keeps using the last good copy, and Farhana gets an alert to fix or approve |

---

## Scenario 10: Adding a New Platform With Only a Profile

**Situation:** The shop owner wants the assistant on a new messaging platform that is not built yet. For this example it is the fictional platform **ExampleChat** (see document 11).

| Step | What happens | Behind the scenes |
|---|---|---|
| 1 | The administrator reads ExampleChat's developer documentation: it uses an API key, a signed webhook, and text messages only | Nothing is built yet. The platform's login, verification, events, send request and rules are noted |
| 2 | The administrator writes a **channel profile** of about 30 lines | The profile says: login by API key, verify with a shared secret, map each incoming message to the common message format, send text with one request, no buttons, no reply window, free of charge |
| 3 | The profile runs through the **test kit** against a mock ExampleChat | The tests check verification, receive, duplicates, sending, errors and fallback. A failed test points to the line in the profile to fix |
| 4 | A real test account is connected, and messages run in **Approval mode** | A human approves each reply in the unified inbox (F32) before it is sent |
| 5 | The owner opens **Add a channel** in the dashboard and sees ExampleChat in the list, with its own setup steps | The connection wizard (F34) builds its steps from the profile. No new screen was written |
| 6 | A customer on ExampleChat asks: "Price of the black polo?" The agent answers in the shop's style, with a greeting | The same agent, Policy Engine and order engine are used. The adapter code and the agent code were **not changed** |
| 7 | The order summary shows "Reply 1 to confirm, 2 to edit, 3 to cancel" | The profile says buttons are not supported, so the adapter uses numbered text options |
| 8 | The dashboard now shows ExampleChat next to the other channels, with its health, costs and results | One adapter, one dashboard. The channel breakdown (F26) includes it automatically |

---

[← Google Sheet and Doc Templates](19_Google_Sheet_and_Doc_Templates.md) | [Index](00_README.md) | [Phase 1: Core Agent that Does Real Work →](21_Phase_1_Core_Agent.md)
