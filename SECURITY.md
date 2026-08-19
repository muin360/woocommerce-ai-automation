# Security Policy & Sanitization Notice

## Overview

This repository contains an open-source, sanitized export of an **n8n automation workflow** designed for educational and demonstration purposes as part of a **Course Assignment / Learning Project**.

To protect privacy and comply with security best practices, **all sensitive identifiers, private credentials, real customer data, webhook IDs, and internal infrastructure URLs have been completely removed and replaced with standard placeholders.**

---

## Sanitization Standards

The public workflow (`workflow/woocommerce-ai-automation.json`) has been rigorously scrubbed:

| Data Category | Sanitization Standard Applied |
| :--- | :--- |
| **API Keys & Credentials** | Replaced with generic placeholders (e.g., `YOUR_OPENAI_CREDENTIAL_ID`, `YOUR_WOOCOMMERCE_CREDENTIAL_ID`). Real credential objects, IDs, and tokens have been stripped. |
| **Telegram Identifiers** | Real chat IDs and bot tokens replaced with `YOUR_TELEGRAM_ADMIN_CHAT_ID` and dynamic context bindings (`{{ $('Telegram Trigger').item.json.message.chat.id }}`). |
| **Email Addresses** | Private/testing email addresses removed and replaced with dynamic customer data variables (`{{ $('format data').item.json.customer_email }}`). |
| **Google Drive & Sheets** | Hardcoded sheet IDs, folder IDs, and private URLs replaced with `YOUR_GOOGLE_SHEET_DOCUMENT_ID`, `YOUR_ORDERS_SHEET_GID`, and `YOUR_GOOGLE_DRIVE_FOLDER_ID`. |
| **Infrastructure & Tunnels** | Private local hostnames (`*.local`), ngrok URLs (`*.ngrok-free.dev`), and WordPress REST endpoints replaced with sanitized templates (e.g., `https://YOUR-WORDPRESS-SITE.com/wp-json/wp/v2/media`). |
| **Webhook Identifiers** | Stripped and replaced with clean template placeholders (`YOUR_TELEGRAM_WEBHOOK_ID`, `YOUR_WOOCOMMERCE_WEBHOOK_ID`). |

---

## Guidelines for Users & Contributors

### 1. Never Commit Secrets or Sensitive Data
When configuring and running this workflow on your own n8n instance:
- **Do not commit** `.env`, `credentials.json`, or exported workflows containing your active API credentials.
- **Do not commit** real customer order data, phone numbers, email addresses, or physical delivery addresses.
- Keep all local backups in `.local.json` or `.env` files, which are ignored by `.gitignore`.

### 2. Required Setup Steps
Before activating the workflow in your n8n environment:
1. Import `workflow/woocommerce-ai-automation.json` into your n8n instance.
2. Configure your own authenticated credentials in n8n for:
   - Telegram Bot API
   - OpenAI API (`gpt-4o-mini` or compatible model)
   - WooCommerce REST API (Consumer Key & Secret)
   - WordPress Basic Auth (Application Password for Media Uploads)
   - HTML/CSS to PDF API
   - Google Drive OAuth2
   - Gmail OAuth2
   - Google Sheets OAuth2
3. Replace all placeholder variables (`YOUR_TELEGRAM_ADMIN_CHAT_ID`, `YOUR_GOOGLE_SHEET_DOCUMENT_ID`, etc.) in the respective node parameters.

---

## Pre-Publication Verification Checklist

If you customize and re-export this workflow for your own repository, perform the following verification:
- [ ] Verify that no API keys or access tokens are present in node parameters.
- [ ] Confirm no personal email addresses, phone numbers, or customer names exist in test fixtures.
- [ ] Confirm no private tunnel/ngrok URLs or internal IP addresses are embedded in HTTP Request nodes.
- [ ] Ensure `.gitignore` properly excludes local workflow files and credential caches.

---

## Reporting a Vulnerability

If you discover an accidental exposure of sensitive information or a security concern within this repository, please do not open a public issue. Instead, contact the repository maintainer directly or submit a security advisory through GitHub.
