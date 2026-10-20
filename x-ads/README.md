# X Ads MCP server

Connect X Ads (Twitter Ads) to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about spend, impressions and link clicks from your X (Twitter) campaigns.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your X Ads data comes from the X Ads account you've connected in Jepto, with the same 3 metrics and 3 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "What did I spend on X Ads last month, by campaign?"
- "Show daily impressions and link clicks for the last 30 days."

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

X Ads is read-only through the MCP: the assistant can query your data but can't change anything in X Ads.

## Fields

6 fields (3 metrics, 3 dimensions), from version 1.0.2 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `twitterAds.impressions` | Impressions | SUM |  | The number of times your ads were shown on X. |
| `twitterAds.clicks` | Link clicks | SUM |  | The number of times users clicked your ad links. |
| `twitterAds.spend` | Spend | SUM | Currency | Total amount spent on ads in your account currency. |

### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `twitterAds.date` | Date | time | Date of the reporting period. |
| `twitterAds.campaignId` | Campaign ID | string | Unique identifier for the X (Twitter) campaign. |
| `twitterAds.segment` | Segment | string | Audience or placement segment applied to the report (e.g. gender, age group, platform, device). |

## Cross-platform fields

`unified.*` fields add X Ads to totals across all your ad platforms, so one question can cover Google Ads, Meta, Microsoft, LinkedIn and TikTok together.

| Unified field | Label | From |
|---|---|---|
| `unified.impressions` | Impressions | `twitterAds.impressions` |
| `unified.clicks` | Clicks | `twitterAds.clicks` |
| `unified.date` | Date | `twitterAds.date` |
| `unified.campaignId` | Campaign ID | `twitterAds.campaignId` |

## Links

- [X Ads on jepto.com](https://www.jepto.com/data-sources/x-ads)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
