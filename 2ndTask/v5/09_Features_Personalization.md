*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# 9. Features: Personalization

This feature lets each business make the agent sound like its own brand. It controls how long and how formal the replies are, how customers are addressed (Vaiya, Apu, Sir or Madam), and how every conversation starts.

---

## F29 Personalization: Reply Style, Address Titles and Greetings (Phase 1, 2 and 4)

**Purpose:** Different businesses want different voices. One shop wants long, professional replies. Another wants short, casual, human-like replies. The agent must also address each customer politely, in chat and on the confirmation call, and must always start a conversation with a greeting.

**Trigger:** Every reply, every phone call and every message the system starts on its own.

The feature has three parts:

| Part | What it controls |
|---|---|
| A. Reply Style | Length, tone, emoji and wording of replies |
| B. Address Titles | Vaiya or Apu, Sir or Madam, or the customer's name |
| C. Greetings | How every chat and call begins |

---

### Part A: Reply Style

**What the owner chooses** (in setup, F25)

| Setting | Options | Default |
|---|---|---|
| Style preset | Casual and Short, Friendly and Balanced, Professional and Detailed | Friendly and Balanced |
| Reply length | Short (1 to 2 sentences), Medium (3 to 4 sentences), Long (5 to 8 sentences, well organised) | Set by the preset |
| Emoji | None, Light (at most 1 per message), Frequent | Set by the preset |
| Agent name | Optional, for example "Mim from [Shop name]" | None |
| Sample replies | The owner pastes 3 to 5 replies written in the shop's own voice | None |
| Words to use or avoid | For example always say "জি", never use slang | None |
| Closing line | Optional sign-off, for example "Thank you for shopping with us" | None |

**The three presets**

| Preset | Length | Tone | Emoji | Address style |
|---|---|---|---|---|
| Casual and Short | Short | Friendly, like a helpful shop assistant | Light | Vaiya / Apu |
| Friendly and Balanced | Medium | Warm and clear | Light | Vaiya / Apu |
| Professional and Detailed | Long | Formal and complete | None | Sir / Madam |

The owner can change any setting after choosing a preset.

**The same answer in two styles**

> **Customer:** "Black polo size M ache? dam koto?"
>
> **Casual and Short:**
> "আসসালামু আলাইকুম ভাইয়া! জি আছে 😊 দাম ৮৫০ টাকা। অর্ডার করবেন?"
> *(Assalamu Alaikum Vaiya! Yes, it is available. The price is 850 taka. Would you like to order?)*
>
> **Professional and Detailed:**
> "আসসালামু আলাইকুম স্যার। [Shop name]-এ আপনাকে স্বাগতম। আপনার জানতে চাওয়া Black Polo (M সাইজ) বর্তমানে স্টকে রয়েছে। এর মূল্য ৮৫০ টাকা। আপনি চাইলে আমি এখনই আপনার অর্ডারটি গ্রহণ করতে পারি।"
> *(Assalamu Alaikum Sir. Welcome to [Shop name]. The Black Polo in size M you asked about is currently in stock. The price is 850 taka. If you wish, I can take your order right now.)*

Both replies contain the same facts. Only the wording changes.

**How it works**
1. The owner picks a preset and adjusts the settings. The system saves them as the shop's **Style Profile**.
2. In Gemini call 2 (the reply call), the Style Profile is added to the instructions: length limit, tone, emoji rule, sample replies, and words to use or avoid.
3. The validator (F21) checks the reply before it is sent: the length is within the limit, the greeting is present when required, the emoji rule is followed, and all facts match the database.
4. If a check fails, the reply is regenerated once with a reminder. If it fails again, the system uses the shop's **template** for that situation (greeting, price answer, order summary lead-in, handoff message, fallback message) written in the same style.
5. Before going live, the owner can type a sample customer message and **preview** the reply in the chosen style (a test script in Phase 1, the owner panel from Phase 3).

**Rules**
- Style changes **wording only**. It can never change a price, policy, discount limit or any other fact or rule.
- Structured messages, such as the order summary with prices, keep their layout. Style affects the opening and closing lines.
- Automatic messages (follow-ups, alerts, handoff and fallback messages) and the call script also follow the Style Profile.
- Messages to the owner (F15) use a fixed plain style and are not affected.
- In Bangla the agent always uses the respectful "আপনি", never "তুমি" or "তুই", in every style.
- A customer can ask for a different length, for example "short e bolen" or "details e bolen". The agent adapts for that chat within the owner's limits.
- The agent avoids repeating the same sentence again and again, so the conversation feels natural.
- If a customer sincerely asks "Are you a bot?", the agent answers honestly that it is the shop's AI assistant.

---

### Part B: Address Titles

**Purpose:** Address each customer in the way Bangladeshi customers expect, so the agent sounds respectful and human.

**What the owner chooses**

| Address style | Male customer | Female customer | Gender unknown |
|---|---|---|---|
| **Vaiya / Apu** (default for the casual presets) | Vaiya (ভাইয়া) | Apu (আপু) | Name only, with polite "আপনি" |
| **Sir / Madam** (default for the professional preset) | Sir (স্যার) | Madam (ম্যাডাম) | Name only, with polite "আপনি" |
| **Name only** | Name | Name | Name |

**How the system decides the title**
1. **Customer's own choice (highest priority):** if the customer says what to call them, or corrects the agent ("ami apu, vaiya na"), that title is used immediately and saved.
2. **Saved title:** if the customer was addressed before, the saved title is used.
3. **Name-based guess:** Gemini looks at the customer's first name (from the Facebook profile or the name given in the order) and returns a likely title with a confidence score. The title is used **only if confidence is high** (default: 0.85 or more).
4. **Otherwise:** the agent uses neutral wording, the customer's name and "আপনি", and does not guess.

**Asking the customer**
- When the agent collects the customer's name during an order (F10), and the title is not yet known with enough confidence, it asks one short friendly question, for example: "What should I call you, Vaiya or Apu?"
- This must be settled **before the confirmation call** (F13), because the call must use the right title.
- If the customer does not answer, the call uses neutral wording.

**Rules**
- The system stores only the **address title**, not the customer's gender. It is used only to choose words.
- Name-based guesses can be wrong, especially for names used by both men and women. That is why a confidence threshold, a question and instant correction exist.
- The title is used in the greeting and then at most once in a message, not in every sentence. Example: "Rafiq Vaiya", "Tania Apu".
- If a customer seems uncomfortable with the title, the agent apologises once, switches to neutral wording and saves the change.

---

### Part C: Greetings

**Purpose:** Every conversation starts politely, in the same way every time.

**The rule:** The agent **always starts with a greeting**.

| Situation | Greeting required? |
|---|---|
| First reply of every new chat (new customer, or a chat after the 24-hour session reset) | Yes |
| Start of every confirmation call | Yes |
| Every automatic message that opens a new chat (win-back alerts, restock alerts, comment private reply) | Yes |
| Later replies in the same chat, and reminders inside an open chat | No, to avoid repeating |
| Public replies to comments (any channel) | A short polite opening only. Titles are used in public only if the owner allows it |

**What the greeting contains**
1. A greeting phrase chosen by the owner: "Assalamu Alaikum" (default), "Hello" or "Hi", "Namaskar", or a time-based greeting such as "Good morning".
2. The customer's title or name, when known (Part B).
3. In the casual presets, a very short line such as "How can I help?". In the professional preset, the shop name and a welcome line.

**How it works**
1. The greeting and the answer are sent **together in the same message**. The customer never has to wait for a separate greeting before getting the answer.
2. If the customer greets first with "Assalamu Alaikum", the agent replies with "Wa Alaikum Assalam" and then continues.
3. If the customer starts with a question and no greeting, the agent still greets first and then answers.
4. **Enforced by code, not by AI:** if the chat is new and the reply has no greeting, the system adds the shop's greeting template before sending. The agent cannot skip it.

---

### Style on Each Channel

The same agent can sound different on each channel, because customers expect different things.

- The owner can set a **style override per channel** (F25). Example: Casual and Short on WhatsApp and Instagram, Friendly and Balanced on Facebook, Professional and Detailed on LinkedIn.
- If no override is set, the shop's main Style Profile is used.
- Greeting and respectful wording rules are the same on every channel.
- Address titles (Vaiya, Apu, Sir, Madam) follow the owner's address style. On LinkedIn, where customers are usually businesses, the recommended setting is "Sir / Madam" or "Name only".
- Length is also limited by the channel. Public replies on X and in comments are kept short.

### The Calling Agent (F13) and Personalization

The confirmation call follows all three parts.

| Part | On the call |
|---|---|
| Greeting | The call begins with a greeting |
| Title | The agent uses the customer's title (Vaiya, Apu, Sir or Madam), or neutral wording when it is not known |
| Style | A casual shop gets a short, friendly script. A professional shop gets a longer, formal script |
| Identity | The agent always says it is the shop's automated assistant |
| Voice | The owner chooses the voice (male or female) offered by the voice provider |

**Example: casual shop, female customer**
> "আসসালামু আলাইকুম আপু। আমি [Shop name]-এর স্বয়ংক্রিয় সহকারী বলছি। আপনার ৯৩০ টাকার অর্ডারটি কি নিশ্চিত করবেন?"
> *(Assalamu Alaikum Apu. I am the automated assistant of [Shop name]. Do you confirm your order of 930 taka?)*

**Example: professional shop, male customer**
> "আসসালামু আলাইকুম স্যার। আমি [Shop name]-এর স্বয়ংক্রিয় সহকারী বলছি। আপনার অর্ডারের বিবরণ হলো: একটি Black Polo, মোট মূল্য ৯৩০ টাকা, ঠিকানা মিরপুর, ঢাকা। আপনি কি এই অর্ডারটি নিশ্চিত করছেন?"
> *(Assalamu Alaikum Sir. I am the automated assistant of [Shop name]. Your order details are: one Black Polo, total 930 taka, address Mirpur, Dhaka. Do you confirm this order?)*

---

**If something goes wrong**
- If the title is uncertain, the agent uses neutral wording instead of guessing.
- If the style check fails twice, the shop's template for that situation is used.
- If the Style Profile cannot be loaded, the default preset (Friendly and Balanced) is used and the administrator is alerted.

---

[← Features: Owner Dashboard and Analytics](08_Features_Owner_Dashboard_and_Analytics.md) | [Index](00_README.md) | [Features: Multi-Channel Platform →](10_Features_Multi_Channel_Platform.md)
