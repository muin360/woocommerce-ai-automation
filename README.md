# 🛒 WooCommerce AI Automation (n8n Workflow)

> **Project Type**: Course Assignment / Learning Project  
> **Status**: Educational Demonstration / Open Source Portfolio  
> **Workflow Nodes**: 53 Nodes across 3 Core Automation Subsystems  
> **Author / Maintainer**: Arefin Mueen  

---

## 📌 Project Overview

**WooCommerce AI Automation** is an end-to-end e-commerce automation and store-assistant system built with [n8n](https://n8n.io/). Created as a comprehensive **course assignment and learning project**, this workflow connects Telegram, WooCommerce, OpenAI, Google Sheets, Google Drive, Gmail, and scheduled reporting engines to automate everyday store management, customer order fulfillment, invoice generation, and sales intelligence.

> [!NOTE]
> This project is designed as an educational demonstration and learning project. It is **not** a production client deployment and should be evaluated and adapted accordingly.

---

## 🚀 Key Capabilities

The workflow consists of **53 connected nodes across 3 core automation subsystems**:

### 1. 🤖 Telegram AI Store Management & Image Automation
Operates directly inside Telegram to process natural language commands (supporting **English, Bangla, and Banglish**) and handle image assets:
- **AI Catalog Assistant (`WooBot`)**:
  - **Create Products**: Automatically validates required parameters (name, regular price, sale price, stock quantity, description).
  - **Update Products**: Context-aware updates to existing items (stock adjustments, price changes) without overwriting unmodified fields.
  - **Delete Products**: Safe deletion of products with ID confirmation.
  - **Search & Get Products**: Fetches real-time price, SKU, and inventory levels.
  - **List Catalog**: Returns clean, formatted catalog summaries directly into Telegram chat.
  - **Entity & Reference Resolution**: Intelligently resolves products from conversational references (e.g., *"the last one"*, *"same item"*, partial names).
  - **Conversation Memory**: Multi-turn conversational buffer window scoped per Telegram user.
- **Product Image Ingestion & Resolution Pipeline**:
  - Ingests image attachments sent directly via Telegram.
  - Uploads images to WordPress Media Library via the WordPress REST API.
  - Extracts hosted public URLs and routes to a specialized **Image Update Agent** to bind the new image to the targeted product catalog entry.

### 2. 📦 WooCommerce Order Processing & Invoicing
Listens to real-time WooCommerce `order.created` webhooks to execute automated post-purchase operations:
- **Data Transformation**: Extracts and normalizes customer profile and line-item records.
- **Dynamic HTML/CSS Invoicing**: Generates a branded, responsive invoice document with itemized products, quantities, prices, and order totals in BDT.
- **Serverless PDF Generation**: Converts HTML into standard A4 PDF invoices.
- **Cloud Archival**: Saves generated invoices directly into a dedicated Google Drive folder (`Invoice-{order_id}.pdf`).
- **Automated Customer Email**: Sends a branded transactional confirmation email via Gmail with the PDF invoice attached.
- **Instant Admin Notification**: Dispatches rich Telegram alerts to store management with order IDs, customer details, and action checklists.
- **Two-Way Google Sheets Sync**:
  - Upserts customer CRM profiles (matching by email).
  - Appends detailed order records with line items to the `Orders` ledger sheet.

### 3. 📊 Sales Analytics & Reporting
Provides scheduled and on-demand executive sales performance briefings:
- **Multi-Branch Execution**: Configured for on-demand execution (Manual Trigger) and recurring scheduled intervals (daily, weekly, and monthly branches).
- **Automated Aggregation**: Aggregates order volume, gross revenue (BDT), line-item counts, and identifies top-selling products.
- **AI Sales Briefings**: Uses OpenAI (`gpt-4o-mini`) to analyze performance metrics, extract business insights, and generate actionable recommendations formatted for Telegram.

---

## 🏗️ Architecture

```text
                                  ┌───────────────────────┐
                                  │   Telegram User/Admin │
                                  └───────────┬───────────┘
                                              │
                                     (Commands / Photos)
                                              ▼
┌──────────────────┐              ┌───────────────────────┐
│   WooCommerce    │◄────────────►│     n8n AI Engine     │
│ (Products/Orders)│              │  (LangChain + OpenAI) │
└────────┬─────────┘              └───────────┬───────────┘
         │                                    │
   (order.created)                     (Alerts / Reports /
         ▼                             Invoice Archival)
┌────────┴────────────────────────────────────┬───────────┐
│                                                         │
│  ┌────────────────┐   ┌───────────────┐   ┌───────────┐ │
└──►  Google Drive  │   │ Google Sheets │   │   Gmail   │ │
   │ (PDF Storage)  │   │  (CRM & Log)  │   │(Customer) │ │
   └────────────────┘   └───────────────┘   └───────────┘ │
└─────────────────────────────────────────────────────────┘
```

> For comprehensive architecture breakdowns, node graphs, and subsystem diagrams, see [**`docs/architecture.md`**](docs/architecture.md).

---

## 🛠️ Tech Stack

| Technology | Purpose |
| :--- | :--- |
| **[n8n](https://n8n.io/)** | Primary workflow automation engine (Self-hosted or Cloud) |
| **[LangChain (n8n-nodes-langchain)](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)** | AI Agent orchestration, tool binding, and conversational memory |
| **[OpenAI (GPT-4o-mini)](https://openai.com/)** | Natural language processing, catalog assistant, and sales analytics |
| **[WooCommerce REST API](https://woocommerce.github.io/woocommerce-rest-api-docs/)** | Store catalog CRUD and real-time order webhook triggers |
| **[WordPress REST API](https://developer.wordpress.org/rest-api/)** | Media upload handler for product imagery |
| **[HTML/CSS to PDF API](https://www.htmlcsstopdf.com/)** | Automated serverless PDF compilation |
| **[Google Workspace](https://workspace.google.com/)** | Google Drive (PDF archive), Google Sheets (Database), Gmail (Email) |
| **[Telegram Bot API](https://core.telegram.org/bots/api)** | Conversational UI, instant alerts, and sales digest delivery |

---

## 📂 Repository Structure

```text
woocommerce-ai-automation/
├── workflow/
│   ├── woocommerce-ai-automation.json            # Sanitized public n8n workflow export
│   └── woocommerce-ai-automation.original.local.json # Local backup (ignored by git)
├── screenshots/
│   └── README.md                                 # Guide & placeholders for UI screenshots
├── docs/
│   └── architecture.md                           # Comprehensive architecture and flow diagrams
├── README.md                                     # Main documentation
├── SECURITY.md                                   # Sanitization standards & security policy
└── .gitignore                                    # Excludes credentials, private files & local backups
```

---

## ⚙️ Setup & Installation

### 1. Prerequisites
- A running instance of **n8n** (v1.x+ recommended).
- A **WooCommerce** store (v7.x+) with REST API access enabled.
- An **OpenAI API Key**.
- A **Telegram Bot** created via [@BotFather](https://t.me/BotFather).
- A **Google Cloud Console Project** with OAuth2 enabled for Google Drive, Google Sheets, and Gmail.
- An **HTML/CSS to PDF** API account.

### 2. Import into n8n
1. Download or copy [`workflow/woocommerce-ai-automation.json`](workflow/woocommerce-ai-automation.json).
2. Open your n8n dashboard.
3. Select **Workflows** &rarr; **Add Workflow** &rarr; **Import from File / Paste JSON**.
4. Click **Save**.

### 3. Required Credentials Checklist
Configure the following authenticated credentials inside your n8n instance:

| Credential Name in n8n | Target Service | Parameters |
| :--- | :--- | :--- |
| `Telegram account` | Telegram API | Bot Access Token |
| `OpenAI account` | OpenAI API | API Key |
| `WooCommerce account` | WooCommerce API | Consumer Key, Consumer Secret, Store URL |
| `WordPress Basic Auth account` | WordPress REST API | Username, Application Password |
| `HTML to PDF account` | HTML/CSS to PDF API | API Key / Token |
| `Google Drive account` | Google Drive OAuth2 | Client ID & Client Secret |
| `Gmail account` | Gmail OAuth2 | Client ID & Client Secret |
| `Google Sheets account` | Google Sheets OAuth2 | Client ID & Client Secret |

### 4. Configure Environment Placeholders
Open the imported nodes and set the following parameters:
- **Telegram Admin Nodes**: Update `chatId` to your numeric Telegram Admin Chat ID (`YOUR_TELEGRAM_ADMIN_CHAT_ID`).
- **HTTP Request (Media Upload)**: Update URL to your live WordPress endpoint (`https://YOUR-WORDPRESS-SITE.com/wp-json/wp/v2/media`).
- **Google Drive Upload Node**: Set `folderId` to your target Google Drive folder ID.
- **Google Sheets Nodes**: Select your target Spreadsheet Document ID and Sheets (`Customer_Information` and `Orders`).

---

## 💬 Example Agent Commands

Interact with `WooBot` on Telegram in English or Bangla:

### Product Management
```text
User: "Create a new product named 'Ergonomic Gaming Chair' with regular price 15000 and stock 25"
Agent: "✅ Product 'Ergonomic Gaming Chair' created successfully with Regular Price: BDT 15,000 and Stock: 25."

User: "Update Gaming Chair stock to 30 and sale price to 13500"
Agent: "✅ Stock updated — Ergonomic Gaming Chair now has 30 units (Sale Price: BDT 13,500)."

User: "এই প্রোডাক্টটির প্রাইস কত?" (What is the price of this product?)
Agent: "Product: Ergonomic Gaming Chair | Price: ৳15,000 | Stock: 30"

User: "List all products in stock"
Agent: "Here is your store inventory: ..."
```

### Image Upload Flow
1. Send an image photo attachment to the Telegram bot with caption:
   `"Change image for Ergonomic Gaming Chair"`
2. The workflow routes the binary image through the WordPress Media API, retrieves the hosted asset URL, verifies the product ID, and updates the WooCommerce product gallery.

---

## 🔍 Known Limitations & Assignment Notes

In accordance with transparent engineering documentation, the following design characteristics from the original assignment are preserved:

1. **Date Range Filtering in Reporting Branches**:
   - The workflow contains 4 reporting branches (Manual, Scheduled Monthly, and Scheduled Weekly triggers).
   - In all branches, the WooCommerce order query is filtered with `after: today 00:00:00` (`new Date().toISOString().split('T')[0] + 'T00:00:00'`).
   - Consequently, each reporting branch currently aggregates **today's incoming orders** rather than multi-week or multi-month historical windows.
   - *TODO for future version*: Implement dynamic ISO timestamp offsets (e.g., `now - 7 days` or `now - 30 days`) for true historical multi-day analytics.

2. **Image Ingestion Domain Tunneling**:
   - For local development setups (e.g., LocalWP / Docker), public image URLs require an external tunnel (such as ngrok or Cloudflare Tunnels) for WooCommerce to fetch media attachments.

3. **PDF Generator Dependency**:
   - The workflow utilizes a cloud HTML-to-PDF service. High-volume deployments may benefit from self-hosted solutions like Gotenberg or Puppeteer.

---

## 🔮 Future Roadmap & Improvements

- [ ] **Dynamic Date Range Calculators**: Parameterize date bounds for 7-day, 30-day, and quarterly financial reports.
- [ ] **Low-Stock Alerting**: Automatic notifications when inventory falls below threshold levels.
- [ ] **Webhook Signature Verification**: HMAC-SHA256 signature checking on incoming WooCommerce webhooks.
- [ ] **Multi-Currency Support**: Dynamic currency symbol selection based on store locale settings.

---

## 🔒 Security & Privacy

This repository contains a **100% sanitized workflow export**. All private tokens, passwords, customer email addresses, phone numbers, and local infrastructure identifiers have been removed. 

For full security policy details, see [**`SECURITY.md`**](SECURITY.md).

---

## 📜 Disclaimer

This project is shared for **educational, learning, and portfolio demonstration purposes**. It is not endorsed by or affiliated with WooCommerce, Automattic, Telegram, or OpenAI.
