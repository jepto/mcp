# Google Analytics 4 MCP server

Connect Google Analytics 4 to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about users, sessions, engagement, conversions and revenue, broken down by channel, source, landing page, device and location.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your GA4 data comes from the GA4 account you've connected in Jepto, with the same 30 metrics and 28 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "Which landing pages drove the most conversions last month?"
- "Compare sessions and conversion rate by default channel group, this quarter vs last."
- "What's my purchase revenue by session source for the last 30 days?"
- "Which cities have the highest engagement rate on mobile?"

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

Google Analytics 4 is read-only through the MCP: the assistant can query your data but can't change anything in GA4.

## Example query

This is the kind of `Analytics_Query` call an assistant makes for you. Field names are the same as in Jepto Reports and Data Chat.

```json
{
  "clientId": "<clientId from Client_List>",
  "measures": [
    "googleAnalytics4.sessions",
    "googleAnalytics4.engagementRate",
    "googleAnalytics4.conversions"
  ],
  "dimensions": [
    "googleAnalytics4.sessionDefaultChannelGroup"
  ],
  "dateRange": [
    "last30days"
  ],
  "order": {
    "googleAnalytics4.sessions": "desc"
  }
}
```

## Fields

58 fields (30 metrics, 28 dimensions), from version 1.1.2 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `googleAnalytics4.activeUsers` | Active users | MAX |  | Active users. Users who had an engaged session, or when Analytics collected first_visit or engagement_time from a page. |
| `googleAnalytics4.totalUsers` | Total users | MAX |  | Users who logged at least one event, new or returning. |
| `googleAnalytics4.newUsers` | New users | MAX |  | Users who interacted with your site or app for the first time. |
| `googleAnalytics4.sessions` | Sessions | SUM |  | Periods when users are actively engaged with your site or app. |
| `googleAnalytics4.sessionsPerUser` | Sessions per user | SUM |  | Average number of sessions per user. |
| `googleAnalytics4.engagedSessions` | Engaged sessions | SUM |  | Sessions that lasted longer than 10 seconds, had a conversion event, or had at least 2 page views. |
| `googleAnalytics4.screenPageViews` | Page views | SUM |  | Total count of screen or page views recorded. |
| `googleAnalytics4.eventCount` | Events | SUM |  | Count of event instances recorded. |
| `googleAnalytics4.eventValue` | Event value | SUM | Currency | Total monetary value attributed to user interactions with your site or app. |
| `googleAnalytics4.itemViews` | Item views | SUM |  | Total count of item views recorded. |
| `googleAnalytics4.addToCarts` | Add-to-carts | SUM |  | Number of times users added items to their shopping cart. |
| `googleAnalytics4.itemsPurchased` | Items purchased | SUM |  | Number of items purchased by users. |
| `googleAnalytics4.purchaseRevenue` | Purchase revenue | SUM | Currency | Total revenue generated from purchases, in your account currency. |
| `googleAnalytics4.totalRevenue` | Total revenue | SUM | Currency | Total revenue across all transaction types recorded, in your account currency. |
| `googleAnalytics4.transactionsPerPurchaser` | Transactions per purchaser | SUM |  | Average number of transactions per user who made a purchase. |
| `googleAnalytics4.averagePurchaseRevenue` | Avg. purchase revenue | SUM | Currency | Average revenue generated per purchase transaction. |
| `googleAnalytics4.purchaserRate` | Purchaser rate | AVG | Percentage | Percentage of active users who made a purchase: purchasers ÷ active users. |
| `googleAnalytics4.conversions` | Conversions | SUM |  | Number of completed conversion actions you are measuring (e.g. purchases, sign-ups, leads). |
| `googleAnalytics4.engagementRate` | Engagement rate | AVG | Percentage | Percentage of sessions that were engaged (lasted 10+ seconds, had a conversion, or had 2+ page views): engaged sessions ÷ total sessions. |
| `googleAnalytics4.userEngagementDuration` | User engagement duration | AVG | Decimal (×) | Total time users spent engaged with your site or app, in seconds. |
| `googleAnalytics4.averageSessionDuration` | Avg. session duration | AVG | Decimal (×) | Average duration of each session in seconds. |
| `googleAnalytics4.bounceRate` | Bounce rate | AVG | Percentage | Percentage of sessions that were not engaged sessions: non-engaged sessions ÷ total sessions. |
| `googleAnalytics4.totalAdCost` | Total ad cost | SUM | Currency | Total amount spent on linked ads (e.g. Google Ads), in your account currency. |
| `googleAnalytics4.advertiserAdClicks` | Ad clicks | SUM |  | Number of clicks on your ads. |
| `googleAnalytics4.advertiserAdImpressions` | Ad impressions | SUM |  | Number of times your ads were displayed to users. |
| `googleAnalytics4.advertiserAdCostPerClick` | Ad CPC | AVG | Currency | Average cost per click on your ads, calculated as total ad cost divided by clicks. |
| `googleAnalytics4.returnOnAdSpend` | ROAS | AVG | Decimal (×) | Return on ad spend: attributed revenue divided by ad cost. |
| `googleAnalytics4.conversionRate` | Conversion rate | Calculated: Conversions ÷ Sessions | Percentage | Percentage of sessions that resulted in a conversion: conversions ÷ sessions. |
| `googleAnalytics4.advertiserAdCtr` | Ad CTR | Calculated: Ad clicks ÷ Ad impressions | Percentage | Click-through rate for linked ads: ad clicks ÷ ad impressions. |
| `googleAnalytics4.adCostPerConversion` | Cost per conversion | Calculated: Total ad cost ÷ Conversions | Currency | Average ad spend per conversion: total ad cost ÷ conversions. |

### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `googleAnalytics4.date` | Date | string | Calendar date for the report row. |
| `googleAnalytics4.isoYearIsoWeek` | ISO year + week | string | ISO year and week number for the report row (e.g. 202305 = week 5 of 2023). |
| `googleAnalytics4.year` | Year | string | Year of the report row. Used when reporting is grouped by calendar year. |
| `googleAnalytics4.yearMonth` | Year + month | string | Year and month of the report row (YYYYMM). |
| `googleAnalytics4.month` | Month | string | Month of the report row (01–12). |
| `googleAnalytics4.week` | Week | string | Week of the year (01–53). |
| `googleAnalytics4.dateHour` | Date + hour | string | Date and hour. Combined date and hour (YYYYMMDDHH). |
| `googleAnalytics4.dateHourMinute` | Date + hour + minute | string | Date, hour, and minute of the report row (YYYYMMDDHHmm). |
| `googleAnalytics4.country` | Country | string | Country associated with user activity. |
| `googleAnalytics4.region` | Region | string | Geographic region for user activity. |
| `googleAnalytics4.city` | City | string | City associated with user activity. |
| `googleAnalytics4.deviceCategory` | Device category | string | Desktop, mobile, or tablet. |
| `googleAnalytics4.platform` | Platform | string | Platform used for the session (Web, iOS, Android). |
| `googleAnalytics4.platformDeviceCategory` | Platform + device | string | Platform and device category combined (e.g. web / mobile, iOS / tablet). |
| `googleAnalytics4.browser` | Browser | string | Web browser used for the session. |
| `googleAnalytics4.operatingSystem` | Operating system | string | Operating system of the device used in the session (e.g. Windows, iOS, Android). |
| `googleAnalytics4.sessionSource` | Session source | string | Traffic source that brought the session (e.g. google, direct, newsletter). |
| `googleAnalytics4.sessionMedium` | Session medium | string | Traffic medium for the session (e.g. cpc, organic, email). |
| `googleAnalytics4.sessionCampaignName` | Session campaign | string | Campaign name that brought the session. |
| `googleAnalytics4.sessionDefaultChannelGroup` | Default channel group | string | Marketing channel group for the session (e.g. Paid Search, Organic). |
| `googleAnalytics4.firstUserSource` | First user source | string | Traffic source that first acquired the user (e.g. google, direct). |
| `googleAnalytics4.firstUserMedium` | First user medium | string | Traffic medium through which the user was first acquired (e.g. cpc, organic). |
| `googleAnalytics4.currencyCode` | Currency code | string | Currency code for the transaction data. |
| `googleAnalytics4.eventName` | Event name | string | Name of the collected event. |
| `googleAnalytics4.pagePath` | Page path | string | Path portion of the page URL. |
| `googleAnalytics4.pageTitle` | Page title | string | HTML document title of the page as collected by Google Analytics. |
| `googleAnalytics4.landingPage` | Landing page | string | Page path where the session began. |
| `googleAnalytics4.unifiedPagePathScreen` | Page path / screen name | string | Unified path for both web pages and app screens. |

## Cross-platform fields

`unified.*` fields add Google Analytics 4 to totals across all your ad platforms, so one question can cover Google Ads, Meta, Microsoft, LinkedIn and TikTok together.

| Unified field | Label | From |
|---|---|---|
| `unified.spend` | Spend | `googleAnalytics4.totalAdCost` |
| `unified.impressions` | Impressions | `googleAnalytics4.advertiserAdImpressions` |
| `unified.reach` | Reach | `googleAnalytics4.activeUsers` |
| `unified.clicks` | Clicks | `googleAnalytics4.advertiserAdClicks` |
| `unified.ctr` | CTR | `googleAnalytics4.advertiserAdCtr` |
| `unified.cpc` | CPC | `googleAnalytics4.advertiserAdCostPerClick` |
| `unified.conversions` | Conversions | `googleAnalytics4.conversions` |
| `unified.costPerConversion` | Cost per conversion | `googleAnalytics4.adCostPerConversion` |
| `unified.roas` | ROAS | `googleAnalytics4.returnOnAdSpend` |
| `unified.date` | Date | `googleAnalytics4.date` |
| `unified.campaignName` | Campaign | `googleAnalytics4.sessionCampaignName` |
| `unified.country` | Country | `googleAnalytics4.country` |
| `unified.device` | Device | `googleAnalytics4.deviceCategory` |

## Links

- [Google Analytics 4 on jepto.com](https://www.jepto.com/data-sources/google-analytics-4)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
