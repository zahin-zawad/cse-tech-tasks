*AI Sales and Order Assistant for Facebook Businesses · Project Documentation v3.1*

# 1. Introduction

## 1.1 Problem Statement

Many small businesses in Bangladesh sell through Facebook Pages and Messenger, a model commonly called F-commerce. Page owners face the same challenges every day:

- Customers ask the same questions repeatedly, such as price, stock, delivery and payment.
- Replies are slow or missing outside working hours, so customers move to other sellers.
- Orders are collected manually in chats and notebooks, which leads to mistakes.
- Fake or unconfirmed orders cause failed deliveries and wasted courier costs.
- Customers who leave because of a high price or delivery charge are never followed up.
- Owners have no clear data about what customers ask, what sells, and why sales are lost.

## 1.2 Proposed Solution

An AI-powered sales and order assistant for Facebook Pages. It works as a digital sales representative that is available 24/7 and uses only the shop's real data. In simple terms, the system does the following:

| Area | What the system does |
|---|---|
| Understands customers | Reads messages in Bangla, English and Banglish, and interprets voice messages, images and documents |
| Answers with real data | Gives price, stock and policy answers taken from the shop's database and knowledge base, never from AI guessing |
| Handles the whole order | Takes the order, calculates the delivery charge, handles payment, confirms by phone call, books the courier and notifies the owner |
| Brings customers back | Replies to comments and moves them to Messenger, follows up on abandoned orders and restocks, and alerts customers when a price offer or free delivery removes the reason they left |
| Stays safe | Hands the chat to a human when things go wrong, and logs everything |
| Gives visibility | Shows the owner analytics and tracks response time, AI cost per conversation and failure rate |

Every feature is defined step by step in documents 04 to 09. The complete list is in [03_Feature_Index.md](03_Feature_Index.md).

## 1.3 Who Uses the System

| Person | Role | What they do with the system |
|---|---|---|
| **Customer** | Buys from the shop | Chats on Messenger, comments on posts, sends voice, photos and receipts, answers the confirmation call |
| **Shop Owner** | Owns the Facebook Page | Sets up products, prices, delivery rules, offers and policies; receives notifications; views analytics |
| **Shop Staff (human agent)** | Works for the owner | Takes over chats when the agent hands over; verifies payments; handles special cases |
| **System Administrator** | The developer (Zahin Zawad) | Deploys, monitors and maintains the system |

## 1.4 Goals

| Goal | Description |
|---|---|
| Fast replies | Reply to customers within seconds, 24/7 |
| Accurate data | Prices and stock always come from the database, never from AI guessing |
| More orders | Capture orders automatically from chats, calls and comments |
| Fewer bad orders | Confirm orders by chat and phone call to reduce fake orders and returns |
| Recover lost sales | Bring back customers who left because of price or delivery charge |
| Reliability | Handle errors, duplicates and failures safely |
| Real impact | Test with real sellers and measure the results |

## 1.5 Scope

**In scope**
- Facebook Page Messenger and Page post comments.
- Product questions, order taking, delivery charge, payment handling, confirmation calls and courier booking.
- Sales follow-ups, owner notifications, owner setup and analytics.

**Out of scope (for this internship)**
- Instagram and WhatsApp (possible future channels).
- A full online store, accounting or purchasing system.
- Automatic refunds and returns processing.
- Holding or processing customer money. The system only records and checks payments.

**Assumptions and dependencies**
- The shop has a Facebook Page with Messenger enabled.
- The owner can provide the product list, prices, delivery rules and policies.
- A courier merchant account (for example Pathao, Steadfast or RedX) is available.
- The Meta app must pass Meta's review for the required messaging permissions before it can serve real customers. During development, it is tested with Page admins and testers.
- A voice call provider that supports Bangla must be selected for the calling agent.

---

[Index](00_README.md) | [System Overview →](02_System_Overview.md)
