# Instagram Ads MCP server

Connect Instagram Ads to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about the Meta ads that run on Instagram: amount spent, reach, frequency, link clicks and results by campaign, placement, age, gender and region.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your Instagram Ads data comes from the Instagram Ads account you've connected in Jepto, with the same 20 metrics and 18 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "Which Instagram placements had the lowest CPM last month?"
- "Show reach and frequency by age group for the last 30 days."
- "Which ads got the most link clicks this week?"

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

Instagram Ads is read-only through the MCP: the assistant can query your data but can't change anything in Instagram Ads.

## Fields

38 fields (20 metrics, 18 dimensions), from version 1.2.3 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `instagramAds.impressions` | Impressions | SUM |  | The number of times your ads were on screen, including when the same person saw your ad more than once. |
| `instagramAds.clicks` | Link clicks | SUM |  | Clicks on links in your ads that go to destinations such as your website, app, or a Meta experience. This is the primary “clicks” metric in Meta Ads Manager reports. |
| `instagramAds.spend` | Amount spent | SUM | Currency | Estimated total amount spent on your ads during the reporting period, in your account currency. |
| `instagramAds.reach` | Reach | MAX |  | The number of people who saw your ads at least once. Reach is an estimated metric. |
| `instagramAds.frequency` | Frequency | AVG |  | The average number of times each person saw your ad (impressions divided by reach). |
| `instagramAds.uniqueClicks` | Unique link clicks | MAX |  | The number of people who performed at least one link click. Differs from link clicks when the same person clicks multiple times. |
| `instagramAds.ctr` | CTR (all) | AVG | Percentage | The percentage of impressions that received any click type (all clicks divided by impressions). |
| `instagramAds.uniqueCtr` | Unique CTR | AVG | Percentage | The percentage of people who saw your ad and clicked a link (unique link clicks divided by reach). |
| `instagramAds.cpc` | CPC (all) | AVG | Currency | The average cost for each click of any type (amount spent divided by all clicks). |
| `instagramAds.cpm` | CPM | AVG | Currency | The average cost per 1,000 impressions (amount spent divided by impressions, ×1,000). |
| `instagramAds.cpp` | Cost per purchase | SUM | Currency | The average cost for each purchase attributed to your ads. |
| `instagramAds.actions` | Actions | SUM |  | Counts of specific action types on your ads (for example purchases, leads, or other standard events), often used with action breakdowns. |
| `instagramAds.actionValues` | Action value | SUM | Currency | The total monetary value reported for actions on your ads, in your account currency. |
| `instagramAds.costPerActionType` | Cost per action type | AVG | Currency | Average cost for each action of a given type when broken down by action type. |
| `instagramAds.websiteCtr` | Website CTR | AVG | Percentage | The percentage of impressions that resulted in a link click to your website. |
| `instagramAds.uniqueClickShare` | Unique click share | Calculated: Unique link clicks ÷ Link clicks |  | Share of clicks from unique users: unique clicks ÷ total clicks. |
| `instagramAds.reachPerSpend` | Reach per spend | Calculated: Reach ÷ Amount spent |  | Number of unique people reached per dollar of spend. |
| `instagramAds.uniqueToAllCtrRatio` | Unique to all CTR ratio | Calculated: Unique CTR ÷ CTR (all) |  | Ratio of unique CTR to all-click CTR: unique CTR ÷ CTR. Indicates how much of your click traffic comes from distinct individuals. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `instagramAds.uniqueClickSharePercent` | Unique click share % | Calculated: Unique click share × 100 |  | Percentage of total clicks that came from unique users. |
| `instagramAds.aggregatedUniqueClickShare` | Aggregated unique click share | Calculated: Unique link clicks ÷ Link clicks |  | Unique click share aggregated across all rows in the dataset (unique clicks ÷ total clicks). |

### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `instagramAds.date` | Date | string | Date when the ad metrics were recorded. |
| `instagramAds.dateStop` | End date | string | End date for the reporting period. |
| `instagramAds.campaignName` | Campaign | string | Name of the ad campaign. Shown in campaign reports and breakdowns. |
| `instagramAds.campaignId` | Campaign ID | string | Unique identifier for the campaign in Meta Ads Manager. |
| `instagramAds.adGroupName` | Ad group | string | Name of the ad group (adset). |
| `instagramAds.adGroupId` | Ad group ID | string | Unique identifier for the ad group (adset) in Meta Ads Manager. |
| `instagramAds.adName` | Ad | string | Name of the ad. Shown in ad-level reports and breakdowns. |
| `instagramAds.adId` | Ad ID | string | Unique identifier for the ad in Meta Ads Manager. |
| `instagramAds.publisherPlatform` | Platform | string | Meta platform where the ad was shown (Facebook, Instagram, Audience Network, etc.). |
| `instagramAds.devicePlatform` | Device platform | string | Device class where the ad was shown (mobile, desktop, etc.). |
| `instagramAds.country` | Country | string | Country where people who saw your ads were located. |
| `instagramAds.age` | Age | string | Age range of people who saw your ads. |
| `instagramAds.gender` | Gender | string | Gender of people who saw your ads. |
| `instagramAds.platformPosition` | Platform position | string | Position where the ad was shown within the platform (e.g. feed, stories, right column). |
| `instagramAds.impressionDevice` | Impression device | string | Device on which the ad impression was recorded (e.g. mobile, desktop). |
| `instagramAds.region` | Region | string | Region or state where people who saw your ads were located. |
| `instagramAds.dma` | DMA region | string | Designated market area (US TV markets) for geographic breakdowns. |
| `instagramAds.hourlyStatsAggregatedByAdvertiserTimeZone` | Hour of day (advertiser time zone) | string | Hour of the day the ad was served, aggregated in the advertiser's time zone. |

## FAQ

**Is there an Instagram Ads MCP server?**
Yes. Jepto hosts an MCP server at https://mcp.jepto.com that gives Claude, ChatGPT, Cursor and other MCP clients access to your Instagram Ads data. There's nothing to install: you sign in to Jepto with OAuth, and the assistant can query every Instagram Ads field in the Jepto data model.

**Which Instagram Ads metrics can I report on?**
Jepto includes Instagram Ads metrics such as Impressions, Link clicks, Amount spent, Reach, Frequency, Unique link clicks, CTR (all) and Unique CTR. You can break them down by dimensions for Campaign, ad set & ad, Date & time, Location, Device & placement and Segments. Every field, with its definition, is listed on this page.

**Can an AI assistant make changes in Instagram Ads?**
No. Through the Jepto MCP, Instagram Ads data is read-only: the assistant can query it but can't change anything in Instagram Ads.

**Can I see feed, stories and reels separately?**
Yes. The Platform position dimension breaks delivery down by placement, so you can compare feed, stories and reels.

**Can I combine Instagram Ads with Google Ads in one report?**
Yes. When both are connected to the same brand, any report or chart can use fields from either source, and Data Chat and the Jepto MCP can answer questions that span the two. Cross-platform fields such as unified.spend also add Instagram Ads to totals across your other ad platforms.

## Links

- [Instagram Ads on jepto.com](https://www.jepto.com/data-sources/instagram-ads)
- [Compare marketing MCP servers](https://www.jepto.com/compare/marketing-mcp-servers)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
