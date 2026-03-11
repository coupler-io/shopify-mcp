<div align="center">

# Shopify MCP Server by Coupler.io

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Transport](https://img.shields.io/badge/Transport-Streamable_HTTP-blue.svg)](#)
[![Auth](https://img.shields.io/badge/Auth-OAuth_2.0-green.svg)](#)

Coupler.io Shopify MCP server for Claude, ChatGPT, Gemini, Cursor, n8n, OpenClaw, and other MCP clients. Query and analyze Shopify data with natural language. Requires the Coupler.io account.

</div>

## Data Access

Access orders, products, inventory, customers, and detailed breakdowns (line items, fulfillments, shipping lines, refunds) from your Shopify store.

<details>
<summary><strong>Available entities</strong></summary>

#### Basics

| Entity | Description | Typical use case |
|---|---|---|
| **Orders** | Order-level data — totals, status, customer info, payment gateway, tags | Daily sales reports, revenue dashboards |
| **Products** | Product catalog — title, vendor, status, published/updated dates | Product management, catalog sync |
| **Inventory** | SKU-level stock quantities across locations | Inventory tracking, reorder alerts |
| **Customers** | Customer records — name, email, total spent, last order info | Customer lists, marketing segments, CRM sync |

#### Order & product breakdowns

| Entity | Description | Typical use case |
|---|---|---|
| **Products with variants** | Products split by variant — one row per variant with price, SKU, inventory | Variant-level pricing, SKU analysis |
| **Orders with line items** | Orders split by line item — one row per product in each order | Product sales analysis, SKU-level reporting |
| **Order fulfillments with line items** | Orders with fulfillment details — tracking info, delivery dates, line items | Shipping and logistics tracking |
| **Orders with shipping lines** | Orders with shipping method details — carrier, cost, delivery category | Shipping cost analysis |
| **Orders refunds transactions** | Orders with refund and transaction details — refund amounts, gateway, status | Refund analysis, financial reconciliation |

</details>

<details>
<summary><strong>Column groups for order entities</strong></summary>

| Column group | Available in | Examples |
|---|---|---|
| **Order** | All order entities | Order name, Created at, Financial status, Fulfillment status, Tags, Discount codes |
| **Customer** | All order entities | Customer name, Amount spent |
| **Billing address** | All order entities | Country |
| **Customer journey** | All order entities | Customer order index, Days to conversion |
| **Order totals in shop currency** | All order entities | Current order total, Net payment, Discounts, Tax, Shipping, Duties |
| **Line items** | Orders with line items | SKU, Quantity, Line item name |
| **Line items totals** | Orders with line items | Discounted total, Original total, Unit price |
| **Fulfillment** | Order fulfillments | Fulfillment name, Status, Created/Delivered/In-transit dates, Tracking info |
| **Fulfillment line item** | Order fulfillments | Line item name, SKU, Quantity, Unfulfilled quantity |
| **Shipping line** | Orders with shipping lines | Code, Title, Source, Delivery category |
| **Shipping line totals** | Orders with shipping lines | Original price, Discounted price |
| **Refund** | Orders refunds transactions | Refund created at, Note, Total refund amount |
| **Refund transaction** | Orders refunds transactions | Account number, Gateway, Status, Amount |

</details>


## Supported Clients

*Note: You will need to set up a data flow in Coupler.io with Shopify as a source and the AI tool of your choice as the destination.*

### Claude

Use with Claude Web, Desktop, Chat, Cowork, or Claude Code.

**Via Web/Desktop:** Go to **Customize**->**Connectors**->**Connect your tools**, search for Coupler.io and add it.

**Via Claude Code CLI:**

```bash
claude mcp add coupler-io --transport streamable-http https://mcp.coupler.io/mcp
```

### ChatGPT

Install from the **ChatGPT Apps** directory — search for "Coupler.io" in the **Apps** section of **Settings**.

### Cursor

Find it on the [Cursor Directory](https://cursor.directory/mcp/coupler-io-official-remote-mcp).

### Gemini CLI

Go to the **AI integrations** -> **Gemini CLI** page in your Coupler.io account to copy the correct command (unique to each account). It will look like this:

```bash
gemini mcp add coupler --transport=http https://mcp.coupler.io/mcp/xxxxx
```

### OpenClaw

Use the **mcporter** skill to connect, or install the **coupler-io** skill from ClawHub.
Directly ask your OpenClaw agent to add the skill and execute it.

## Links

- **Landing page:** [Shopify MCP by Coupler.io](https://www.coupler.io/mcp/shopify)
- **Coupler.io:** [https://coupler.io](https://coupler.io)
- **MCP Server endpoint:** `https://mcp.coupler.io/mcp`