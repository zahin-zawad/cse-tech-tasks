*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# 4. Features: Conversation and Understanding

These features decide how the system receives, understands and answers customer messages.

---

## F01 Message Gateway: the Inbound Half of the Universal Social Adapter (Phase 1)

**Purpose:** Receive every Facebook event safely and quickly, so no message is lost and fake requests are rejected.

**Trigger:** A platform (Facebook, Instagram, WhatsApp, X, LinkedIn or any other) sends an event to the webhook (new message, attachment, comment or button click).

**How it works**
1. The Rust adapter receives the HTTPS request. The gateway is not a separate program. It is the inbound half of the Universal Social Adapter (F30).
2. It finds the channel's profile (F43) and checks the request with the verification method the profile names. For the Meta channels (Messenger, Instagram and WhatsApp) this is the `X-Hub-Signature-256` header with the Meta app secret. Other channels use the method their platform requires (see documents 13 to 17). If the check fails, the request is rejected.
3. It reads the events using the profile's mapping rules, tags each one with its channel (Facebook, Instagram, WhatsApp, X, LinkedIn or any other) and its shop, converts it to the common message format, and ignores messages sent by the shop itself and delivery receipts.
4. It checks the message ID in Redis. If the ID was seen before, the event is a duplicate and is dropped. New IDs are stored for 24 hours.
5. It applies rate limiting per customer (default: 20 messages per minute) to stop spam.
6. It replies "200 OK" to Facebook right away and puts the event in the queue.

**Rules**
- The gateway never does AI work. Its only job is to accept, check and queue.
- Target response time to Facebook: under 100 milliseconds.
- It also answers Facebook's one-time webhook verification request during setup.

**If something goes wrong:** If the queue is unavailable, the gateway writes events to a local buffer and retries, and an alert is sent to the administrator.

---

## F02 Language Understanding (Phase 1 basic, Phase 2 full)

**Purpose:** Understand customers the way they really write, in Bangla script, English and Banglish (Bangla written in English letters).

**Trigger:** Every text message, voice transcript or comment.

**How it works**
1. In Gemini call 1, the message is analysed together with a short glossary of common Banglish shopping words.
2. Gemini returns the language (`bn`, `en`, `banglish` or `mixed`) with the other details.
3. Bangla digits (১২৩) are converted to normal digits so quantities and phone numbers can be read.
4. The reply language is chosen by the rule below.

**Rules**
- Bangla message: reply in Bangla.
- English message: reply in English.
- Banglish message: reply in Bangla script by default. The owner can change this to "mirror the customer" in settings.
- A thumbs-up sticker or emoji is treated as friendly acknowledgement. It is **never** treated as order confirmation. Orders need an explicit confirm button or a clear "yes".
- The style of the reply (length, tone, greeting and address title) is defined in F29.

**Examples of Banglish the system must understand**

| Customer writes | Meaning |
|---|---|
| dam koto? | What is the price? |
| ache? / nai? | Is it available? / Is it out? |
| lagbe | I want it |
| nibo / kinbo | I will take it / I will buy |
| delivery charge koto? | How much is the delivery charge? |
| kobe pabo? | When will I receive it? |

**If something goes wrong:** If the language or meaning is unclear, the agent asks a short clarifying question in the customer's likely language.

---

## F03 Intent and Detail Extraction (Phase 1)

**Purpose:** Turn a free-text message into a clear, structured request that code can act on.

**Trigger:** Every customer message, after any voice, image or document conversion.

**How it works**
1. Gemini call 1 receives the latest message, the last few messages and the current order state.
2. It returns structured data like this:

```json
{
  "language": "banglish",
  "intents": ["price_query", "stock_query"],
  "product_text": "black polo",
  "variant": { "size": "M", "color": "black" },
  "quantity": null,
  "sentiment": "neutral",
  "confidence": 0.92
}
```

3. Code checks the data. A message can contain more than one intent, and each is handled in order.

**Intents the system understands**

| Intent | Meaning |
|---|---|
| greeting | Hello, salam, hi |
| product_info | Details about a product |
| price_query | Asking for price |
| stock_query | Asking for availability, size or color |
| order_intent | Wants to buy |
| order_details | Gives name, phone, address, quantity or variant |
| order_confirm / order_cancel / order_edit | Confirms, cancels or changes an order |
| delivery_query | Delivery charge or delivery time |
| payment_query | Payment methods |
| policy_query | Return, exchange, warranty and other policies |
| order_status | Where is my order? |
| discount_request | Asking for a lower price |
| complaint | Unhappy or problem report |
| human_request | Wants to talk to a person |
| other | Anything else |

**Rules**
- If `confidence` is below 0.6, the agent asks a clarifying question instead of guessing.
- If two clarifying questions in a row fail, the chat is handed to a human (see F22).
- Details such as product, size and quantity are checked by code against the real catalog before use.

**Example:** "Black polo size M ache? dam koto?" gives intents `stock_query` and `price_query`. Code checks stock and price, then the reply answers both.

---

## F04 Conversation Memory and Returning Customers (Phase 2)

**Purpose:** Make the agent remember the conversation and recognise returning customers, so it feels human and saves the customer from repeating details.

**Trigger:** Every message, and the start of a new chat from a known customer.

**How it works**
1. **Current state:** the product, variant, quantity and order details collected so far are stored as structured data.
2. **Recent messages:** the last 10 messages are given to Gemini.
3. **Summary:** older messages are condensed into a short summary so cost and delay stay low.
4. **Returning customers:** the customer is recognised by their account ID on the channel (Facebook sender ID, Instagram ID, WhatsApp number, X ID or LinkedIn ID). The same person on several channels is linked into one customer profile (F33). If they ordered before, the agent offers the saved details: "Last time you used this address: ... Send to the same address?"

**Rules**
- A chat with no activity for 24 hours is closed. A new message starts a fresh chat, but the customer profile is kept.
- Saved name, phone and address come only from details the customer gave for an order.
- A customer can ask to delete their data (see F24).

**If something goes wrong:** If memory cannot be loaded, the agent asks again for the missing details instead of guessing.

---

## F05 Voice Message Understanding (Phase 2)

**Purpose:** Let customers who prefer speaking get answers, because many people send voice messages in Bangla.

**Trigger:** The customer sends a voice message on Messenger.

**How it works**
1. The audio file is downloaded from Facebook.
2. Gemini converts the speech to text and detects the language.
3. The text goes through the normal pipeline (F03 onward).
4. For order-critical details such as phone number, address and quantity, the agent repeats what it heard and asks the customer to confirm: "I heard: 01712345678. Is this correct?"
5. The audio file is deleted after processing. Only the transcript is kept.

**Rules**
- Maximum length: 2 minutes (default).
- Replies are sent as text.
- Voice messages are supported where the channel delivers them (Messenger, Instagram and WhatsApp). Other channels are handled in their own documents.

**If something goes wrong:** If the audio is unclear or empty, the agent says it could not hear properly and asks the customer to repeat or type the message.

---

## F06 Image Understanding (Phase 2)

**Purpose:** Let customers send a photo of a product and find the matching item in the catalog.

**Trigger:** The customer sends an image.

**How it works**
1. Gemini first classifies the image: product photo, payment screenshot, address or note screenshot, or other.
2. For a **product photo**, Gemini describes it (type, color, pattern, visible text or logo).
3. The system compares the description and image with the catalog (names, descriptions and product images, which can come from the owner's Google Drive, F40) and finds the top 3 matches with a score.
4. If the top match is strong, the agent asks: "Is this the one? Black Polo, 850 taka." If scores are close, it shows the top 3 and asks the customer to choose.
5. If no match is strong enough, the agent says it could not find it and offers to connect a human.
6. Payment screenshots go to F07.

**Rules**
- The agent never assumes a match. The customer always confirms.
- Only products in the shop's catalog can be matched.

---

## F07 Document and Screenshot Understanding (Phase 2)

**Purpose:** Read files and screenshots that customers send, mainly payment receipts, so the shop does not need to check them by hand.

**Trigger:** The customer sends a PDF, file or screenshot classified as a document.

**How it works**
1. Gemini identifies the type: payment receipt, address or note, product detail sheet, or other.
2. For a **payment receipt**, it extracts the transaction ID (TrxID), amount, sender number, payment method and time.
3. Code compares the amount with the order total and checks that the TrxID has not been used before.
4. The result is passed to the payment feature (F12).
5. For an **address or note** screenshot, the text is extracted and shown to the customer for confirmation.

**Rules**
- Extracted values are always shown back to the customer or owner for confirmation before use.
- Unsupported file types get a polite message and the chat is handed to a human if needed.

---

## F08 Shop Knowledge Base (Phase 2)

**Purpose:** Answer questions about return policy, delivery time, warranty and other shop information correctly from the shop's own content.

**Trigger:** The intent is `policy_query`, or a question cannot be answered from product data.

**How it works**
1. The owner adds policies and FAQs as text, as a document, or as Google Docs that stay in sync automatically (F25, F39).
2. The content is split into small pieces and stored with vector search in PostgreSQL.
3. For a question, the system finds the 3 most relevant pieces.
4. Gemini call 2 answers using only those pieces.
5. If no piece is relevant enough, the agent does not answer. It says it will check with the shop team, hands over to a human (F22), and records the question as "unanswered".

**Rules**
- The agent never invents a policy.
- Unanswered questions appear in the owner's analytics so the owner can add the missing answer.

**Example:** "Exchange kora jabe?" The agent finds the shop's exchange policy and answers: "Yes, you can exchange within 3 days if the product is unused."

---

## F09 Product, Price and Stock Answers (Phase 1)

**Purpose:** Give correct product, price and availability information, the most common customer need.

**Trigger:** Intents `product_info`, `price_query` or `stock_query`.

**How it works**
1. **Find the product.** Code matches the customer's words to the catalog in this order: exact name or alias (Bangla, English or Banglish), then close spelling match, then meaning match. If several products fit, the agent asks which one. If none fit, it says so and suggests similar products.
2. **Read the facts** from the database: price, active offer price, variants and stock for the chosen size and color. If the owner keeps this data in Google Sheets, the sync engine has already copied it to the database (F37).
3. **Build the facts** as structured data, for example `{product, variant, price: 850, in_stock: true}`.
4. Gemini call 2 writes the reply from these facts.
5. The validator checks that the numbers in the reply match the facts.

**Rules**
- Stock is shown as "available", "only a few left" (default: 5 or fewer) or "out of stock". Exact stock counts are not shown to customers.
- If an offer is active, the reply shows the offer price.
- If the item is out of stock, the agent suggests similar available products and offers a restock alert (F20).

**Example**
> **Customer:** "Black polo size M ache? dam koto?"
> **Agent:** "Yes, the black polo in size M is available. The price is 850 taka. Would you like to order it?"

---

[← Feature Index](03_Feature_Index.md) | [Index](00_README.md) | [Features: Orders and Fulfilment →](05_Features_Orders_and_Fulfilment.md)
