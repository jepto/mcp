# Facebook Ads MCP server

Connect Facebook Ads to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about your Meta campaigns, ad sets and ads: amount spent, reach, frequency, link clicks and results, broken down by platform, placement, age, gender and region.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your Facebook Ads data comes from the Facebook Ads account you've connected in Jepto, with the same 17 metrics and 19 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "Compare amount spent, CPM and link clicks for Facebook vs Instagram over the last 14 days."
- "Which ads have the highest frequency this month?"
- "Break down reach by age and gender for my prospecting campaigns."

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

Facebook Ads is read-only through the MCP: the assistant can query your data but can't change anything in Facebook Ads.

## Example query

This is the kind of `Analytics_Query` call an assistant makes for you. Field names are the same as in Jepto Reports and Data Chat.

```json
{
  "clientId": "<clientId from Client_List>",
  "measures": [
    "facebookAds.spend",
    "facebookAds.reach",
    "facebookAds.frequency"
  ],
  "dimensions": [
    "facebookAds.publisherPlatform",
    "facebookAds.platformPosition"
  ],
  "dateRange": [
    "last14days"
  ]
}
```

## Fields

36 fields (17 metrics, 19 dimensions), from version 1.2.5 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `facebookAds.impressions` | Impressions | SUM |  | The number of times your ads were on screen, including when the same person saw your ad more than once. |
| `facebookAds.clicks` | Link clicks | SUM |  | Clicks on links in your ads that go to destinations such as your website, app, or a Meta experience. |
| `facebookAds.spend` | Amount spent | SUM | Currency | Total amount spent on your ads during the reporting period, in your account currency. |
| `facebookAds.reach` | Reach | MAX |  | The number of people who saw your ads at least once. Reach is an estimated metric. |
| `facebookAds.frequency` | Frequency | Calculated: Impressions ÷ Reach | Number | The average number of times each person saw your ad (impressions divided by reach). |
| `facebookAds.uniqueClicks` | Unique link clicks | MAX |  | The number of people who performed at least one link click. Differs from link clicks when the same person clicks multiple times. |
| `facebookAds.ctr` | CTR (all) | Calculated: Link clicks ÷ Impressions | Percentage | The percentage of impressions that received any click type (all clicks divided by impressions). |
| `facebookAds.uniqueCtr` | Unique CTR | Calculated: Unique link clicks ÷ Reach | Percentage | The percentage of people who saw your ad and clicked a link (unique link clicks divided by reach). |
| `facebookAds.cpc` | CPC (all) | Calculated: Amount spent ÷ Link clicks | Currency | The average cost for each click of any type (amount spent divided by all clicks). |
| `facebookAds.cpm` | CPM | Calculated: (Amount spent ÷ Impressions) × 1,000 | Currency | The average cost per 1,000 impressions (amount spent divided by impressions, ×1,000). |
| `facebookAds.cpp` | Cost per purchase | Calculated: Amount spent ÷ Actions | Currency | The average cost for each purchase attributed to your ads. |
| `facebookAds.actions` | Actions | SUM |  | Counts of specific action types on your ads (for example purchases, leads, or other standard events), often used with action breakdowns. |
| `facebookAds.actionValues` | Action value | SUM | Currency | The total monetary value reported for actions on your ads, in your account currency. |
| `facebookAds.costPerActionType` | Cost per action type | AVG | Currency | Average cost for each action of a given type when broken down by action type. |
| `facebookAds.websiteCtr` | Website CTR | Calculated: Link clicks ÷ Impressions | Percentage | The percentage of impressions that resulted in a link click to your website. |
| `facebookAds.reachPerSpend` | Reach per spend | Calculated: Reach ÷ Amount spent |  | People reached per unit of currency spent (reach / spend). Distinct from cpp and other native cost metrics. |
| `facebookAds.uniqueClickSharePercent` | Unique click share % | Calculated: (Unique link clicks ÷ Link clicks) × 100 |  | Percentage of total clicks that came from unique users: unique clicks ÷ total clicks. |

### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `facebookAds.date` | Date | string | Date when the ad metrics were recorded. |
| `facebookAds.dateStop` | End date | string | End date for the reporting period. |
| `facebookAds.campaignName` | Campaign | string | Name of the ad campaign. Shown in campaign reports and breakdowns. |
| `facebookAds.campaignId` | Campaign ID | string | Unique identifier for the ad campaign in Facebook Ads Manager. |
| `facebookAds.adGroupName` | Ad group | string | Name of the ad group (adset). |
| `facebookAds.adGroupId` | Ad group ID | string | Unique identifier for the ad group (adset) in Facebook Ads Manager. |
| `facebookAds.adName` | Ad | string | Name of the ad. Shown in ad-level reports and breakdowns. |
| `facebookAds.adId` | Ad ID | string | Unique identifier for the ad in Facebook Ads Manager. |
| `facebookAds.publisherPlatform` | Platform | string | Meta platform where the ad was shown (Facebook, Instagram, Audience Network, etc.). |
| `facebookAds.devicePlatform` | Device platform | string | Device class where the ad was shown (mobile, desktop, etc.). |
| `facebookAds.country` | Country | string | Country where people who saw your ads were located. |
| `facebookAds.age` | Age | string | Age range of people who saw your ads (used with age breakdowns). |
| `facebookAds.gender` | Gender | string | Gender of people who saw your ads (used with gender breakdowns). |
| `facebookAds.platformPosition` | Platform position | string | Position where the ad was shown within the platform (e.g. feed, stories, right column). |
| `facebookAds.impressionDevice` | Impression device | string | Device on which the ad impression was recorded (e.g. mobile, desktop). |
| `facebookAds.region` | Region | string | Region or state where people who saw your ads were located. |
| `facebookAds.dma` | DMA region | string | Designated market area (US TV markets) for geographic breakdowns. Deprecated by Facebook; requests are routed to comscore_market. |
| `facebookAds.comscoreMarket` | Comscore market | string | Comscore market for US geographic breakdowns (replaces deprecated dma). |
| `facebookAds.hourlyStatsAggregatedByAdvertiserTimeZone` | Hour of day (advertiser time zone) | string | Hour of the day the ad was served, aggregated in the advertiser's time zone. |

## Cross-platform fields

`unified.*` fields add Facebook Ads to totals across all your ad platforms, so one question can cover Google Ads, Meta, Microsoft, LinkedIn and TikTok together.

| Unified field | Label | From |
|---|---|---|
| `unified.spend` | Spend | `facebookAds.spend` |
| `unified.impressions` | Impressions | `facebookAds.impressions` |
| `unified.reach` | Reach | `facebookAds.reach` |
| `unified.clicks` | Clicks | `facebookAds.clicks` |
| `unified.ctr` | CTR | `facebookAds.ctr` |
| `unified.cpm` | CPM | `facebookAds.cpm` |
| `unified.cpc` | CPC | `facebookAds.cpc` |
| `unified.frequency` | Frequency | `facebookAds.frequency` |
| `unified.date` | Date | `facebookAds.date` |
| `unified.campaignName` | Campaign | `facebookAds.campaignName` |
| `unified.campaignId` | Campaign ID | `facebookAds.campaignId` |
| `unified.adGroupName` | Ad group | `facebookAds.adGroupName` |
| `unified.adGroupId` | Ad group ID | `facebookAds.adGroupId` |
| `unified.adName` | Ad | `facebookAds.adName` |
| `unified.adId` | Ad ID | `facebookAds.adId` |
| `unified.placement` | Placement | `facebookAds.publisherPlatform` |
| `unified.country` | Country | `facebookAds.country` |
| `unified.device` | Device | `facebookAds.devicePlatform` |

## Links

- [Facebook Ads on jepto.com](https://www.jepto.com/data-sources/facebook-ads)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
