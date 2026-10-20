# Microsoft Ads MCP server

Connect Microsoft Ads to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about your Microsoft Advertising (Bing) campaigns, ad groups and ads: spend, conversions, revenue, impression share and Quality Score.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your Microsoft Ads data comes from the Microsoft Ads account you've connected in Jepto, with the same 85 metrics and 41 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "Which campaigns lost the most impression share to budget last month?"
- "Show Quality Score, expected CTR and landing page experience by ad group."
- "What's ROAS by device type this month?"

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

Microsoft Ads is read-only through the MCP: the assistant can query your data but can't change anything in Microsoft Ads.

## Fields

126 fields (85 metrics, 41 dimensions), from version 1.0.2 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `microsoftAds.impressions` | Impressions | SUM |  | Number of times your ads were shown, including repeat views by the same person. |
| `microsoftAds.clicks` | Clicks | SUM |  | Number of clicks on your ads during the reporting period. |
| `microsoftAds.ctr` | CTR | AVG | Percentage | Click-through rate: clicks divided by impressions, shown as a percentage. |
| `microsoftAds.spend` | Spend | SUM | Currency | Total advertising spend during the reporting period, in your account currency. |
| `microsoftAds.avgCpc` | Avg. CPC | AVG | Currency | Average cost per click: spend divided by clicks, in account currency. |
| `microsoftAds.avgCpm` | Avg. CPM | AVG | Currency | Average cost per 1,000 impressions: spend divided by impressions (×1,000), in account currency. |
| `microsoftAds.avgPosition` | Avg. position | AVG |  | Average position of your ad on the search results page during the reporting period. |
| `microsoftAds.conversions` | Conversions | SUM |  | Number of completed conversion actions you are measuring (e.g. purchases, sign-ups, leads). |
| `microsoftAds.conversionRate` | Conversion rate | AVG | Percentage | Conversion rate as percentage: (Conversions / Clicks) × 100. |
| `microsoftAds.costPerConversion` | Cost per conversion | AVG | Currency | Average spend for each conversion: spend divided by the number of conversion events. |
| `microsoftAds.viewThroughConversions` | View-through conversions | SUM |  | Number of conversions from users who saw your ad but didn't click, then converted within the attribution window. |
| `microsoftAds.viewThroughConversionsQualified` | View-through conversions (qualified) | SUM |  | Qualified view-through conversions including partial values from external attribution. |
| `microsoftAds.viewThroughRate` | View-through rate | AVG | Percentage | Percentage of impressions that resulted in a view-through conversion: view-through conversions ÷ impressions. |
| `microsoftAds.viewThroughRevenue` | View-through revenue | SUM | Currency | Revenue attributed to view-through conversions during the reporting period. |
| `microsoftAds.conversionValue` | Conversion Value | SUM | Currency | Total conversion value reported for attributed conversion events, in your account currency. Only populated when conversion tracking sends a value. |
| `microsoftAds.revenueSpendRatio` | ROAS | AVG | Decimal (×) | Return on ad spend: total revenue divided by total advertising spend. |
| `microsoftAds.revenuePerConversion` | Revenue per conversion | AVG | Currency | Average revenue per conversion: total conversion value ÷ conversions. |
| `microsoftAds.assists` | Assists | SUM |  | Number of times a click on your ad preceded a conversion that was ultimately attributed to a different ad. |
| `microsoftAds.costPerAssist` | Cost per assist | AVG | Currency | Average spend for each assist: spend divided by the number of assist events. |
| `microsoftAds.revenuePerAssist` | Revenue per assist | AVG | Currency | Average revenue per assist: total revenue ÷ assists. |
| `microsoftAds.impressionSharePercent` | Impression share | AVG | Percentage | Percentage of available impressions your ads received: impressions ÷ total eligible impressions. |
| `microsoftAds.impressionLostToBudgetPercent` | Impression share lost (budget) | AVG | Percentage | Percentage of eligible impressions your ads missed because your budget was too low. |
| `microsoftAds.impressionLostToRankPercent` | Impression share lost (rank) | AVG | Percentage | Percentage of eligible impressions your ads missed due to poor ad rank. |
| `microsoftAds.qualityScore` | Quality Score | SUM |  | Measures how relevant your ad, keywords, and landing page are to users. |
| `microsoftAds.expectedCtr` | Expected CTR | AVG | Percentage | Expected click-through rate component of quality score (1–3: Below Average, Average, Above Average). |
| `microsoftAds.adRelevance` | Ad relevance | SUM |  | Ad relevance score indicating how closely your ad matches user intent: 1=Below Average, 2=Average, 3=Above Average. |
| `microsoftAds.landingPageExperience` | Landing page experience | SUM |  | Landing page relevance score: 1=Below Average, 2=Average, 3=Above Average. |
| `microsoftAds.lowQualityClicks` | Low quality clicks | SUM |  | Number of clicks on your ads flagged as low quality (invalid or fraudulent). |
| `microsoftAds.lowQualityClicksPercent` | Low quality click rate | AVG | Percentage | Percentage of total clicks flagged as low quality: low quality clicks ÷ total clicks. |
| `microsoftAds.lowQualityImpressions` | Low quality impressions | SUM |  | Number of impressions flagged as low quality. |
| `microsoftAds.lowQualityImpressionsPercent` | Low quality impression rate | AVG | Percentage | Percentage of total impressions flagged as low quality: low quality impressions ÷ total impressions. |
| `microsoftAds.phoneImpressions` | Phone impressions | SUM |  | Number of times your phone number was displayed with your ad. |
| `microsoftAds.phoneCalls` | Phone calls | SUM |  | Number of calls made to the phone number displayed in your ad. |
| `microsoftAds.ptr` | Phone-through rate | SUM |  | Phone-through rate: percentage of phone impressions that resulted in a phone call. |
| `microsoftAds.absoluteTopImpressionRatePercent` | Abs. top impression rate | AVG | Percentage | Percentage of your impressions that appeared in the absolute top position (above all search results). |
| `microsoftAds.topImpressionRatePercent` | Top impression rate | AVG | Percentage | Percentage of your impressions that appeared in a top position above organic search results. |
| `microsoftAds.absoluteTopImpressionSharePercent` | Abs. top impression share | AVG | Percentage | Percentage of total eligible impressions your ads received in the absolute top position. |
| `microsoftAds.topImpressionSharePercent` | Top impression share | AVG | Percentage | Percentage of total eligible impressions your ads received in a top position above organic search results. |
| `microsoftAds.clickSharePercent` | Click share | AVG | Percentage | Percentage of total eligible clicks your ads received: clicks ÷ total eligible clicks. |
| `microsoftAds.exactMatchImpressionSharePercent` | Exact match impression share | AVG | Percentage | Percentage of eligible exact-match impressions your ads received for searches that exactly matched your keywords. |
| `microsoftAds.conversionsQualified` | Conversions (qualified) | SUM |  | Conversions including partial externally attributed offline conversions. |
| `microsoftAds.allConversionsQualified` | All conversions (qualified) | SUM |  | All conversions including partial values, regardless of ExcludeFromBidding setting. |
| `microsoftAds.allConversionRate` | All conversion rate | AVG | Percentage | Percentage of clicks that resulted in any conversion (all types): all conversions ÷ clicks. |
| `microsoftAds.allCostPerConversion` | All cost per conversion | AVG | Currency | Average spend per all-type conversion: spend ÷ all conversions. |
| `microsoftAds.allRevenue` | All revenue | SUM | Currency | Total revenue attributed to all conversion types during the reporting period. |
| `microsoftAds.allReturnOnAdSpend` | All ROAS | AVG | Decimal (×) | Return on ad spend including all conversion types: all revenue ÷ spend. |
| `microsoftAds.allRevenuePerConversion` | All revenue per conversion | AVG | Currency | Average revenue per all-type conversion: all revenue ÷ all conversions. |
| `microsoftAds.videoViews` | Video views | SUM |  | Number of times your video ad was viewed. |
| `microsoftAds.videoViewsAt25Percent` | Video views 25% | SUM | Percentage | Number of times your video ad was watched to at least 25% completion. |
| `microsoftAds.videoViewsAt50Percent` | Video views 50% | SUM | Percentage | Number of times your video ad was watched to at least 50% completion. |
| `microsoftAds.videoViewsAt75Percent` | Video views 75% | SUM | Percentage | Number of times your video ad was watched to at least 75% completion. |
| `microsoftAds.completedVideoViews` | Video completions | SUM |  | Number of times your video ad was watched to completion. |
| `microsoftAds.videoCompletionRate` | Video completion rate | AVG | Percentage | Percentage of video views that were watched to completion: video completions ÷ video views. |
| `microsoftAds.avgCpv` | Avg. CPV | AVG | Currency | Average cost per video view: spend ÷ video views. |
| `microsoftAds.avgWatchTimePerVideoView` | Avg. watch time per view | AVG |  | Average time users spent watching your video ad per view. |
| `microsoftAds.avgWatchTimePerImpression` | Avg. watch time per impression | AVG |  | Average time users spent watching your video ad per impression. |
| `microsoftAds.sales` | Sales | SUM |  | Number of sales events attributed to your ads. |
| `microsoftAds.costPerSale` | Cost per sale | AVG | Currency | Average spend for each sale: spend divided by the number of sale events. |
| `microsoftAds.revenuePerSale` | Revenue per sale | AVG | Currency | Average revenue per sale event: total revenue ÷ sales. |
| `microsoftAds.installs` | Installs | SUM |  | Number of app installs attributed to the ad. |
| `microsoftAds.costPerInstall` | Cost per install | AVG | Currency | Average spend for each install: spend divided by the number of install events. |
| `microsoftAds.revenuePerInstall` | Revenue per install | AVG | Currency | Average revenue per install: total revenue ÷ installs. |
| `microsoftAds.appInstalls` | App installs | SUM |  | Number of app installs attributed to your ads. |
| `microsoftAds.relativeCtr` | Relative CTR | AVG | Percentage | Your CTR compared to the average CTR of all other advertisers on the same websites: your CTR ÷ competitor average CTR. |
| `microsoftAds.conversionRateByClicksPct` | Conversion rate (by clicks) | Calculated: (Conversions ÷ Clicks) × 100 |  | Conversion rate (%) from clicks; same basis as API ConversionRate, as an explicit calculated measure. For session-based rates, join session data from another source—Microsoft Ads reports do not expose sessions. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.conversionRateByImpressionPct` | Conversion rate (by impressions) | Calculated: (Conversions ÷ Impressions) × 100 |  | Percentage of impressions that resulted in a conversion: conversions ÷ impressions. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.allConversionRateByImpressionPct` | All conversion rate (by impressions) | Calculated: (All conversions (qualified) ÷ Impressions) × 100 |  | Percentage of impressions that resulted in any conversion (all types): all conversions ÷ impressions. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.revenuePerClick` | Revenue per click | Calculated: Conversion Value ÷ Clicks |  | Average revenue per click: total conversion value ÷ clicks. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.revenuePerImpression` | Revenue per impression | Calculated: Conversion Value ÷ Impressions |  | Average revenue per impression: total conversion value ÷ impressions. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.netConversionValue` | Net conversion value | Calculated: Conversion Value − Spend |  | Profit from conversions after deducting ad spend: conversion value − spend. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.conversionValueMarginPct` | Conversion value margin | Calculated: ((Conversion Value − Spend) ÷ Conversion Value) × 100 |  | Percentage of conversion value remaining after deducting ad spend: (conversion value − spend) ÷ conversion value. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.clickThroughRatio` | Click-through ratio | Calculated: Clicks ÷ Impressions |  | Click-through ratio: clicks ÷ impressions, expressed as a decimal ratio. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.costPerMilleFromSpend` | CPM (from spend) | Calculated: (Spend ÷ Impressions) × 1,000 |  | Average cost per 1,000 impressions: spend ÷ impressions × 1,000. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.assistsPerConversion` | Assists per conversion | Calculated: Assists ÷ Conversions |  | Average number of assist clicks per recorded conversion: assists ÷ conversions. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.assistsPerClick` | Assists per click | Calculated: Assists ÷ Clicks |  | Average number of assist clicks per total click: assists ÷ clicks. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.viewThroughToClickConversionRatio` | View-through to click conversion ratio | Calculated: View-through conversions ÷ Conversions |  | Ratio of view-through conversions to click-based conversions: view-through conversions ÷ conversions. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.allConversionsPerClick` | All conversions per click | Calculated: All conversions (qualified) ÷ Clicks |  | All conversions per click: all qualified conversions ÷ clicks. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.installRatePct` | Install rate | Calculated: (Installs ÷ Clicks) × 100 |  | Percentage of clicks that resulted in an install: installs ÷ clicks. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.appInstallRatePct` | App install rate | Calculated: (App installs ÷ Clicks) × 100 |  | Percentage of clicks that resulted in an app install: app installs ÷ clicks. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.salesRatePct` | Sales rate | Calculated: (Sales ÷ Clicks) × 100 |  | Percentage of clicks that resulted in a sale: sales ÷ clicks. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.videoViewsPerImpressionPct` | Video view rate | Calculated: (Video views ÷ Impressions) × 100 |  | Percentage of impressions that resulted in a video view: video views ÷ impressions. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.completedVideoViewsPerImpressionPct` | Video completion rate (by impressions) | Calculated: (Video completions ÷ Impressions) × 100 |  | Percentage of impressions that resulted in a completed video view: video completions ÷ impressions. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.avgWatchTimeMsPerImpressionDerived` | Avg. watch time per impression | Calculated |  | Average total watch time in ms per impression (derived). |
| `microsoftAds.lowQualityClickRatePct` | Low quality click rate (calculated) | Calculated: (Low quality clicks ÷ Clicks) × 100 |  | Percentage of total clicks flagged as low quality: low quality clicks ÷ total clicks. Calculated in Jepto from other fields in this report using the formula on the metric. |
| `microsoftAds.phoneThroughRatePctDerived` | Phone-through rate | Calculated: (Phone calls ÷ Phone impressions) × 100 |  | Percentage of phone impressions that resulted in a phone call: phone calls ÷ phone impressions. Calculated in Jepto from other fields in this report using the formula on the metric. |

### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `microsoftAds.date` | Date | string | Date of the reporting period. |
| `microsoftAds.accountId` | Account ID | string | Unique identifier for the Microsoft Advertising account. |
| `microsoftAds.accountName` | Account name | string | Name of the Microsoft Advertising account. |
| `microsoftAds.accountStatus` | Account status | string | Current delivery status in the advertising platform (for example enabled, paused, or removed). |
| `microsoftAds.campaignId` | Campaign ID | string | Unique identifier for the campaign. |
| `microsoftAds.campaignName` | Campaign | string | Name of the campaign. Shown in campaign reports and breakdowns. |
| `microsoftAds.campaignStatus` | Campaign status | string | Current delivery status in the advertising platform (for example enabled, paused, or removed). |
| `microsoftAds.campaignType` | Campaign type | string | Type of campaign (e.g. Search, Shopping, Audience, Dynamic Search Ads). |
| `microsoftAds.adGroupId` | Ad group ID | string | Unique identifier for the ad group. |
| `microsoftAds.adGroupName` | Ad group | string | Name of the ad group. Shown in ad group reports and breakdowns. |
| `microsoftAds.adGroupStatus` | Ad group status | string | Current delivery status of the ad group (e.g. Active, Paused, Deleted). |
| `microsoftAds.adId` | Ad ID | string | Unique identifier for the ad. |
| `microsoftAds.adName` | Ad name | string | Title or headline of the ad. |
| `microsoftAds.adStatus` | Ad status | string | Current delivery status in the advertising platform (for example enabled, paused, or removed). |
| `microsoftAds.adType` | Ad type | string | Type of ad (e.g. Expanded Text Ad, Responsive Search Ad, Dynamic Search Ad). |
| `microsoftAds.adDistribution` | Ad distribution | string | Where your ad was distributed (e.g. Search, Content, Mixed). |
| `microsoftAds.deviceType` | Device type | string | Type of device your ad was shown on (e.g. Computer, Smartphone, Tablet). |
| `microsoftAds.deviceOs` | Device OS | string | Operating system of the device your ad was shown on (e.g. Windows, iOS, Android). |
| `microsoftAds.language` | Language | string | Language of the user query or ad targeting at the time of impression. |
| `microsoftAds.network` | Network | string | Search network where your ad was shown (e.g. Bing Search, Bing Search Partners, Bing and AOL). |
| `microsoftAds.topVsOther` | Top vs other | string | Whether your ad appeared in a top position above organic results or in another position. |
| `microsoftAds.bidMatchType` | Bid match type | string | The match type you bid on for this keyword (Exact, Phrase, Broad). |
| `microsoftAds.deliveredMatchType` | Delivered match type | string | The actual match type that triggered your ad for a given search (Exact, Phrase, Broad). |
| `microsoftAds.currencyCode` | Currency code | string | Currency code for the account (e.g. USD, EUR, AUD). |
| `microsoftAds.goal` | Goal | string | Name of the conversion goal attributed to this result. |
| `microsoftAds.goalType` | Goal type | string | Type of conversion goal (e.g. URL, Duration, Pages Per Visit, Event, App Install). |
| `microsoftAds.destinationUrl` | Destination URL | string | The URL of the webpage where users land after clicking the ad. |
| `microsoftAds.displayUrl` | Display URL | string | The URL displayed in your ad creative. |
| `microsoftAds.finalUrl` | Final URL | string | The final landing page URL users were directed to after clicking your ad. |
| `microsoftAds.adDescription` | Ad description | string | The description text of your ad. |
| `microsoftAds.adDescription2` | Ad description 2 | string | The second description line of your ad. |
| `microsoftAds.titlePart1` | Title part 1 | string | First part of the ad title. |
| `microsoftAds.titlePart2` | Title part 2 | string | Second part of the ad title. |
| `microsoftAds.titlePart3` | Title part 3 | string | Third part of the ad title. |
| `microsoftAds.headline` | Headline | string | Headline text of your ad. |
| `microsoftAds.longHeadline` | Long headline | string | Long headline text used in multimedia or responsive ads. |
| `microsoftAds.path1` | Path 1 | string | First display path appended to your URL in the ad. |
| `microsoftAds.path2` | Path 2 | string | Second display path appended to your URL in the ad. |
| `microsoftAds.businessName` | Business name | string | Business name shown in your ad. |
| `microsoftAds.customerId` | Customer ID | string | Unique identifier for the Microsoft Advertising customer (manager account). |
| `microsoftAds.customerName` | Customer name | string | Name of the Microsoft Advertising customer (manager account). |

## Cross-platform fields

`unified.*` fields add Microsoft Ads to totals across all your ad platforms, so one question can cover Google Ads, Meta, Microsoft, LinkedIn and TikTok together.

| Unified field | Label | From |
|---|---|---|
| `unified.spend` | Spend | `microsoftAds.spend` |
| `unified.impressions` | Impressions | `microsoftAds.impressions` |
| `unified.clicks` | Clicks | `microsoftAds.clicks` |
| `unified.ctr` | CTR | `microsoftAds.ctr` |
| `unified.cpm` | CPM | `microsoftAds.avgCpm` |
| `unified.cpc` | CPC | `microsoftAds.avgCpc` |
| `unified.conversions` | Conversions | `microsoftAds.conversions` |
| `unified.costPerConversion` | Cost per conversion | `microsoftAds.costPerConversion` |
| `unified.viewThroughConversions` | View-through conversions | `microsoftAds.viewThroughConversions` |
| `unified.conversionValue` | Conversion value | `microsoftAds.conversionValue` |
| `unified.roas` | ROAS | `microsoftAds.revenueSpendRatio` |
| `unified.date` | Date | `microsoftAds.date` |
| `unified.campaignName` | Campaign | `microsoftAds.campaignName` |
| `unified.campaignId` | Campaign ID | `microsoftAds.campaignId` |
| `unified.adGroupName` | Ad group | `microsoftAds.adGroupName` |
| `unified.adGroupId` | Ad group ID | `microsoftAds.adGroupId` |
| `unified.adName` | Ad | `microsoftAds.adName` |
| `unified.adId` | Ad ID | `microsoftAds.adId` |
| `unified.placement` | Placement | `microsoftAds.adDistribution` |
| `unified.device` | Device | `microsoftAds.deviceType` |

## Links

- [Microsoft Ads on jepto.com](https://www.jepto.com/data-sources/microsoft-ads)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
