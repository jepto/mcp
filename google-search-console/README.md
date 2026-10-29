# Google Search Console MCP server

Connect Google Search Console to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about the queries and pages that bring you organic traffic: clicks, impressions, CTR and average position by query, page, country, device and search appearance.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your Search Console data comes from the Search Console account you've connected in Jepto, with the same 4 metrics and 6 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "Show me queries where my average position is between 5 and 15 and impressions are over 1,000 in the last 90 days."
- "Which pages lost the most clicks this month compared with last month?"
- "Break down clicks and CTR by device and country for the last 28 days."

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

Google Search Console is read-only through the MCP: the assistant can query your data but can't change anything in Search Console.

## Example query

This is the kind of `Analytics_Query` call an assistant makes for you. Field names are the same as in Jepto Reports and Data Chat.

```json
{
  "clientId": "<clientId from Client_List>",
  "measures": [
    "googleSearchConsole.clicks",
    "googleSearchConsole.impressions",
    "googleSearchConsole.position"
  ],
  "dimensions": [
    "googleSearchConsole.query"
  ],
  "dateRange": [
    "last90days"
  ],
  "filters": [
    {
      "member": "googleSearchConsole.impressions",
      "operator": "gt",
      "values": [
        "1000"
      ]
    }
  ],
  "order": {
    "googleSearchConsole.impressions": "desc"
  },
  "limit": 50
}
```

## Fields

10 fields (4 metrics, 6 dimensions), from version 1.1.3 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `googleSearchConsole.clicks` | Clicks | SUM |  | Number of times users clicked through to your site from Google Search results. |
| `googleSearchConsole.impressions` | Impressions | SUM |  | Number of times your pages appeared in Google Search results. |
| `googleSearchConsole.ctr` | CTR | AVG | Percentage | Click-through rate: clicks divided by impressions. Shown as a percentage in Search Console. |
| `googleSearchConsole.position` | Average position | AVG |  | Average ranking position of your pages in search results (lower is better). |

### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `googleSearchConsole.date` | Date | string | Date of the reporting period. |
| `googleSearchConsole.country` | Country | string | Country where the search user was located. |
| `googleSearchConsole.device` | Device | string | Device category (mobile, desktop, or tablet) used for the search. |
| `googleSearchConsole.page` | Page | string | Page URL of your site that appeared in Google Search results. |
| `googleSearchConsole.query` | Query | string | Search terms users typed before seeing your site in results. |
| `googleSearchConsole.searchAppearance` | Search appearance | string | How your content appeared in search results (e.g. AMP, rich result, web light, video). |

## Cross-platform fields

`unified.*` fields add Google Search Console to totals across all your ad platforms, so one question can cover Google Ads, Meta, Microsoft, LinkedIn and TikTok together.

| Unified field | Label | From |
|---|---|---|
| `unified.date` | Date | `googleSearchConsole.date` |

## FAQ

**Is there a Google Search Console MCP server?**
Yes. Jepto hosts an MCP server at https://mcp.jepto.com that gives Claude, ChatGPT, Cursor and other MCP clients access to your Google Search Console data. There's nothing to install: you sign in to Jepto with OAuth, and the assistant can query every Search Console field in the Jepto data model.

**Which Google Search Console metrics can I report on?**
Jepto includes Search Console metrics: Clicks, Impressions, CTR and Average position. You can break them down by dimensions such as Date, Country, Device, Page, Query and Search appearance. Every field, with its definition, is listed on this page.

**Can an AI assistant make changes in Google Search Console?**
No. Through the Jepto MCP, Search Console data is read-only: the assistant can query it but can't change anything in Search Console.

**Can I combine Search Console with Google Ads in one report?**
Yes. When both are connected to the same brand, any report or chart can use fields from either source, and Data Chat and the Jepto MCP can answer questions that span the two. That lets you put organic queries next to paid search terms.

**Why are the most recent days missing?**
Google publishes Search Console data with a delay of a couple of days. Jepto fills those days in as Google releases them.

## Links

- [Google Search Console on jepto.com](https://www.jepto.com/data-sources/google-search-console)
- [Compare marketing MCP servers](https://www.jepto.com/compare/marketing-mcp-servers)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
