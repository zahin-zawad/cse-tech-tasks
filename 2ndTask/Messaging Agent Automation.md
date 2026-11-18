# Messaging Agent Automation – Project Plan

**Task:** AI Messaging Agent for a Facebook Page (Product Price Replies)
**Internship at:** CSE TECH, Daffodil International University
**Supervisor:** Md Shoaib Khan
**Submitted by:** Zahin Zawad, ID: 232-15-886
**Date:** 18 Nov 2026

---

## 1. Task

My supervisor has asked me to plan a messaging agent automation for my Facebook Page. When someone sends a message to the page, the system will understand what the person wants and reply automatically. Most users ask for the **price of a product**, so the agent will find the price from a Google Sheet and reply in a natural, human-like way.

This report explains the plan **before implementation**.

## 2. Tools to Be Used

- **n8n** – to build the automation workflow
- **Facebook Page and Meta for Developers** – to receive messages through a webhook
- **Webhook node (n8n)** – to start the workflow when a new message arrives
- **Google Gemini** – to understand the message and write the reply
- **Google Sheets** – to store product names and prices
- **Google OAuth** – to connect n8n with Google Sheets

## 3. Planned Workflow

```
User messages the Facebook Page
        ↓
Webhook Trigger (n8n) receives the message
        ↓
Gemini node: understands what the user wants
        ↓
Google Sheets node: finds the product name and price
        ↓
Gemini: writes a friendly, human-like reply
        ↓
Reply is sent back to the user on Messenger
```

**Diagram / Screenshot: Planned workflow in n8n (add after implementation)**

> [ Insert screenshot here ]

## 4. How Each Step Will Work

| Step | Node | What it will do |
|---|---|---|
| 1 | Webhook Trigger | Receives the user's message and sender ID from Facebook |
| 2 | Gemini (analysis) | Reads the message and finds the product the user is asking about |
| 3 | Google Sheets | Searches the sheet for that product and gets its price |
| 4 | Gemini (reply) | Writes a polite, natural message using the price found |
| 5 | Send reply | Sends the message back to the user on the Facebook Page |

## 5. Google Sheet Structure

The sheet will have a list of all products and their prices. Example:

| Product Name | Price |
|---|---|
| Product A | 500 |
| Product B | 1200 |
| Product C | 850 |

## 6. Example Conversation

**User:** "Hi, how much is Product B?"

**Agent:** "Hello! Product B is available for 1200 taka. Let me know if you would like to order it."

## 7. Implementation Steps (Plan)

1. Create or use a Meta for Developers app and connect it to the Facebook Page.
2. Add a Webhook node in n8n and connect it to the Meta app so page messages reach n8n.
3. Add the Gemini node and write a prompt so it understands the user's request.
4. Add the Google Sheets node (OAuth) and connect it to the product price sheet.
5. Use Gemini again to write a friendly reply from the price data.
6. Send the reply back to the user through the Facebook Page.
7. Test with different messages and fix any problems.

## 8. Expected Result

When a user messages the page asking about a product price, the agent will reply automatically within a few seconds with the correct price in a natural tone. No manual work will be needed from the page admin.

## 9. Possible Challenges

| Challenge | Plan |
|---|---|
| User spells the product name differently | Tell Gemini to match similar names |
| Product is not in the sheet | Reply politely that it is not available or ask for more details |
| User asks something other than price | Reply politely and say a team member will help |
| Facebook token expires | Renew the token when needed |

## 10. Conclusion

This plan shows how a messaging agent can be built with n8n, Gemini and Google Sheets to answer price questions on a Facebook Page automatically. After my supervisor's approval, I will start the implementation and add screenshots and results to the final report.

I thank my supervisor, Md Shoaib Khan, for his guidance.