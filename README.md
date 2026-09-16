<div align="center">

# Shopify MCP Server by Coupler.io

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Transport](https://img.shields.io/badge/Transport-Streamable_HTTP-blue.svg)](#)
[![Auth](https://img.shields.io/badge/Auth-OAuth_2.0-green.svg)](#)

Connect Shopify data to AI with the Coupler.io MCP server. Ask natural-language questions about orders, revenue, products, customers, inventory, fulfillment, refunds, and ecommerce performance in ChatGPT, Claude, Gemini, Cursor, and other MCP-compatible AI tools. Requires a Coupler.io account.

[Landing page](https://www.coupler.io/mcp/shopify) · [Documentation](https://docs.coupler.io/ai/mcp) · [All Coupler.io MCP integrations](https://github.com/coupler-io)

</div>

## What you can ask

- Which products generated the most revenue last month?
- Which products have low inventory but high sales velocity?
- How has average order value changed?
- Which customers have the highest lifetime spend?
- Which products have the highest refund rates?

## How it works

This repository documents the Shopify integration for the Coupler.io MCP server.

1. Connect Shopify to Coupler.io.
2. Select your AI tool as the destination.
3. Connect your AI client to Coupler.io MCP.
4. Ask questions about your Shopify data in natural language.

Coupler.io sits between Shopify and your AI client. It holds the Shopify credential, imports the data on a schedule, and exposes the result as a data set the AI can query. Your AI client never calls the Shopify API itself.

```
  Shopify
      |        credential held by Coupler.io
      v
  Coupler.io          import, transform, store on a schedule
      |
      v
  MCP server          schema, SQL query execution
      |
      v
  Your AI client      your question, in plain language
```

When you ask a question, the AI reads the data set's schema, writes SQL, and Coupler.io runs that query on its own side. Only the result comes back to the AI, so a large data set does not have to fit into the model's context window.

| | |
|---|---|
| **MCP endpoint** | `https://mcp.coupler.io/mcp` |
| **Transport** | Streamable HTTP |
| **Authentication** | OAuth 2.0 |
| **Query language** | SQL, executed by Coupler.io |
| **Refresh schedule** | From monthly to every 15 minutes, depending on your plan |

## Get started

*Note: You will need to set up a data flow in Coupler.io with Shopify as a source and the AI tool of your choice as the destination.*

### Claude

Use with Claude Web, Desktop, Chat, Cowork, or Claude Code.

**Via Web/Desktop:** Go to **Customize**->**Connectors**->**Connect your tools**, search for Coupler.io and add it.

**Via Claude Code CLI:**

```bash
claude mcp add coupler-io --transport streamable-http https://mcp.coupler.io/mcp
```

### ChatGPT

Install the [Coupler.io ChatGPT app](https://l.rw.rw/couplerio-chatgpt-app) and complete the authentication. You can also find it by searching for "Coupler.io" in the **Apps** section of **Settings**.

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

## Data you can access

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

Coupler.io imports order entities with the column groups you select in the data flow. Line-item, fulfillment, shipping, and refund detail each come from a different entity, so pick the one that matches the level of detail your question needs.

## Example questions

### Sales and products

- Which products generated the most revenue last month?
- How has average order value changed over the last six months?
- Which products have the highest refund rates?

### Customers

- Which customers have the highest lifetime spend?
- What share of orders last quarter came from repeat customers?
- How does first-order value compare with repeat-order value?

### Inventory and fulfillment

- Which products have low inventory but high sales velocity?
- What is our average time from order to fulfillment?
- Which shipping methods cost the most relative to the orders that use them?

## Security and permissions

Your AI client never connects to Shopify directly. Coupler.io holds the Shopify credential, imports the data, and exposes only the resulting data set over MCP.

- **Your Shopify data is never modified.** Coupler.io only reads from Shopify. The AI queries the copy Coupler.io imported and cannot edit, delete, or overwrite it, let alone write anything back to your Shopify account.
- **Visibility is scoped per AI tool.** An AI client sees only the data sets from data flows that have *that client* set as a destination. Adding Claude as a destination does not expose the data flow to ChatGPT, though one flow can name both.
- **Configuration changes are possible, and confirmed first.** With the full tool set available, the AI can create data flows, add sources and destinations, change a schedule, or trigger a run. Those are real changes to your workspace, so the server instructs the AI to confirm before making one you did not ask for.
- **Nothing else is reachable.** The MCP server exposes the data sets described above and nothing more. It cannot reach your other accounts or your machine.
- **Disconnect at any time** by removing the connector in your AI client, or by deleting the credential or the data flow in Coupler.io.

Coupler.io is SOC 2 certified and compliant with GDPR and HIPAA.

## Troubleshooting

**The AI cannot find my Shopify data set.**
Usually you have not added that AI tool as a destination yet. Open the data flow in Coupler.io and add your AI client. One data flow can have several AI tools as destinations at the same time, so adding ChatGPT does not displace Claude. Each tool sees only the flows it is named on.

**The numbers look out of date.**
The AI reads the last imported snapshot, not Shopify live. Check the data flow's refresh schedule, or ask your AI client to run the data flow now.

**A field I need is missing.**
Coupler.io imports only the fields you select in the data flow's source. Add the missing fields and re-run the flow.

**The AI misreads a metric.**
Save the business context on the data set: what a metric means, which currency it is in, which rows to exclude. You do not have to leave your AI tool to do it, just tell the assistant to update the data set context and it saves it for you. The AI reads that context before it queries, so the next conversation uses your definitions instead of guessing.

**The connector does not appear in my AI client.**
Availability differs by AI client and subscription plan. Follow the client-specific steps under [Get started](#get-started), and check the [AI destination docs](https://docs.coupler.io/destinations/categories/ai) for that tool.

## Related Coupler.io MCP integrations

- [Google Analytics 4 MCP](https://github.com/coupler-io/google-analytics-4-mcp) — connect ecommerce sales with website traffic and conversion behavior
- [Google Ads MCP](https://github.com/coupler-io/google-ads-mcp) — analyze paid search performance alongside ecommerce revenue
- [Facebook Ads MCP](https://github.com/coupler-io/facebook-ads-mcp) — connect paid social campaigns with orders and sales
- [QuickBooks MCP](https://github.com/coupler-io/quickbooks-mcp) — connect ecommerce performance with accounting and financial data

[Explore all Coupler.io MCP integrations](https://github.com/coupler-io)

## FAQ

### What is the Shopify MCP server?

It is the Shopify integration for the Coupler.io MCP server, an endpoint that lets AI clients query your Shopify data in plain language. Coupler.io imports the data, stores it, and answers the AI's SQL queries on its own infrastructure.

### Do I need a Coupler.io account?

Yes. The MCP server serves data from your Coupler.io workspace, so you need an account with a data flow that has Shopify as a source and your AI tool as a destination.

### Does this connect directly to my Shopify account?

No. Coupler.io connects to Shopify, imports the data, and exposes the resulting data set over MCP. Your AI client talks to Coupler.io, never to Shopify.

### Which Shopify data can AI access?

Whatever your data flow imports. See [Data you can access](#data-you-can-access) for the full catalog of report types and fields. The AI reaches only the data sets in flows that name your AI tool as a destination.

### Is the integration read-only?

Yes. Nothing you or your AI client does through Coupler.io changes your Shopify data. Coupler.io only reads from Shopify, and the AI only queries the copy Coupler.io imported. It cannot edit, delete, or write anything back to your Shopify account.

### Which AI assistants can I use?

Claude, ChatGPT, Cursor, Gemini CLI, OpenClaw, Perplexity, and any client that speaks MCP through the Custom MCP destination. Setup steps for the clients above are under [Get started](#get-started); for the rest, see the [AI destination docs](https://docs.coupler.io/destinations/categories/ai).

### Do I need to write SQL or code?

No. You ask in plain language; the AI writes the SQL and Coupler.io runs it. Writing SQL yourself stays an option if you want a specific transformation.

### How fresh is the data?

As fresh as the last data flow run. Schedules range from monthly to every 15 minutes depending on your plan, and you can ask your AI client to refresh the flow on demand.

## Links

- **Landing page:** [Shopify MCP by Coupler.io](https://www.coupler.io/mcp/shopify)
- **Documentation:** [Coupler.io MCP](https://docs.coupler.io/ai/mcp)
- **Coupler.io:** [https://coupler.io](https://coupler.io)
- **MCP Server endpoint:** `https://mcp.coupler.io/mcp`
- **All Coupler.io MCP integrations:** [https://github.com/coupler-io](https://github.com/coupler-io)
