*AI Sales and Order Assistant for Social Media Businesses · Project Documentation v5*

# 17. Features: Google Integration

Many shop owners already keep their product list in **Google Sheets**, their policies in **Google Docs**, and their photos in **Google Drive**. Instead of asking them to move everything to a new system, the platform works with the files they already use.

Seven features (F36 to F42) cover this. The exact sheet and document layouts are in [18_Google_Sheet_and_Doc_Templates.md](18_Google_Sheet_and_Doc_Templates.md).

---

## 17.1 How Google Fits In

| Google tool | What the shop keeps there | What the system does |
|---|---|---|
| **Google Sheets** | Products, prices, stock, offers, delivery rules | Reads them (sync). Writes orders, leads, lost sales and reports back |
| **Google Docs** | Policies, FAQs, product details, brand voice guide, invoice and quotation templates | Reads them for the knowledge base and style. Fills the templates |
| **Google Drive** | Product images, PDF catalogs and price lists | Reads images and files. Stores generated PDFs |
| **Gmail and Google Chat** | The owner's inbox and team chat | Sends owner notifications and daily summaries |
| **Looker Studio** | Reports | Shows dashboards from the report sheets |
| **Sign in with Google** | The owner's and staff's Google accounts | Used to log in to the dashboard |

## 17.2 Design Rules

1. **The owner edits in Google. The agent reads from the database.** A sync engine copies data from Google into the database. The agent never calls Google while answering a customer, so replies stay fast, even if Google is slow or the request limit is reached.
2. **Prices still come only from the database** (F21). The database now holds a checked copy of the owner's sheet.
3. **The last good copy always wins over bad data.** If a sync finds wrong data, the previous good copy stays in use.
4. **Least access.** The system works only with the files the owner chooses, and asks for the smallest permission that works.
5. **The owner stays in control.** The owner sees what changed, can pause sync, and can roll back.

## 17.3 The Sync Engine (shared by F37, F39 and F40)

| Step | What happens |
|---|---|
| 1. Trigger | A change is detected, a schedule runs, or the owner presses **Sync now** |
| 2. Read | The file is read in as few requests as possible (a whole tab at once) |
| 3. Clean | Spaces are trimmed, Bangla digits (১২৩) are converted, and signs such as "৳", "Tk" and commas are removed from prices |
| 4. Check | Each row or section is checked against the rules (see F37 and F39) |
| 5. Compare | The new data is compared with the last good copy. The differences are listed |
| 6. Safety check | If too much would change at once (default: more than 30% of products), the sync is **held** and the owner is asked to approve |
| 7. Publish | Valid changes are saved to the database. Invalid rows are skipped and reported |
| 8. Report | The dashboard shows the time, number of items, and any problems |

**How changes are detected**
- **Push notifications:** Google Drive can notify a webhook when a file changes. Notifications only say that a file changed, not what changed, so the engine re-reads the file. The notification channels expire after some time, so the system renews them.
- **Scheduled sync (safety net):** the engine also syncs on a schedule (default: every 5 minutes), in case a notification is missed or a channel has expired.
- **Manual:** the owner can press **Sync now**.

**Request limits**
- The Google Sheets API has per-minute limits. At the time of writing: 300 read requests and 300 write requests per minute for the whole project, and 60 of each per minute per user. Requests over the limit are refused, and Google says to retry with increasing waiting time. Google has also said that going over the limits may be charged from later in 2026.
- A service account counts as a single user for these limits.
- So the engine reads whole tabs in one request, batches writes, caches results, syncs shops in turns, and retries with increasing waiting time. Customers are never affected, because the agent reads from the database.

---

## F36 Google Connection and Access (Phase 1 basic, Phase 3 full)

**Purpose:** Let the owner connect Google safely, with the least access needed.

**Trigger:** The owner connects Google in the dashboard (F25).

**Two ways to connect**

| Way | How it works | Good for |
|---|---|---|
| **Share with the system** (service account) | The owner shares each file or folder with the system's email address: **Viewer** for files the system only reads, **Editor** for files it writes to | The pilot and small shops. No sign-in or Google approval screens |
| **Sign in with Google** (OAuth) | The owner signs in, allows access, and picks the exact files with Google's file picker. The system gets access only to the files that were picked | Many shops and the final product |

**How it works**
1. The owner chooses a way to connect.
2. The owner picks the files and gives each a **role**: Catalog Sheet, Orders Sheet, Policy Doc, FAQ Doc, Style Guide Doc, Images Folder, Invoice Template.
3. The system tests access to each file and shows the result.
4. The connection status is shown in the dashboard: Connected, Warning or Disconnected (same as F34).
5. **Sign in with Google** can also be used by the owner and staff to log in to the dashboard.

**Rules**
- **Least access:** read-only wherever the system does not need to write. Only the chosen files are used. The system never browses the rest of the owner's Drive.
- Some Google permissions are classed as sensitive or restricted. Using them for many shops needs Google's app verification and, for the most restricted ones, a security assessment. Narrow per-file access avoids most of this. During the pilot, sharing with the system's service account is used. These requirements are re-checked before launch.
- Google tokens are stored encrypted (F24). If the owner disconnects or removes access, the system stops at once and deletes the tokens.
- When Google is disconnected, the last good copy stays in the database until the owner deletes it or changes the data in the dashboard.
- The owner can see which files are connected and what the system does with each.

**If something goes wrong:** If access is removed or a token expires, the status changes to Disconnected, the agent keeps working from the last good copy, and the owner is alerted with a guide to reconnect.

---

## F37 Google Sheets Catalog and Business Data Sync (Phase 1)

**Purpose:** Let the shop keep its products, stock, offers and delivery rules in Google Sheets, and have the agent always use the latest checked data.

**Trigger:** A change in the sheet, a scheduled sync, or **Sync now**.

**What is synced**

| Sheet tab | What it holds | Used by |
|---|---|---|
| Products | Name, other names, category, price, description, image, active | F09, F06 |
| Variants and Stock | Size, color and stock for each product | F09, F10 |
| Offers | Discounts and free-delivery campaigns with dates | F09, F18 |
| Delivery Rules | Zones, charges and free-delivery conditions | F11 |

**How it works**
1. **Use your own sheet or the template.** Owners can use the template (document 18) or their existing sheet.
2. **Column mapping.** The owner's own column names (in Bangla or English) are matched to the system's fields. Gemini suggests the matches from the headings and a few sample rows, and the owner confirms them once. The mapping is saved.
3. The sync engine runs the steps in 17.3.
4. **Checks for each row:**

| Check | Rule |
|---|---|
| Name | Required |
| Price | A number above zero after cleaning |
| Stock | A whole number, zero or more |
| Duplicates | The same product and variant twice is reported, and the first row is used |
| Dates (offers) | The end date is not before the start date |
| Unknown values | An unknown size or zone is reported |

5. **Changes:** new rows add products, edited rows update them. A row that is removed makes the product **inactive** (it is not deleted), so old orders stay valid.
6. **Other names.** An optional column holds other names (Bangla, English, Banglish) for matching (F09). If it is empty, Gemini suggests names and the owner approves them.
7. The dashboard shows the result, for example: "Last sync 10:42. 148 products. 3 rows skipped."

**Stock modes** (the owner chooses one)

| Mode | How it works |
|---|---|
| **A. Owner updates stock** (default) | The Stock number in the sheet is the owner's count. The system never changes it. It counts the orders it confirms after the owner's last update and shows them in a "Sold by assistant" column. **Available = Stock − sold by assistant.** When the owner changes the Stock number, the system treats the new number as the truth and starts counting again |
| **B. Assistant updates stock** | The system updates the Stock cell itself when an order is confirmed or cancelled. Before writing, it checks that the cell still has the value it last read (**check-before-write**). If the owner changed it in between, the system does not write, takes the owner's number, and alerts the owner |

In both modes, the owner is advised to update the Stock number after packing all pending orders, so the count stays right.

**Rules**
- Only the chosen tabs and the header row are read. Formulas are fine (the calculated value is used). Hidden rows are ignored.
- Maximum size: 10,000 rows per tab by default.
- Offers and delivery rules from the sheet follow the same rules as those set in the dashboard.
- If the sheet is changed by mistake (for example, many rows deleted or the price column removed), the safety check holds the sync and asks the owner (17.3).

**If something goes wrong:** The agent keeps using the last good copy. The owner is alerted and can fix the sheet or approve the change.

---

## F38 Google Sheets Write-Back (Phase 1 orders, Phase 4 the rest)

**Purpose:** Give the owner a live record in Google Sheets of orders and results, which many small shops already use to run their business.

**Trigger:** An order is confirmed or changes status, a lead or lost sale is recorded, or the daily summary is made.

**What is written**

| Tab | What it holds | Phase |
|---|---|---|
| Orders | Every order, its channel, payment and delivery status | 1 |
| Leads | Business enquiries (for example from LinkedIn) | 4 |
| Lost Sales | Product, reason, channel and whether the customer agreed to alerts | 4 |
| Unanswered Questions | Questions the agent could not answer | 4 |
| Daily Summary | Conversations, orders, response time, handoffs and cost for each channel and day | 4 |

**How it works**
1. When an event happens, a row is placed in a queue.
2. A worker writes the rows in **batches** (default: every 30 seconds, or after 20 rows) in a single request, to respect Google's limits.
3. Every row has a **unique key** (for example, the Order ID). The system checks the key before adding, so a row is never written twice, even after a retry.
4. When an order's status changes (confirmed, shipped, delivered, cancelled), the **same row** is updated. The system finds the row by its key, and checks the key again just before writing, so it still works if the owner sorted the sheet or inserted rows.

**Rules**
- The system writes **only to its own tabs and columns**, marked with a colored header ("Written by the assistant. Please do not edit."). It never changes the owner's other cells.
- Owners are advised to protect these ranges in Google Sheets.
- **Privacy option:** the owner can choose to mask phone numbers or leave out addresses in the sheet.
- The database is always the master record. The sheet is a mirror, so nothing is lost if the sheet is deleted.

**If something goes wrong:** Writes are retried with increasing waiting time and kept in a queue on disk. If the sheet is deleted or access is lost, the owner is alerted. The dashboard has a **Rebuild sheet from database** button.

---

## F39 Google Docs Knowledge Base and Style Guide Sync (Phase 2)

**Purpose:** Let owners write their policies, FAQs and brand voice in Google Docs, and have the agent use them without any copying.

**Trigger:** A change in a connected Doc, a scheduled sync, or **Sync now**.

**Document roles**

| Role | What the owner writes | Used by |
|---|---|---|
| **Policy Doc** | Return, exchange, warranty, delivery time, payment methods, contact hours | F08 |
| **FAQ Doc** | Common questions and answers | F08 |
| **Product Details Doc** | Long descriptions, size charts, care instructions | F08, F09 |
| **Style Guide Doc** | Tone, length, greeting, address style, sample replies, words to use and avoid, closing line | F29 |

**How it works**
1. The system reads the Doc and splits it by **headings**. Each section becomes one piece of the knowledge base, with its heading as the title.
2. The pieces are indexed for search (F08).
3. When the Doc changes, the system compares sections, and re-indexes **only the changed ones**.
4. The dashboard reports, for example: "Knowledge base updated. 2 sections changed."
5. For the **Style Guide Doc**, the system reads the headings (Tone, Sample replies, Words to use, Words to avoid and so on), builds a draft Style Profile, and shows a **preview** to the owner. The profile is used only after the owner approves it (F29).

**Rules**
- Only the published text is used. Comments and unaccepted suggestions are ignored.
- Anything under a heading that starts with **"Private"** is never used.
- Tables (such as size charts) are read as text.
- If a Doc suddenly becomes empty or much shorter (default: more than 50% removed), the sync is held and the owner is asked.
- Docs can be written in Bangla, English or both.

**If something goes wrong:** The previous knowledge base stays in use, and the owner is alerted.

---

## F40 Google Drive Files: Images, Catalogs and Price Lists (Phase 2)

**Purpose:** Use the product photos and PDF price lists that shops already keep in Google Drive.

**Trigger:** The owner connects a folder or file, or a file in a connected folder changes.

**Product images**
1. The owner picks a folder of product images.
2. Images are matched to products by file name (starting with the product ID or name) or by an **Image** column in the Products sheet.
3. The system keeps its own cached copy (resized), because Drive links are slow and depend on permissions.
4. The images are used in two ways:
   - The agent can **send a product photo** when the customer asks, where the channel allows images.
   - Image matching (F06) compares customers' photos with these catalog images.
5. The image is sent to the customer as an upload through the channel. The Drive link is **never shared**, so the owner's Drive stays private.

**PDF catalogs and price lists**
1. The owner selects a PDF or image of a catalog or price list.
2. Gemini reads it and creates a **draft table** of products, prices and variants.
3. The owner **reviews and approves** the draft. Nothing is applied without approval.
4. Approved rows are added to the catalog.

**Rules**
- Only files in the folders the owner chose are used.
- Supported images: JPG, PNG and WebP, with a size limit.
- If a product has no image, the agent describes it in text and offers a human.

---

## F41 Document Generation: Invoices and Quotations (Phase 4)

**Purpose:** Create professional PDFs for orders and business enquiries without manual work.

**Trigger:** An order is confirmed (invoice or order slip), or a business enquiry asks for a quotation (F38 lead).

**How it works**
1. The owner makes a **Google Docs template** with placeholders such as `{{customer_name}}`, `{{order_id}}`, `{{items_table}}`, `{{total}}` and `{{date}}`.
2. The system copies the template into the shop's folder, and fills in the placeholders with data from the order or lead.
3. It exports the result as a **PDF** and saves it in a "Generated" folder in the owner's Drive.
4. The PDF is sent:
   - **Invoices and order slips:** automatically to the customer where the channel allows files and the Policy Engine (F31) permits it, or to the owner, as the owner chooses.
   - **Quotations:** always to the **owner for approval** first. The agent never promises custom prices (F21).

**Rules**
- All numbers come from the order or lead data, never from AI.
- Generated files contain customer data, so they stay in the owner's own Drive folder with restricted access.
- Old generated files are kept for the owner's retention period (F24).

**If something goes wrong:** If a placeholder is missing or the template cannot be read, the owner is alerted and a plain text summary is used instead.

---

## F42 Google Notifications and Reporting (Phase 4)

**Purpose:** Let owners who live in Google Workspace receive notifications and see reports there.

**Trigger:** A notification event (F15), the daily summary, or a report refresh.

**What is included**

| Part | What it does |
|---|---|
| **Gmail** | Sends owner notifications and the daily summary from the owner's Gmail or Workspace account |
| **Google Chat** | Sends notifications to a Google Chat space, as an alternative to Telegram |
| **Looker Studio** | A ready report built on the Orders and Daily Summary sheets (F38) shows orders, sales, response time, channels and costs. The owner can share it with the team |

**Rules**
- The owner chooses which events go to which place.
- Notifications contain only what the owner needs (F15).
- Reports refresh when the sheets are updated. The system's own analytics (F26) stay available too.

**Not included for now:** other Google services such as Calendar, Forms and Meet are possible future additions.

---

[← Channel: LinkedIn](16_Channel_LinkedIn.md) | [Index](00_README.md) | [Google Sheet and Doc Templates →](18_Google_Sheet_and_Doc_Templates.md)
