# Google Ads MCP server

Connect Google Ads to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about campaigns, ad groups, keywords, search terms, Performance Max asset groups and Shopping products in plain English.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your Google Ads data comes from the Google Ads account you've connected in Jepto, with the same 12 metrics and 69 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "Which campaigns had the highest cost per conversion last week?"
- "Show search terms with spend over $100 and no conversions in the last 30 days."
- "What's ROAS by campaign type this month?"
- "Which Performance Max asset groups drove the most conversion value last month?"
- "Compare CTR and avg. CPC by device for my brand campaign over the last 14 days."

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

## Actions

Besides reading data, the Jepto MCP can make changes in Google Ads. Every action shows a preview first, and nothing changes until you confirm it in the chat.

| Action | What it does |
|---|---|
| `googleAdsPauseCampaign` | Pause campaigns. Ad delivery stops immediately. |
| `googleAdsEnableCampaign` | Re-enable paused campaigns. Ad delivery resumes immediately. |

`Actions_List` returns the current catalog and `Actions_Execute` runs an action after you confirm it.

## Example query

This is the kind of `Analytics_Query` call an assistant makes for you. Field names are the same as in Jepto Reports and Data Chat.

```json
{
  "clientId": "<clientId from Client_List>",
  "measures": [
    "googleAds.spend",
    "googleAds.conversions",
    "googleAds.costPerConversion"
  ],
  "dimensions": [
    "googleAds.campaignName"
  ],
  "dateRange": [
    "last30days"
  ],
  "order": {
    "googleAds.spend": "desc"
  },
  "limit": 10
}
```

## Fields

81 fields (12 metrics, 69 dimensions), from version 1.4.3 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `googleAds.clicks` | Clicks | SUM |  | Number of clicks on your ads during the reporting period. |
| `googleAds.impressions` | Impressions | SUM |  | Number of times your ads were shown, including repeat views by the same person. |
| `googleAds.spend` | Cost | SUM | Currency | Total amount spent on your ads during the reporting period, in your account currency. |
| `googleAds.ctr` | CTR | Calculated: Clicks ÷ Impressions | Percentage | Click-through rate: clicks divided by impressions, shown as a percentage. |
| `googleAds.cpm` | CPM | Calculated: (Cost ÷ Impressions) × 1,000 | Currency | Average cost per 1,000 impressions: spend divided by impressions (×1,000), in account currency. |
| `googleAds.cpc` | CPC | Calculated: Cost ÷ Clicks | Currency | Average cost per click: spend divided by clicks, in account currency. |
| `googleAds.costPerConversion` | Cost per conversion (CPA) | Calculated: Cost ÷ Conversions | Currency | Average spend for each conversion: spend divided by the number of conversion events. |
| `googleAds.conversionValue` | Conversion value | SUM | Currency | Total value attributed to conversion events (e.g. revenue from purchases) during the reporting period. |
| `googleAds.roas` | ROAS | Calculated: Conversion value ÷ Cost | Decimal (×) | Return on ad spend for attributed revenue or value divided by spend. |
| `googleAds.viewThroughConversions` | View-through conversions | SUM |  | Conversions recorded when a user saw (but did not click) your ad and later converted. |
| `googleAds.conversions` | Conversions | SUM |  | Number of completed conversion actions you are measuring (e.g. purchases, sign-ups, leads). |
| `googleAds.conversionRate` | Conversion rate | Calculated: Conversions ÷ Clicks | Percentage | Percentage of clicks that resulted in a conversion: conversions ÷ clicks. |

### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `googleAds.date` | Date | string | Date when the ad metrics were recorded. |
| `googleAds.adNetworkType` | Ad network | string | Ad network where your ads were shown (e.g. Google Search, Display Network, YouTube). |
| `googleAds.campaignId` | Campaign ID | number | Unique identifier for the campaign. |
| `googleAds.device` | Device | string | Device type on which the ad was shown (mobile, desktop, tablet). |
| `googleAds.campaignName` | Campaign | string | Name of the ad campaign. |
| `googleAds.campaignStatus` | Campaign status | string | Current delivery status of the campaign (enabled, paused, or removed). |
| `googleAds.campaignServingStatus` | Campaign serving status | string | Serving status of the campaign indicating why it may not be delivering (e.g. ended, pending, serving). |
| `googleAds.advertisingChannelType` | Campaign type | string | Campaign type that determines where ads can appear (e.g. Search, Display, Shopping, Video). |
| `googleAds.advertisingChannelSubType` | Campaign sub-type | string | Sub-type of the campaign (e.g. Search Standard, Shopping Smart). |
| `googleAds.campaignBudgetId` | Campaign budget ID | number | Unique identifier for the campaign budget in Google Ads. |
| `googleAds.campaignBudgetName` | Campaign budget name | string | Name of the budget shared across campaigns. |
| `googleAds.campaignBudgetAmountMicros` | Campaign daily budget | number | Daily budget for the campaign, in the account's currency. |
| `googleAds.campaignBudgetTotalAmountMicros` | Campaign total budget | number | Total lifetime budget for the campaign, in the account's currency. |
| `googleAds.campaignBudgetType` | Campaign budget type | string | The type of budget strategy used for the campaign (e.g., daily, shared). |
| `googleAds.campaignBudgetPeriod` | Campaign budget period | string | Time period the campaign budget covers (e.g. daily, monthly). |
| `googleAds.campaignBudgetExplicitlyShared` | Shared budget | boolean | Whether the budget is explicitly shared across multiple campaigns. |
| `googleAds.campaignCriterionId` | Campaign criterion ID | number | Unique identifier for the campaign-level targeting criterion in Google Ads. |
| `googleAds.campaignCriterionType` | Campaign criterion type | string | Type of targeting criterion applied at the campaign level (e.g. location, keyword). |
| `googleAds.campaignCriterionNegative` | Negative criterion | boolean | Whether the criterion is excluded (negative targeting) at the campaign level. |
| `googleAds.biddingStrategyId` | Bidding strategy ID | number | Unique identifier for the campaign's bidding strategy in Google Ads. |
| `googleAds.biddingStrategyName` | Bidding strategy name | string | Name of the bidding strategy applied to this campaign. |
| `googleAds.biddingStrategyType` | Bidding strategy type | string | Type of bid strategy used (e.g. Target CPA, Maximize Conversions, Manual CPC). |
| `googleAds.adGroupId` | Ad group ID | number | Unique identifier for the ad group. |
| `googleAds.adGroupName` | Ad group | string | Name of the ad group. |
| `googleAds.adGroupStatus` | Ad group status | string | Current delivery status of the ad group (enabled, paused, or removed). |
| `googleAds.adGroupType` | Ad group type | string | Type of ad group (e.g. Standard, Dynamic Search Ads, Shopping). |
| `googleAds.keywordId` | Keyword ID | number | Unique identifier for the keyword in Google Ads. |
| `googleAds.keywordText` | Keyword | string | The keyword phrase that triggered your ad. |
| `googleAds.keywordMatchType` | Match type | string | How closely the keyword must match the user's search (Broad, Phrase, or Exact). |
| `googleAds.keywordStatus` | Keyword status | string | Current delivery status of the keyword (enabled, paused, or removed). |
| `googleAds.keywordNegative` | Negative keyword | boolean | Whether the keyword is excluded as a negative keyword. |
| `googleAds.searchTerm` | Search term | string | The actual search query entered by a user that triggered your ad. |
| `googleAds.searchTermStatus` | Search term status | string | Whether the search term is included or excluded in your targeting. |
| `googleAds.adStatus` | Ad status | string | Current delivery status of the ad (enabled, paused, or removed). |
| `googleAds.adId` | Ad ID | number | Unique identifier for the ad. |
| `googleAds.adName` | Ad | string | Name of the ad. |
| `googleAds.adType` | Ad type | string | Format of the ad (e.g. Responsive Search Ad, Responsive Display Ad). |
| `googleAds.adGroupAssetFieldType` | Asset field type | string | Type of asset associated with the ad group (e.g. headline, image, description). |
| `googleAds.adGroupAssetStatus` | Asset status | string | Delivery status of the ad group asset (enabled, paused, or removed). |
| `googleAds.assetId` | Asset ID | number | Unique identifier for the asset in Google Ads. |
| `googleAds.assetName` | Asset name | string | Name of the asset (e.g. image, headline, video). |
| `googleAds.assetType` | Asset type | string | Type of the asset (e.g. image, text, video, call to action). |
| `googleAds.assetGroupId` | Asset group ID | number | Unique identifier for the asset group in Google Ads. |
| `googleAds.assetGroupName` | Asset group name | string | Name of the asset group within a Performance Max campaign. |
| `googleAds.assetGroupStatus` | Asset group status | string | Delivery status of the asset group (enabled, paused, or removed). |
| `googleAds.assetGroupListingGroupFilterType` | Listing group filter type | string | Type of listing group filter applied to this asset group (e.g. Subdivision, Unit). |
| `googleAds.merchantId` | Merchant ID | number | Unique identifier for the Google Merchant Center account. |
| `googleAds.storeCode` | Store code | string | Store identifier for local inventory ad reporting. |
| `googleAds.offerId` | Offer ID | string | Product offer ID from Google Merchant Center. |
| `googleAds.productTitle` | Product title | string | Title of the product as listed in Google Merchant Center. |
| `googleAds.productBrand` | Product brand | string | Brand of the product as listed in Google Merchant Center. |
| `googleAds.productCategoryLevel1` | Product category (L1) | string | Top-level product category (e.g. Apparel, Electronics). |
| `googleAds.productCategoryLevel2` | Product category (L2) | string | Second-level product category breakdown (e.g. Shirts within Apparel). |
| `googleAds.productTypeL1` | Product type (L1) | string | Top-level custom product type label used for product segmentation. |
| `googleAds.productTypeL2` | Product type (L2) | string | Second-level custom product type label. |
| `googleAds.unexpandedFinalUrl` | Landing page URL | string | The destination URL for the landing page before any URL expansion by Google. |
| `googleAds.detailPlacement` | Placement | string | Specific website or app where your Display ad was shown. |
| `googleAds.displayKeywordText` | Display keyword | string | Keyword used to target Display Network placements. |
| `googleAds.ageRangeType` | Age range | string | Age range of users targeted or who saw your ads (e.g. 18–24, 25–34). |
| `googleAds.genderType` | Gender | string | Gender of users targeted or who saw your ads. |
| `googleAds.countryCriterionId` | Country criterion ID | number | Numeric code identifying the targeted country in Google Ads. |
| `googleAds.userLocationCountryCriterionId` | User location country ID | number | Numeric code identifying the country where the user was physically located when the ad was shown. |
| `googleAds.performanceMaxPlacementDisplayName` | Performance Max placement name | string | Display name of the placement where your Performance Max ad appeared. |
| `googleAds.performanceMaxPlacement` | Performance Max placement | string | URL or app where your Performance Max ad was shown. |
| `googleAds.performanceMaxPlacementType` | Performance Max placement type | string | Type of placement for Performance Max (e.g. website, YouTube, app). |
| `googleAds.customerId` | Account ID | number | Unique identifier for the Google Ads account (customer ID). |
| `googleAds.customerDescriptiveName` | Account name | string | Name of the Google Ads account. |
| `googleAds.customerCurrencyCode` | Account currency | string | Currency used by the Google Ads account. |
| `googleAds.customerTimeZone` | Account time zone | string | Time zone configured for the Google Ads account. |

## Cross-platform fields

`unified.*` fields add Google Ads to totals across all your ad platforms, so one question can cover Google Ads, Meta, Microsoft, LinkedIn and TikTok together.

| Unified field | Label | From |
|---|---|---|
| `unified.spend` | Spend | `googleAds.spend` |
| `unified.impressions` | Impressions | `googleAds.impressions` |
| `unified.clicks` | Clicks | `googleAds.clicks` |
| `unified.ctr` | CTR | `googleAds.ctr` |
| `unified.cpm` | CPM | `googleAds.cpm` |
| `unified.cpc` | CPC | `googleAds.cpc` |
| `unified.conversions` | Conversions | `googleAds.conversions` |
| `unified.costPerConversion` | Cost per conversion | `googleAds.costPerConversion` |
| `unified.viewThroughConversions` | View-through conversions | `googleAds.viewThroughConversions` |
| `unified.conversionValue` | Conversion value | `googleAds.conversionValue` |
| `unified.roas` | ROAS | `googleAds.roas` |
| `unified.date` | Date | `googleAds.date` |
| `unified.campaignName` | Campaign | `googleAds.campaignName` |
| `unified.campaignId` | Campaign ID | `googleAds.campaignId` |
| `unified.adGroupName` | Ad group | `googleAds.adGroupName` |
| `unified.adGroupId` | Ad group ID | `googleAds.adGroupId` |
| `unified.adName` | Ad | `googleAds.adName` |
| `unified.adId` | Ad ID | `googleAds.adId` |
| `unified.placement` | Placement | `googleAds.adNetworkType` |
| `unified.device` | Device | `googleAds.device` |

## Links

- [Google Ads on jepto.com](https://www.jepto.com/data-sources/google-ads)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
