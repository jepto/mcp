# LinkedIn Ads MCP server

Connect LinkedIn Ads to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about LinkedIn campaigns and creatives: spend, leads, conversions, engagement and video, broken down by the job titles, seniorities, industries and company sizes you reached.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your LinkedIn Ads data comes from the LinkedIn Ads account you've connected in Jepto, with the same 98 metrics and 26 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "What was my cost per qualified lead by campaign last quarter?"
- "Which job functions and seniorities clicked most last month?"
- "How many Lead Gen Form opens turned into submissions this month?"

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

LinkedIn Ads is read-only through the MCP: the assistant can query your data but can't change anything in LinkedIn Ads.

## Fields

124 fields (98 metrics, 26 dimensions), from version 1.0.3 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `linkedInAds.impressions` | Impressions | SUM |  | Number of times your ad was shown (or messages sent for Sponsored Messaging). |
| `linkedInAds.clicks` | Clicks | SUM |  | Chargeable clicks on your LinkedIn ads during the reporting period. |
| `linkedInAds.costInLocalCurrency` | Spend | SUM | Currency | Total amount spent in your account currency during the reporting period. |
| `linkedInAds.spend` | Spend | SUM | Currency | Total amount spent in your account currency during the reporting period. |
| `linkedInAds.costInUsd` | Cost (USD) | SUM | Currency | Total amount spent in USD during the reporting period. |
| `linkedInAds.costPerQualifiedLead` | Cost per qualified lead | AVG | Currency | Average spend per qualified lead: spend ÷ qualified leads. |
| `linkedInAds.conversions` | Website conversions | SUM |  | Number of website conversion events attributed to your ads. |
| `linkedInAds.externalWebsiteConversions` | Website conversions | SUM |  | Number of website conversion events attributed to your ads. |
| `linkedInAds.externalWebsitePostClickConversions` | Post-click conversions | SUM |  | Website conversions attributed to clicks on your ads. |
| `linkedInAds.externalWebsitePostViewConversions` | Post-view conversions | SUM |  | Website conversions attributed to viewing your ads without clicking. |
| `linkedInAds.conversionValueInLocalCurrency` | Conversion value | SUM | Currency | Total revenue attributed to conversion events in your account currency. |
| `linkedInAds.landingPageClicks` | Landing page clicks | SUM |  | Number of clicks that directed users to your landing page. |
| `linkedInAds.companyPageClicks` | Company page clicks | SUM |  | Number of clicks to your LinkedIn company page from your ads. |
| `linkedInAds.likes` | Likes | SUM |  | Number of likes on your sponsored content. |
| `linkedInAds.shares` | Shares | SUM |  | Number of times your sponsored content was shared. |
| `linkedInAds.comments` | Comments | SUM |  | Number of comments on your sponsored content. |
| `linkedInAds.commentLikes` | Comment likes | SUM |  | Number of likes on comments on your sponsored content. |
| `linkedInAds.reactions` | Reactions | SUM |  | Number of reactions (likes, loves, etc.) on your sponsored content. |
| `linkedInAds.follows` | Follows | SUM |  | Number of new LinkedIn page followers gained from your ads. |
| `linkedInAds.totalEngagements` | Total engagements | SUM |  | Total engagement actions on your ads (clicks, reactions, shares, comments, etc.). |
| `linkedInAds.otherEngagements` | Other engagements | SUM |  | Other engagement actions on your sponsored content not counted separately. |
| `linkedInAds.oneClickLeads` | Lead Gen Form submissions | SUM |  | Number of leads captured via LinkedIn's one-click Lead Gen Forms. |
| `linkedInAds.oneClickLeadFormOpens` | Lead Gen Form opens | SUM |  | Number of times your Lead Gen Form was opened. |
| `linkedInAds.qualifiedLeads` | Qualified leads | SUM |  | Number of leads that matched your qualification criteria. |
| `linkedInAds.validWorkEmailLeads` | Valid work email leads | SUM |  | Number of leads with a verified work email address. |
| `linkedInAds.talentLeads` | Talent leads | SUM |  | Number of talent leads captured from LinkedIn Talent Insights ads. |
| `linkedInAds.leadGenerationMailContactInfoShares` | InMail contact info shares | SUM | Decimal (×) | Number of times contact info was shared via a Sponsored InMail Lead Gen form. |
| `linkedInAds.leadGenerationMailInterestedClicks` | InMail interested clicks | SUM | Decimal (×) | Number of interested clicks on Sponsored InMail Lead Gen messages. |
| `linkedInAds.opens` | Opens | SUM |  | Number of times your Sponsored InMail or message ads were opened. |
| `linkedInAds.sends` | Sends | SUM |  | Number of Sponsored InMail or message ads sent. |
| `linkedInAds.actionClicks` | Action clicks | SUM |  | Number of clicks on the call-to-action button in your Sponsored InMail. |
| `linkedInAds.adUnitClicks` | Ad unit clicks | SUM |  | Number of clicks on the display ad unit within a Sponsored InMail. |
| `linkedInAds.textUrlClicks` | Text URL clicks | SUM |  | Number of clicks on text links in your Sponsored InMail. |
| `linkedInAds.jobApplications` | Job applications | SUM |  | Number of job applications attributed to your ads. |
| `linkedInAds.postClickJobApplications` | Job applications (post-click) | SUM |  | Job applications attributed to clicks on your ads. |
| `linkedInAds.postViewJobApplications` | Job applications (post-view) | SUM |  | Job applications attributed to viewing your ads without clicking. |
| `linkedInAds.jobApplyClicks` | Job apply clicks | SUM |  | Number of clicks on the Apply button from your ads. |
| `linkedInAds.postClickJobApplyClicks` | Job apply clicks (post-click) | SUM |  | Apply button clicks attributed to clicks on your ads. |
| `linkedInAds.postViewJobApplyClicks` | Job apply clicks (post-view) | SUM |  | Apply button clicks attributed to viewing your ads without clicking. |
| `linkedInAds.registrations` | Event registrations | SUM | Decimal (×) | Number of event registrations attributed to your ads. |
| `linkedInAds.postClickRegistrations` | Event registrations (post-click) | AVG | Decimal (×) | Event registrations attributed to clicks on your ads. |
| `linkedInAds.postViewRegistrations` | Event registrations (post-view) | AVG | Decimal (×) | Event registrations attributed to viewing your ads without clicking. |
| `linkedInAds.videoViews` | Video views | SUM |  | Number of times your video ad was viewed. |
| `linkedInAds.videoStarts` | Video starts | SUM |  | Number of times your video ad started playing. |
| `linkedInAds.videoCompletions` | Video completions | SUM |  | Number of times your video was watched to at least 97% completion. |
| `linkedInAds.videoFirstQuartileCompletions` | Video views 25% | SUM |  | Number of times your video was watched to at least 25% completion. |
| `linkedInAds.videoMidpointCompletions` | Video views 50% | SUM |  | Number of times your video was watched to at least 50% completion. |
| `linkedInAds.videoThirdQuartileCompletions` | Video views 75% | SUM |  | Number of times your video was watched to at least 75% completion. |
| `linkedInAds.videoWatchTime` | Video watch time | SUM |  | Total time users spent watching your video ads. |
| `linkedInAds.fullScreenPlays` | Full screen plays | SUM |  | Number of times users expanded your video ad to full screen. |
| `linkedInAds.documentCompletions` | Document completions | SUM |  | Number of times your Document Ad was scrolled to the final page. |
| `linkedInAds.documentFirstQuartileCompletions` | Document viewed 25% | SUM |  | Number of times your Document Ad was scrolled to at least 25% of pages. |
| `linkedInAds.documentMidpointCompletions` | Document viewed 50% | SUM |  | Number of times your Document Ad was scrolled to at least 50% of pages. |
| `linkedInAds.documentThirdQuartileCompletions` | Document viewed 75% | SUM |  | Number of times your Document Ad was scrolled to at least 75% of pages. |
| `linkedInAds.downloadClicks` | Download clicks | SUM |  | Number of clicks on the download icon in your Document Ad. |
| `linkedInAds.subscriptionClicks` | Subscription clicks | SUM |  | Number of clicks on the subscription button in your ads. |
| `linkedInAds.headlineClicks` | Headline clicks | SUM |  | Number of clicks on the headline of your Sponsored Content ads. |
| `linkedInAds.headlineImpressions` | Headline impressions | SUM |  | Number of times the headline of your Sponsored Content was displayed. |
| `linkedInAds.approximateMemberReach` | Approximate member reach | MAX |  | Estimated number of unique LinkedIn members who saw your ads. |
| `linkedInAds.averageDwellTime` | Average dwell time | AVG |  | Average time users spent with your ad in view. |
| `linkedInAds.audiencePenetration` | Audience penetration | AVG | Decimal (×) | Proportion of your target audience that was reached by your ads: unique members reached ÷ total audience size. |
| `linkedInAds.cardClicks` | Card clicks | SUM |  | Number of clicks on the cards within your Carousel Ad. |
| `linkedInAds.cardImpressions` | Card impressions | SUM |  | Number of times the cards within your Carousel Ad were displayed. |
| `linkedInAds.viralImpressions` | Viral impressions | SUM |  | Number of times your ad was shown through organic sharing (likes, shares, follows) by members who engaged with it. |
| `linkedInAds.viralClicks` | Viral clicks | SUM |  | Number of clicks on your ad that originated from organic viral distribution. |
| `linkedInAds.viralLikes` | Viral likes | SUM |  | Number of likes on your ad that originated from organic viral distribution. |
| `linkedInAds.viralShares` | Viral shares | SUM |  | Number of times your ad was shared through organic viral distribution. |
| `linkedInAds.viralComments` | Viral comments | SUM |  | Number of comments on your ad that originated from organic viral distribution. |
| `linkedInAds.viralLandingPageClicks` | Viral landing page clicks | SUM |  | Number of landing page clicks from viral distribution of your ad. |
| `linkedInAds.viralTotalEngagements` | Viral total engagements | SUM |  | Total engagement actions from viral distribution of your ad (clicks, likes, shares, comments, etc.). |
| `linkedInAds.viralExternalWebsiteConversions` | Viral website conversions | SUM |  | Website conversions attributed to viral (organic) distribution of your ad. |
| `linkedInAds.viralExternalWebsitePostClickConversions` | Viral post-click conversions | SUM |  | Website conversions from clicks on a viral (organically shared) version of your ad. |
| `linkedInAds.viralExternalWebsitePostViewConversions` | Viral post-view conversions | SUM |  | Website conversions from views of a viral (organically shared) version of your ad without clicking. |
| `linkedInAds.viralVideoViews` | Viral video views | SUM |  | Number of times your video was viewed through organic viral distribution. |
| `linkedInAds.viralVideoStarts` | Viral video starts | SUM |  | Number of times your video started playing through organic viral distribution. |
| `linkedInAds.viralVideoCompletions` | Viral video completions | SUM |  | Number of times your video was watched to at least 97% completion through organic viral distribution. |
| `linkedInAds.viralOneClickLeads` | Viral Lead Gen Form submissions | SUM |  | Leads captured via Lead Gen Forms through viral (organic) distribution of your ad. |
| `linkedInAds.viralCardClicks` | Viral card clicks | SUM |  | Number of clicks on carousel cards from viral (organic) distribution of your ad. |
| `linkedInAds.viralCardImpressions` | Viral card impressions | SUM |  | Number of times carousel cards were displayed through viral (organic) distribution of your ad. |
| `linkedInAds.eventViews` | Event views | SUM |  | Number of times your LinkedIn Event Ad was viewed. |
| `linkedInAds.eventViewsOver15Seconds` | Event views 15s+ | SUM |  | Number of times your Event Ad was viewed for more than 15 seconds. |
| `linkedInAds.eventViewsOver30Seconds` | Event views 30s+ | SUM |  | Number of times your Event Ad was viewed for more than 30 seconds. |
| `linkedInAds.eventViewsOver2Minutes` | Event views 2min+ | SUM |  | Number of times your Event Ad was viewed for more than 2 minutes. |
| `linkedInAds.revenueAttributionMetrics` | Revenue attribution | SUM | Currency | Total revenue attributed to your ads via LinkedIn's revenue attribution reporting. |
| `linkedInAds.ctr` | CTR | Calculated: Clicks ÷ Impressions |  | Click-through rate: clicks divided by impressions, shown as a percentage. |
| `linkedInAds.cpm` | CPM | Calculated: (Spend ÷ Impressions) × 1,000 |  | Average cost per 1,000 impressions in your account currency. |
| `linkedInAds.cpc` | CPC | Calculated: Spend ÷ Clicks |  | Average cost per click: spend divided by clicks. |
| `linkedInAds.costPerConversion` | Cost per conversion | Calculated: Spend ÷ Website conversions |  | Average spend for each conversion: spend divided by the number of conversion events. |
| `linkedInAds.conversionRate` | Conversion rate | Calculated: Website conversions ÷ Clicks |  | Percentage of clicks that resulted in a website conversion: conversions ÷ clicks. |
| `linkedInAds.conversionRateByImpressions` | Conversion rate (by impressions) | Calculated: Website conversions ÷ Impressions |  | Percentage of impressions that resulted in a website conversion: conversions ÷ impressions. |
| `linkedInAds.engagementRate` | Engagement rate | Calculated: Total engagements ÷ Impressions |  | Percentage of impressions that resulted in an engagement action: total engagements ÷ impressions. |
| `linkedInAds.costPerLead` | Cost per lead | Calculated: Spend ÷ Lead Gen Form submissions |  | Average spend for each lead: spend divided by the number of lead events. |
| `linkedInAds.leadRate` | Lead rate | Calculated: Lead Gen Form submissions ÷ Impressions |  | Percentage of impressions that resulted in a Lead Gen Form submission: leads ÷ impressions. |
| `linkedInAds.roas` | ROAS | Calculated: Conversion value ÷ Spend |  | Return on ad spend for this event: attributed revenue or value divided by spend. |
| `linkedInAds.landingPageCtr` | Landing page CTR | Calculated: Landing page clicks ÷ Impressions |  | Percentage of impressions that resulted in a click to your landing page: landing page clicks ÷ impressions. |
| `linkedInAds.videoViewRate` | Video view rate | Calculated: Video views ÷ Impressions |  | Percentage of impressions that resulted in a video view: video views ÷ impressions. |
| `linkedInAds.videoCompletionRate` | Video completion rate | Calculated: Video completions ÷ Video starts |  | Percentage of video starts that were watched to at least 97% completion: video completions ÷ video starts. |
| `linkedInAds.qualifiedLeadRate` | Qualified lead rate | Calculated: Qualified leads ÷ Lead Gen Form submissions |  | Percentage of Lead Gen Form submissions that were qualified leads: qualified leads ÷ total leads. |

### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `linkedInAds.pivotValues` | Breakdown values | string[] | Values identifying the breakdown category for this row (e.g. campaign name, country, device). |
| `linkedInAds.date` | Date | string | Date when the ad metrics were recorded. |
| `linkedInAds.account` | Account | string | LinkedIn ad account associated with these results. |
| `linkedInAds.campaignId` | Campaign | string | Name of the campaign in LinkedIn Campaign Manager. |
| `linkedInAds.campaignName` | Campaign | string | Name of the campaign in LinkedIn Campaign Manager. |
| `linkedInAds.creativeId` | Creative | string | Performance broken down by sponsored creative (the ad unit users see). |
| `linkedInAds.campaignGroupId` | Campaign group | string | Campaign group (the folder level above campaigns) for these results. |
| `linkedInAds.shareId` | Sponsored share | string | Unique identifier for a specific sponsored share post. |
| `linkedInAds.companyId` | Company | string | LinkedIn company page associated with these ad results. |
| `linkedInAds.conversion` | Conversion | string | Specific conversion rule or event used for attribution. |
| `linkedInAds.country` | Country | string | Member country where impressions or clicks occurred. |
| `linkedInAds.region` | Region | string | Member region within the selected country. |
| `linkedInAds.county` | County | string | Member county within the selected region. |
| `linkedInAds.memberCompany` | Member company | string | Company where LinkedIn members work, used for audience breakdowns. |
| `linkedInAds.jobFunction` | Job function | string | Job function of members who saw or engaged with your ads. |
| `linkedInAds.jobTitle` | Job title | string | Job title of members who saw or engaged with your ads. |
| `linkedInAds.seniority` | Seniority | string | Seniority level of members who saw or engaged with your ads. |
| `linkedInAds.industry` | Industry | string | Industry of members who saw or engaged with your ads. |
| `linkedInAds.companySize` | Company size | string | Size band of companies where members work. |
| `linkedInAds.device` | Device | string | Device type where the ad was shown (for example, desktop or mobile). |
| `linkedInAds.placement` | Placement | string | Where the ad appeared on LinkedIn (feed, sidebar, etc.). |
| `linkedInAds.servingLocation` | Serving location | string | Whether the impression was served on LinkedIn (onsite) or on the audience network (offsite). |
| `linkedInAds.cardIndex` | Carousel card | string | Index of the card in a carousel creative when multiple cards are shown. |
| `linkedInAds.eventStage` | Event stage | string | Stage of a LinkedIn event campaign (before, during, or after the live event). |
| `linkedInAds.conversationNode` | Conversation node | string | Step in a sponsored messaging conversation flow. |
| `linkedInAds.objectiveType` | Objective | string | Campaign objective type (for example brand awareness or lead generation). |

## Cross-platform fields

`unified.*` fields add LinkedIn Ads to totals across all your ad platforms, so one question can cover Google Ads, Meta, Microsoft, LinkedIn and TikTok together.

| Unified field | Label | From |
|---|---|---|
| `unified.spend` | Spend | `linkedInAds.spend` |
| `unified.impressions` | Impressions | `linkedInAds.impressions` |
| `unified.reach` | Reach | `linkedInAds.approximateMemberReach` |
| `unified.clicks` | Clicks | `linkedInAds.clicks` |
| `unified.ctr` | CTR | `linkedInAds.ctr` |
| `unified.cpm` | CPM | `linkedInAds.cpm` |
| `unified.cpc` | CPC | `linkedInAds.cpc` |
| `unified.conversions` | Conversions | `linkedInAds.conversions` |
| `unified.costPerConversion` | Cost per conversion | `linkedInAds.costPerConversion` |
| `unified.viewThroughConversions` | View-through conversions | `linkedInAds.externalWebsitePostViewConversions` |
| `unified.conversionValue` | Conversion value | `linkedInAds.conversionValueInLocalCurrency` |
| `unified.roas` | ROAS | `linkedInAds.roas` |
| `unified.date` | Date | `linkedInAds.date` |
| `unified.campaignName` | Campaign | `linkedInAds.campaignName` |
| `unified.campaignId` | Campaign ID | `linkedInAds.campaignId` |
| `unified.adGroupName` | Ad group | `linkedInAds.campaignName` |
| `unified.adGroupId` | Ad group ID | `linkedInAds.campaignId` |
| `unified.adId` | Ad ID | `linkedInAds.creativeId` |
| `unified.placement` | Placement | `linkedInAds.placement` |
| `unified.country` | Country | `linkedInAds.country` |
| `unified.device` | Device | `linkedInAds.device` |

## Links

- [LinkedIn Ads on jepto.com](https://www.jepto.com/data-sources/linkedin-ads)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
