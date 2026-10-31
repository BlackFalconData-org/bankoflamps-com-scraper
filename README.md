# Bank of Lamps Product and Price Scraper

Extract structured data from [bankoflamps.com](https://bankoflamps.com) — bankoflamps.com — lamps, light bulbs and luminaires with EAN/GTIN, SKU, MPN, brand, B2B list price excluding VAT, pack and box quantities, stock status, wattage, lamp base, full specs and datasheet links. Incremental mode returns only new or changed products.

**[Bank of Lamps Product and Price Scraper on Apify →](https://apify.com/blackfalcondata/bankoflamps-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from bankoflamps.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from bankoflamps.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "lamps",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Catalogue section to scrape — a category or brand path on bankoflamps.com, e.g. `lamps`, `light-bulbs`, `luminaires`, `osram`. This is not a keyword search: bankoflamps.com has none, so use a section path. Use a JSON array for several, e.g. ["lamps","osram"]. Required if no startUrls provided. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste category or brand listing URLs, or individual product page URLs, from bankoflamps.com — mixed in one run is fine (facet filters in a listing URL's query string are kept). Each URL becomes its own task; results are merged and deduped by product ID across all URLs. When provided, startUrls REPLACE the category field (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch each product page for description, full specs, datasheets, all images and categories. Slower, but far richer records. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from your search inputs (categories, startUrls) so different catalogue sections never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When incremental, also emit products that have not changed since the last run (marked UNCHANGED). |
| `emitExpired` | boolean | `false` | When incremental, also emit previously seen products that are no longer listed (marked EXPIRED). Scans the complete result set to decide. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |

---

## Output fields

Every product returns the same 50-field schema. Missing values are `null` — never omitted.

- `recordType`
- `type`
- `productId`
- `ean`
- `gtin`
- `sku`
- `mpn`
- `title`
- `name`
- `description`
- `brand`
- `brandUrl`
- `category`
- `categories`
- `price`
- `currency`
- `priceCurrency`
- `priceBasis`
- `priceNote`
- `priceValidity`
- `splitBoxPrice`
- `moq`
- `packageQty`
- `boxQty`
- `palletQty`
- `availability`
- `schemaAvailability`
- `inStock`
- `stockLabels`
- `discontinued`
- `leadTime`
- `condition`
- `wattage`
- `lampBase`
- `specs`
- `documents`
- `keywords`
- `imageUrl`
- `images`
- `url`
- `productUrl`
- `portalUrl`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "recordType": "product",
  "type": "product",
  "productId": "31577",
  "ean": "4008321275622",
  "gtin": "04008321275622",
  "sku": "4008321275622",
  "mpn": "4008321275622",
  "title": "HALOLINE® PRO 64701 230 W 240 V R7S",
  "name": "HALOLINE® PRO 64701 230 W 240 V R7S 4008321275622 — 4008321275622",
  "description": "Double ended halogen lamps with tube base. Product features: Average life: 2,000 h. Dimmable. Color rendering index Ra: 100. Product benefits: Brilliant accent lighting. Direct rep…",
  "brand": "OSRAM",
  "brandUrl": "https://bankoflamps.com/osram"
}
```

*Truncated — full records contain 50 fields. See Output fields for the complete schema.*

**[Try Bank of Lamps Product and Price Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/bankoflamps-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.0018 |

See the [actor on Apify](https://apify.com/blackfalcondata/bankoflamps-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape bankoflamps.com?**
Use this actor on Apify to extract structured data from bankoflamps.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get bankoflamps.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape bankoflamps.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check bankoflamps.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Bank of Lamps Product and Price Scraper](https://apify.com/blackfalcondata/bankoflamps-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
