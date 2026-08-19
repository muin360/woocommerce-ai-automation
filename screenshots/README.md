# Screenshots & Visual Documentation

This directory is designated for visual walkthroughs and workflow execution captures.

> [!NOTE]
> In accordance with open-source integrity practices, fake or synthetic screenshots are not included in this repository. Maintainers and contributors running this workflow in their own test environment can capture and place real execution screenshots here.

---

## Recommended Screenshots

| Filename | Expected Content |
| :--- | :--- |
| **`workflow-overview.png`** | Full-canvas view of the 53-node n8n workflow showing all 4 subsystems. |
| **`telegram-agent.png`** | Screenshot of Telegram chat interacting with `WooBot` (creating/updating products). |
| **`woo-commerce-actions.png`** | Screenshot of WooCommerce admin panel showing products created/modified by the AI agent. |
| **`invoice-flow.png`** | Generated PDF invoice and corresponding customer confirmation email in Gmail. |
| **`sales-report.png`** | Telegram message showing AI-generated sales analytics with BDT currency formatting. |

---

## How to Add Screenshots
1. Open your self-hosted or cloud n8n canvas with the imported workflow.
2. Capture clean PNG screenshots of the canvas and Telegram/Store interactions.
3. Save the images using the exact filenames listed above into the `screenshots/` directory.
4. Update the main [`README.md`](../README.md) if custom paths are used.
