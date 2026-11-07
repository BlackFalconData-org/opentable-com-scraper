# OpenTable Restaurant Scraper [💰$1.08]

Extract structured data from [opentable.com](https://opentable.com) — OpenTable restaurant search results for any US city, cuisine or map point — one row per restaurant with rating, review count, price band, cuisine, address and phone. Optional profile details add opening hours and website, dress code and executive chef.

**[OpenTable Restaurant Scraper [💰$1.08] on Apify →](https://apify.com/blackfalcondata/opentable-com-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by description format, and more.

**Detail enrichment** — Fetch full descriptions, contact information for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N records (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from opentable.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from opentable.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "italian",
  "location": "New York, NY",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Restaurant search term — a cuisine, a restaurant name, or a city (e.g. "italian", "sushi", "new york"). Required if no startUrls provided. Use a JSON array for multiple searches, e.g. ["italian","sushi"]. |
| `location` | string | — | City, metro, or neighborhood (e.g. "New York, NY"). Combined with the search term into one OpenTable search. Use a JSON array for multiple locations. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste OpenTable search-results URLs (for example https://www.opentable.com/s?term=italian%20chicago). Each URL becomes its own search; results are merged and deduped by restaurant ID. When provided, start URLs REPLACE the query/location fields (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch each restaurant's profile page for website, email, hours, dress code, chef, payment options, parking, cuisines and more. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip restaurants whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit restaurants whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit restaurants that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED restaurants. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of restaurants included in each notification message (1–20). |
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
| `lat` | number | — | Latitude of the point to search around (pair with Longitude). Results are ranked by distance from it; combine with a query such as "sushi" or leave the query empty to list every restaurant near the point. |
| `lon` | number | — | Longitude of the point to search around (pair with Latitude). |
| `radiusKm` | integer | — | Keep only restaurants within this many kilometres of the latitude/longitude point. Leave empty to keep every result, nearest first. |

---

## Output fields

Every record returns the same 77-field schema. Missing values are `null` — never omitted.

- `cuisines`
- `primaryCuisine`
- `email`
- `website`
- `hours`
- `dressCode`
- `executiveChef`
- `publicTransit`
- `parkingInfo`
- `parkingDetails`
- `crossStreet`
- `metro`
- `entertainment`
- `cateringDetails`
- `facebookUrl`
- `timeZone`
- `paymentOptions`
- `additionalDetails`
- `popularDishes`
- `hasDeliveryPartners`
- `hasDeliveryDirect`
- `maxAdvanceDays`
- `privateDining`
- `features`
- `profilePhoto`
- `restaurantId`
- `name`
- `url`
- `portalUrl`
- `profileUrl`
- `cuisine`
- `priceBand`
- `priceBandId`
- `priceRange`
- `currencySymbol`
- `neighborhood`
- `address`
- `city`
- `state`
- `postalCode`
- `latitude`
- `longitude`
- `phone`
- `phoneFormatted`
- `rating`
- `reviewCount`
- `recentReservationCount`
- `diningStyle`
- `description`
- `topReview`
- `hasTakeout`
- `orderOnlineLink`
- `deliveryPartners`
- `hasPrivateDining`
- `hasBar`
- `hasCounter`
- `hasHighTop`
- `hasOutdoor`
- `maxPartySize`
- `awards`
- `justAdded`
- `restaurantType`
- `slotDiscovery`
- `photoUrl`
- `photos`
- `position`
- `searchTerm`
- `searchLocation`
- `searchTotalResults`
- `listingId`
- `contentHash`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "cuisines": [
    "Pizzeria",
    "Italian",
    "Wine Bar"
  ],
  "primaryCuisine": "Pizzeria",
  "email": "",
  "website": "https://www.eataly.com/us_en/stores/nyc-downtown/restaurants/downtown-pizza-pasta",
  "hours": "Daily 11:00 am–10:00 pm",
  "dressCode": "Casual Dress",
  "executiveChef": null,
  "publicTransit": "Fulton Street A, C, J, Z, 2, 3, 4, 5 Cortland ST R,W",
  "parkingInfo": "None",
  "parkingDetails": null,
  "crossStreet": "Liberty and Church",
  "metro": null
}
```

*Truncated — full records contain 77 fields. See Output fields for the complete schema.*

**[Try OpenTable Restaurant Scraper [💰$1.08] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/opentable-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.00045 |
| Result | $0.00108 |

See the [actor on Apify](https://apify.com/blackfalcondata/opentable-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape opentable.com?**
Use this actor on Apify to extract structured data from opentable.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get opentable.com data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape opentable.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check opentable.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [OpenTable Restaurant Scraper [💰$1.08]](https://apify.com/blackfalcondata/opentable-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
