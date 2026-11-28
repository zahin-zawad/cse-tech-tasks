*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v7*

# 19. Google Sheet and Doc Templates

This document shows how the owner's Google files should look. It gives owners a simple starting point. Owners can also use their **own** column names, because the column mapping (F37) matches them to the system's fields.

**Colors:** tabs and columns **written by the assistant** have a colored header ("Written by the assistant. Please do not edit."). Tabs the **owner edits** have a plain header.

---

## 19.1 Sheets the Owner Edits (read by the system)

### Products tab

| Column | Example | Notes |
|---|---|---|
| Product ID | P001 | Optional. Helps match images and orders |
| Product Name | Black Polo | Required |
| Other Names | কালো পোলো, black polo, kalo polo | Optional. Bangla, English and Banglish |
| Category | T-shirts | Optional |
| Price | 850 | Required. A number |
| Description | Cotton polo, regular fit | Optional |
| Image | P001.jpg or a Drive link | Optional (F40) |
| Active | Yes | Yes or No |

### Variants and Stock tab

| Column | Example | Notes |
|---|---|---|
| Product ID or Name | P001 | Required |
| Size | M | Optional |
| Color | Black | Optional |
| Stock | 12 | Required. A whole number |
| Sold by assistant | 3 | **Written by the assistant** in stock mode A (F37) |

### Offers tab

| Column | Example | Notes |
|---|---|---|
| Offer Name | Weekend Sale | Required |
| Product ID or Name | P001 (or All) | Required |
| Discount Type | Percent, Amount or New price | Required |
| Value | 15 | Required |
| Free Delivery | Yes | Optional |
| Minimum Order | 1500 | Used with free delivery |
| Zone | Outside Dhaka | Optional |
| Start Date | 2026-12-05 | Required |
| End Date | 2026-12-07 | Required |
| Active | Yes | Yes or No |

### Delivery Rules tab

| Column | Example | Notes |
|---|---|---|
| Zone | Inside Dhaka | Required |
| Districts or Areas | Dhaka, Savar, Gazipur | Required |
| Charge | 80 | Required |
| Free Delivery Above | 2000 | Optional |

---

## 19.2 Sheets the Assistant Writes (F38)

### Orders tab

| Column | Example |
|---|---|
| Order ID | ORD-1042 |
| Date | 2026-12-01 14:20 |
| Channel | WhatsApp |
| Customer Name | Rafiq |
| Phone | 017XXXXXXXX (can be masked) |
| Address | House 12, Road 5, Mirpur, Dhaka |
| Items | 1 x Black Polo (M) |
| Subtotal | 850 |
| Delivery Charge | 80 |
| Total | 930 |
| Payment Method | Cash on Delivery |
| Payment Status | COD |
| Order Status | Confirmed |
| Call Result | Confirmed |
| Courier and Tracking ID | Steadfast, TRK12345 |

### Leads tab

| Column | Example |
|---|---|
| Lead ID | L-0207 |
| Date | 2026-12-01 |
| Channel | LinkedIn |
| Company | ABC Traders |
| Contact | Mr. Hasan |
| Phone or Email | hasan@abc.com |
| Product and Quantity | 200 x Black Polo |
| Status | New, Quoted or Won |

### Lost Sales tab

| Column | Example |
|---|---|
| Date | 2026-12-01 |
| Channel | Instagram |
| Customer (masked) | Mim A. |
| Product | Linen Kurti |
| Reason | Price |
| Price Shown | 1850 |
| Delivery Charge Shown | 80 |
| Agreed to Alerts | No |
| Win-back Status | Not eligible |

### Unanswered Questions tab

| Column | Example |
|---|---|
| Date | 2026-12-01 |
| Channel | Facebook |
| Question | Do you have gift wrapping? |
| Times Asked | 4 |
| Status | Add to FAQ |

### Daily Summary tab (used by Looker Studio)

| Column | Example |
|---|---|
| Date | 2026-12-01 |
| Channel | WhatsApp |
| Conversations | 64 |
| Orders | 11 |
| Average Reply Time (seconds) | 6 |
| Handoffs | 3 |
| AI Cost | 0.42 |
| Platform Cost | 0.80 |

---

## 19.3 Google Docs Templates

### Policy Doc

Use these headings. Anything under a heading that starts with **Private** is never used.

```
Return Policy
Exchange Policy
Delivery Time
Payment Methods
Warranty
Contact and Business Hours
Private: Notes for the team
```

### FAQ Doc

Use a heading for each topic, or write questions and answers in pairs:

```
Q: Do you deliver outside Dhaka?
A: Yes. It takes 3 to 5 days and the charge is 130 taka.
```

### Product Details Doc

One heading for each product or product group, followed by the long description, the size chart (a table is fine), and care instructions.

### Style Guide Doc

```
Tone and Length         (for example: short and casual, or detailed and formal)
Greeting                (for example: Assalamu Alaikum)
Address Style           (Vaiya / Apu, Sir / Madam, or name only)
Sample Replies          (3 to 5 replies written in the shop's own voice)
Words to Use
Words to Avoid
Closing Line
```

The system builds a draft style from this Doc and shows it to the owner for approval (F39, F29).

### Invoice or Quotation Template

Normal Google Doc with placeholders. The system replaces them when it makes a PDF (F41).

| Placeholder | Filled with |
|---|---|
| `{{shop_name}}` | The shop's name |
| `{{customer_name}}` | The customer's name |
| `{{customer_address}}` | The delivery address |
| `{{order_id}}` | The order or quotation number |
| `{{date}}` | The date |
| `{{items_table}}` | A table of items, quantity and price |
| `{{subtotal}}`, `{{delivery_charge}}`, `{{total}}` | The amounts |
| `{{payment_method}}` | The payment method |
| `{{notes}}` | Notes, such as quotation validity |

---

## 19.4 Tips for Owners

- Keep one header row at the top of each tab, and do not leave blank columns inside the table.
- Do not merge cells in the data area.
- Write prices as plain numbers. Signs such as "৳" and "Tk" are cleaned automatically.
- Update the Stock number after packing all pending orders, so the stock count stays correct (F37).
- Do not edit the tabs and columns written by the assistant.
- If a sync is held with a message about too many changes, check the sheet for accidental deletions, then approve or fix it.

---

[← Features: Google Integration](18_Features_Google_Integration.md) | [Index](00_README.md) | [End-to-End Scenarios →](20_End_to_End_Scenarios.md)
