# TikTok Ads MCP server

Connect TikTok Ads to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about TikTok campaigns, ad groups and ads: spend, results, conversions, video views and engagement, by age, gender, country and placement.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your TikTok Ads data comes from the TikTok Ads account you've connected in Jepto, with the same 203 metrics and 55 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "Which ads had the best cost per result last week?"
- "Compare 2-second and 6-second video views by ad this month."
- "Break down spend and conversions by age and gender for the last 30 days."

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

TikTok Ads is read-only through the MCP: the assistant can query your data but can't change anything in TikTok Ads.

## Fields

258 fields (203 metrics, 55 dimensions), from version 1.0.2 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `tiktokAds.spend` | Spend | SUM | Currency | Total amount spent on ads during the reporting period in TikTok Ads Manager. |
| `tiktokAds.impressions` | Impressions | SUM |  | Number of times your ads were shown. |
| `tiktokAds.clicks` | Clicks | SUM |  | Number of clicks to your destination URL (landing page) from your ads. |
| `tiktokAds.reach` | Reach | MAX |  | Number of unique users who saw your ads at least once. |
| `tiktokAds.result` | Results | SUM |  | Times your ad delivered your selected optimization goal. Not available at campaign level—use ad group or ad breakdowns. |
| `tiktokAds.conversion` | Conversions | SUM |  | Secondary conversion events attributed to your ads. |
| `tiktokAds.costPerConversion` | Cost per conversion | AVG | Currency | Average spend for each conversion: spend divided by conversions. |
| `tiktokAds.conversionRate` | Conversion rate | AVG | Percentage | Percentage of impressions that resulted in a conversion: conversions ÷ impressions. |
| `tiktokAds.conversionRateV2` | Conversion rate (by clicks) | AVG | Percentage | Percentage of clicks that resulted in a conversion: conversions ÷ clicks. |
| `tiktokAds.realTimeConversion` | Real-time conversions | SUM |  | Conversions tracked in real time during the reporting period. |
| `tiktokAds.realTimeCostPerConversion` | Real-time cost per conversion | AVG | Currency | Average spend per conversion using real-time conversion data. |
| `tiktokAds.realTimeConversionRate` | Real-time CVR | AVG | Percentage | Real-time conversion rate: conversions ÷ clicks. |
| `tiktokAds.realTimeConversionRateV2` | Real-time CVR (by impressions) | AVG | Percentage | Real-time conversion rate (v2): conversions ÷ impressions. |
| `tiktokAds.costPerResult` | Cost per result | AVG | Currency | Average spend for each result: spend divided by results (your selected optimization goal). |
| `tiktokAds.resultRate` | Result rate | AVG | Percentage | Percentage of impressions that achieved your selected optimization goal: results ÷ impressions. |
| `tiktokAds.realTimeResult` | Real-time results | SUM |  | Results tracked in real time during the reporting period. |
| `tiktokAds.realTimeCostPerResult` | Real-time cost per result | AVG | Currency | Average spend per result using real-time result data. |
| `tiktokAds.realTimeResultRate` | Real-time result rate | AVG | Percentage | Real-time result rate: results ÷ impressions. |
| `tiktokAds.secondaryGoalResult` | Secondary goal results | SUM |  | Secondary goal results attributed to your ads. |
| `tiktokAds.costPerSecondaryGoalResult` | Cost per secondary goal result | AVG | Currency | Average spend for each secondary goal result. |
| `tiktokAds.secondaryGoalResultRate` | Secondary goal result rate | AVG | Percentage | Percentage of impressions that achieved the secondary goal: secondary goal results ÷ impressions. |
| `tiktokAds.totalLandingPageView` | Total landing page views | SUM |  | Total number of landing page views attributed to your ads. |
| `tiktokAds.cpc` | CPC | AVG | Currency | Average cost per click (spend divided by clicks). Shown in account currency. |
| `tiktokAds.cpm` | CPM | AVG | Currency | Average cost per 1,000 impressions. Shown in account currency. |
| `tiktokAds.ctr` | CTR | AVG | Percentage | Click-through rate: clicks divided by impressions. Shown as a percentage in TikTok Ads Manager. |
| `tiktokAds.grossImpressions` | Gross impressions | SUM |  | Total gross impressions including duplicate counts. |
| `tiktokAds.costPer1000Reached` | Cost per 1,000 people reached | AVG | Currency | Average cost to reach 1,000 unique people (CPM-style reach efficiency). |
| `tiktokAds.frequency` | Frequency | AVG |  | Average number of times each person saw your ad. |
| `tiktokAds.videoPlayActions` | Video plays | SUM |  | Number of times your video ad started playing. |
| `tiktokAds.videoViewsP25` | Video views 25% | SUM |  | Number of times your video was watched to at least 25% completion. |
| `tiktokAds.videoViewsP50` | Video views 50% | SUM |  | Number of times your video was watched to at least 50% completion. |
| `tiktokAds.videoViewsP75` | Video views 75% | SUM |  | Number of times your video was watched to at least 75% completion. |
| `tiktokAds.videoViewsP100` | Video views 100% | SUM |  | Number of times your video was watched to full completion. |
| `tiktokAds.videoWatched2s` | Video watched 2s | SUM |  | Number of times your video was watched for at least 2 seconds. |
| `tiktokAds.videoWatched6s` | Video watched 6s | SUM |  | Number of times your video was watched for at least 6 seconds. |
| `tiktokAds.averageVideoPlay` | Avg video play duration | AVG |  | Average duration users watched your video ad. |
| `tiktokAds.averageVideoPlayPerUser` | Avg video plays per user | AVG |  | Average number of times each user played your video ad. |
| `tiktokAds.engagedView` | Engaged views (6s) | SUM |  | Number of 6-second engaged views on your video ad. |
| `tiktokAds.paidEngagedView` | Paid engaged views (6s) | SUM |  | Number of paid 6-second engaged views on your video ad. |
| `tiktokAds.paidEngagementEngagedView` | Paid engagement views | SUM |  | Number of paid engagement engaged views on your video ad. |
| `tiktokAds.engagedView15s` | Engaged views (15s) | SUM |  | Number of 15-second engaged views on your video ad. |
| `tiktokAds.paidEngagedView15s` | Paid engaged views (15s) | SUM |  | Number of paid 15-second engaged views on your video ad. |
| `tiktokAds.paidEngagementEngagedView15s` | Paid engagement views (15s) | SUM |  | Number of paid engagement 15-second engaged views on your video ad. |
| `tiktokAds.engagements` | Clicks (all) | SUM |  | Total clicks on your ads, including destination, social, and interaction clicks. Separate from Clicks (destination) in TikTok Ads Manager. |
| `tiktokAds.engagementRate` | Engagement rate | AVG | Percentage | Percentage of impressions that resulted in any engagement: engagements ÷ impressions. |
| `tiktokAds.likes` | Likes | SUM |  | Number of likes on your ads. |
| `tiktokAds.comments` | Comments | SUM |  | Number of comments on your ads. |
| `tiktokAds.shares` | Shares | SUM |  | Number of times your ads were shared. |
| `tiktokAds.follows` | Follows | SUM |  | Number of new followers gained from your ads. |
| `tiktokAds.profileVisits` | Profile visits | SUM |  | Number of visits to your TikTok profile from your ads. |
| `tiktokAds.profileVisitsRate` | Profile visit rate | AVG | Percentage | Percentage of impressions that led to a profile visit: profile visits ÷ impressions. |
| `tiktokAds.clicksOnMusicDisc` | Music disc clicks | SUM |  | Number of clicks on the music disc in your ad. |
| `tiktokAds.duetClicks` | Duet clicks | SUM |  | Number of clicks on the Duet button in your ad. |
| `tiktokAds.stitchClicks` | Stitch clicks | SUM |  | Number of clicks on the Stitch button in your ad. |
| `tiktokAds.soundUsageClicks` | Sound usage clicks | SUM |  | Number of clicks on the Use Sound option in your ad. |
| `tiktokAds.anchorClicks` | Anchor clicks | SUM |  | Number of clicks on an anchor link in your ad. |
| `tiktokAds.anchorClickRate` | Anchor click rate | AVG | Percentage | Percentage of impressions that resulted in an anchor click: anchor clicks ÷ impressions. |
| `tiktokAds.clicksOnHashtagChallenge` | Hashtag challenge clicks | SUM |  | Number of clicks on the hashtag challenge in your ad. |
| `tiktokAds.ixPageDurationAvg` | Instant Experience page duration avg. | AVG | Decimal (×) | Average time users spent on a page in Instant Experience ads (full-screen mobile experience). |
| `tiktokAds.ixPageViewrateAvg` | Instant Experience page view rate avg. | AVG | Percentage | Average percentage of an Instant Experience page viewed by users. |
| `tiktokAds.ixVideoViews` | Instant Experience video views | SUM |  | Metric for full-screen Instant Experience ads - total video views. |
| `tiktokAds.ixVideoViewsP25` | Instant Experience video views 25% | SUM |  | Video views at 25% completion in Instant Experience ads. |
| `tiktokAds.ixVideoViewsP50` | Instant Experience video views 50% | SUM |  | Video views at 50% completion in Instant Experience ads. |
| `tiktokAds.ixVideoViewsP75` | Instant Experience video views 75% | SUM |  | Video views at 75% completion in Instant Experience ads. |
| `tiktokAds.ixVideoViewsP100` | Instant Experience video views 100% | SUM |  | Video views at 100% completion (fully watched) in Instant Experience ads. |
| `tiktokAds.ixAverageVideoPlay` | Instant Experience avg. video play | AVG |  | Average number of video plays per user in Instant Experience ads. |
| `tiktokAds.ttPlaylistVisit` | Playlist visits | SUM |  | Number of visits to a TikTok playlist from your ad. |
| `tiktokAds.ttPlaylistVisitRate` | Playlist visit rate | AVG | Percentage | Percentage of impressions that led to a playlist visit: playlist visits ÷ impressions. |
| `tiktokAds.completePaymentRoas` | Complete payment ROAS | AVG | Decimal (×) | Return on ad spend for complete payments: total complete payment value ÷ spend. |
| `tiktokAds.completePayment` | Complete payments | SUM |  | Number of complete payment events attributed to your ads. |
| `tiktokAds.costPerCompletePayment` | Cost per complete payment | AVG | Currency | Average spend per complete payment: spend ÷ complete payments. |
| `tiktokAds.completePaymentRate` | Complete payment rate | AVG | Percentage | Percentage of clicks that resulted in a complete payment: complete payments ÷ clicks. |
| `tiktokAds.valuePerCompletePayment` | Value per complete payment | AVG | Currency | Average value per complete payment event. |
| `tiktokAds.totalCompletePaymentRate` | Total complete payment rate | AVG | Percentage | Percentage of total impressions that resulted in a complete payment: complete payments ÷ impressions. |
| `tiktokAds.costPerLandingPageView` | Cost per landing page view | AVG | Currency | Average spend per landing page view: spend ÷ landing page views. |
| `tiktokAds.landingPageViewRate` | Landing page view rate | AVG | Percentage | Percentage of clicks that resulted in a landing page view: landing page views ÷ clicks. |
| `tiktokAds.totalPageview` | Total page views | SUM |  | Total number of page views on your landing page attributed to your ads. |
| `tiktokAds.costPerPageview` | Cost per page view | AVG | Currency | Average spend per page view: spend ÷ page views. |
| `tiktokAds.pageviewRate` | Page view rate | AVG | Percentage | Percentage of clicks that resulted in a page view: page views ÷ clicks. |
| `tiktokAds.avgValuePerPageview` | Avg value per page view | AVG | Currency | Average revenue per page view on your landing page. |
| `tiktokAds.totalValuePerPageview` | Total value per page view | AVG | Currency | Total revenue attributed to page views on your landing page. |
| `tiktokAds.buttonClick` | Button clicks | SUM |  | Number of button clicks on your landing page attributed to your ads. |
| `tiktokAds.costPerButtonClick` | Cost per button click | AVG | Currency | Average spend per button click: spend ÷ button clicks. |
| `tiktokAds.buttonClickRate` | Button click rate | AVG | Percentage | Percentage of clicks that resulted in a button click on your landing page: button clicks ÷ clicks. |
| `tiktokAds.valuePerButtonClick` | Value per button click | AVG | Currency | Average value per button click on your landing page. |
| `tiktokAds.totalButtonClickValue` | Total button click value | SUM | Currency | Total value attributed to button click events on your landing page. |
| `tiktokAds.webEventAddToCart` | (Page) Add to cart | SUM |  | Web event measured on your landing page after an ad click. |
| `tiktokAds.costPerWebEventAddToCart` | Cost per add to cart (Page) | AVG | Currency | Average spend per add-to-cart event on your landing page: spend ÷ add to carts. |
| `tiktokAds.webEventAddToCartRate` | (Page) Add to cart rate | AVG | Percentage | Percentage of clicks that resulted in an add-to-cart event on your landing page: add to carts ÷ clicks. |
| `tiktokAds.valuePerWebEventAddToCart` | Value per add to cart (Page) | AVG | Currency | Average value per add-to-cart event on your landing page. |
| `tiktokAds.totalWebEventAddToCartValue` | Total add to cart value (Page) | SUM | Currency | Total revenue attributed to add-to-cart events on your landing page. |
| `tiktokAds.initiateCheckout` | Initiate checkout | SUM |  | Number of checkout initiations on your landing page attributed to your ads. |
| `tiktokAds.costPerInitiateCheckout` | Cost per initiate checkout | AVG | Currency | Average spend per checkout initiation: spend ÷ initiate checkouts. |
| `tiktokAds.initiateCheckoutRate` | Initiate checkout rate | AVG | Percentage | Percentage of clicks that resulted in a checkout initiation: initiate checkouts ÷ clicks. |
| `tiktokAds.valuePerInitiateCheckout` | Value per initiate checkout | AVG | Currency | Average value per checkout initiation on your landing page. |
| `tiktokAds.totalInitiateCheckoutValue` | Total initiate checkout value | SUM | Currency | Total revenue attributed to checkout initiations on your landing page. |
| `tiktokAds.pageEventSearch` | (Page) Search | SUM |  | Number of search events on your landing page attributed to your ads. |
| `tiktokAds.costPerPageEventSearch` | Cost per search (Page) | AVG | Currency | Average spend per search event on your landing page: spend ÷ searches. |
| `tiktokAds.pageEventSearchRate` | (Page) Search rate | AVG | Percentage | Percentage of clicks that resulted in a search event on your landing page: searches ÷ clicks. |
| `tiktokAds.valuePerPageEventSearch` | Value per search (Page) | AVG | Currency | Average value per search event on your landing page. |
| `tiktokAds.totalPageEventSearchValue` | Total search value (Page) | SUM | Currency | Total revenue attributed to search events on your landing page. |
| `tiktokAds.onWebOrder` | (Page) Order | SUM |  | Web event measured on your landing page after an ad click. |
| `tiktokAds.costPerOnWebOrder` | Cost per order (Page) | AVG | Currency | Average spend per order on your landing page: spend ÷ orders. |
| `tiktokAds.onWebOrderRate` | (Page) Order rate | AVG | Percentage | Percentage of clicks that resulted in an order on your landing page: orders ÷ clicks. |
| `tiktokAds.valuePerOnWebOrder` | Value per order (Page) | AVG | Currency | Average value per order on your landing page. |
| `tiktokAds.totalOnWebOrderValue` | Total order value (Page) | SUM | Currency | Total revenue attributed to orders on your landing page. |
| `tiktokAds.addBilling` | Add billing | SUM |  | Number of payment info additions attributed to your ads. |
| `tiktokAds.costPerAddBilling` | Cost per add billing | AVG | Currency | Average spend per payment info addition: spend ÷ add billing events. |
| `tiktokAds.addBillingRate` | Add billing rate | AVG | Percentage | Percentage of clicks that resulted in a payment info addition: add billing events ÷ clicks. |
| `tiktokAds.valuePerAddBilling` | Value per add billing | AVG | Currency | Average value per payment info addition. |
| `tiktokAds.totalAddBillingValue` | Total add billing value | SUM | Currency | Total value attributed to payment info addition events. |
| `tiktokAds.onWebAddToWishlist` | (Page) Add to wishlist | SUM |  | Number of add-to-wishlist events on your landing page attributed to your ads. |
| `tiktokAds.costPerOnWebAddToWishlist` | Cost per add to wishlist (Page) | AVG | Currency | Average spend per add-to-wishlist event on your landing page: spend ÷ wishlist adds. |
| `tiktokAds.onWebAddToWishlistPerClick` | (Page) Add to wishlist per click | SUM |  | Wishlist add events per click on your landing page. |
| `tiktokAds.valuePerOnWebAddToWishlist` | Value per add to wishlist (Page) | AVG | Currency | Average value per add-to-wishlist event on your landing page. |
| `tiktokAds.totalOnWebAddToWishlistValue` | Total add to wishlist value (Page) | SUM | Currency | Total revenue attributed to add-to-wishlist events on your landing page. |
| `tiktokAds.onWebSubscribe` | (Page) Subscribe | SUM |  | Number of subscription events on your landing page attributed to your ads. |
| `tiktokAds.costPerOnWebSubscribe` | Cost per subscribe (Page) | AVG | Currency | Average spend per subscription event on your landing page: spend ÷ subscribes. |
| `tiktokAds.onWebSubscribePerClick` | (Page) Subscribe per click | SUM |  | Subscription events per click on your landing page. |
| `tiktokAds.valuePerOnWebSubscribe` | Value per subscribe (Page) | AVG | Currency | Average value per subscription event on your landing page. |
| `tiktokAds.totalOnWebSubscribeValue` | Total subscribe value (Page) | SUM | Currency | Total revenue attributed to subscription events on your landing page. |
| `tiktokAds.form` | Form submissions | SUM |  | Number of form submissions attributed to your ads. |
| `tiktokAds.costPerForm` | Cost per form | AVG | Currency | Average spend for each form submission: spend ÷ form submissions. |
| `tiktokAds.formRate` | Form submission rate | AVG | Percentage | Percentage of clicks that resulted in a form submission: form submissions ÷ clicks. |
| `tiktokAds.valuePerForm` | Value per form | AVG | Currency | Average value per form submission. |
| `tiktokAds.totalFormValue` | Total form value | SUM | Currency | Total value attributed to form submissions. |
| `tiktokAds.downloadStart` | Download starts | SUM |  | Number of download starts attributed to your ads. |
| `tiktokAds.costPerDownloadStart` | Cost per download start | AVG | Currency | Average spend per download start: spend ÷ download starts. |
| `tiktokAds.downloadStartRate` | Download start rate | AVG | Percentage | Percentage of clicks that resulted in a download start: download starts ÷ clicks. |
| `tiktokAds.valuePerDownloadStart` | Value per download start | AVG | Currency | Average value per download start. |
| `tiktokAds.totalDownloadStartValue` | Total download start value | SUM | Currency | Total value attributed to download start events. |
| `tiktokAds.customPageEvents` | (Page) Custom events | SUM |  | Number of custom events on your landing page attributed to your ads. |
| `tiktokAds.costPerCustomPageEvent` | Cost per custom event (Page) | AVG | Currency | Average spend per custom event on your landing page: spend ÷ custom events. |
| `tiktokAds.customPageEventRate` | (Page) Custom event rate | AVG | Percentage | Percentage of clicks that resulted in a custom event on your landing page: custom events ÷ clicks. |
| `tiktokAds.valuePerCustomPageEvent` | Value per custom event (Page) | AVG | Currency | Average value per custom event on your landing page. |
| `tiktokAds.customPageEventsValue` | Total custom event value (Page) | SUM | Currency | Total value attributed to custom events on your landing page. |
| `tiktokAds.pageBrowseView` | (Page) Browse views | SUM |  | Number of browse events on your landing page attributed to your ads. |
| `tiktokAds.costPerPageBrowseView` | Cost per browse view (Page) | AVG | Currency | Average spend per browse event on your landing page: spend ÷ browse views. |
| `tiktokAds.pageBrowseViewRate` | (Page) Browse view rate | AVG | Percentage | Percentage of clicks that resulted in a browse event on your landing page: browse views ÷ clicks. |
| `tiktokAds.totalPageBrowseViewValue` | Total browse view value (Page) | SUM | Currency | Total value attributed to browse events on your landing page. |
| `tiktokAds.valuePerPageBrowseView` | Value per browse view (Page) | AVG | Currency | Average value per browse event on your landing page. |
| `tiktokAds.onlineConsult` | (Page) Online consults | SUM |  | Number of online consult events on your landing page attributed to your ads. |
| `tiktokAds.costPerOnlineConsult` | Cost per online consult (Page) | AVG | Currency | Average spend per online consult event: spend ÷ online consults. |
| `tiktokAds.onlineConsultRate` | Online consult rate (Page) | AVG | Percentage | Percentage of clicks that resulted in an online consult event: online consults ÷ clicks. |
| `tiktokAds.valuePerOnlineConsult` | Value per online consult (Page) | AVG | Currency | Average value per online consult event on your landing page. |
| `tiktokAds.totalOnlineConsultValue` | Total online consult value (Page) | SUM | Currency | Total value attributed to online consult events on your landing page. |
| `tiktokAds.userRegistration` | User registrations | SUM | Decimal (×) | Number of user registration events attributed to your ads. |
| `tiktokAds.costPerUserRegistration` | Cost per user registration | AVG | Decimal (×) | Average spend per user registration: spend ÷ user registrations. |
| `tiktokAds.userRegistrationRate` | User registration rate | AVG | Decimal (×) | Percentage of clicks that resulted in a user registration: user registrations ÷ clicks. |
| `tiktokAds.valuePerUserRegistration` | Value per user registration | AVG | Decimal (×) | Average value per user registration event. |
| `tiktokAds.totalUserRegistrationValue` | Total user registration value | AVG | Decimal (×) | Total value attributed to user registration events. |
| `tiktokAds.pageContentViewEvents` | (Page) Content views | SUM |  | Number of content view events on your landing page attributed to your ads. |
| `tiktokAds.costPerPageContentViewEvent` | Cost per content view (Page) | AVG | Currency | Average spend per content view event on your landing page: spend ÷ content views. |
| `tiktokAds.pageContentViewEventRate` | (Page) Content view rate | AVG | Percentage | Percentage of clicks that resulted in a content view on your landing page: content views ÷ clicks. |
| `tiktokAds.valuePerPageContentViewEvent` | Value per content view (Page) | AVG | Currency | Average value per content view event on your landing page. |
| `tiktokAds.totalPageViewContentEventsValue` | Total content view value (Page) | SUM | Currency | Total value attributed to content view events on your landing page. |
| `tiktokAds.productDetailsPageBrowse` | (Page) Product detail views | SUM |  | Number of product detail page views attributed to your ads. |
| `tiktokAds.costPerProductDetailsPageBrowse` | Cost per product detail view (Page) | AVG | Currency | Average spend per product detail page view: spend ÷ product detail views. |
| `tiktokAds.productDetailsPageBrowseRate` | (Page) Product detail view rate | AVG | Percentage | Percentage of clicks that resulted in a product detail page view: product detail views ÷ clicks. |
| `tiktokAds.valuePerProductDetailsPageBrowse` | Value per product detail view (Page) | AVG | Currency | Average value per product detail page view. |
| `tiktokAds.totalProductDetailsPageBrowseValue` | Total product detail view value (Page) | SUM | Currency | Total value attributed to product detail page views. |
| `tiktokAds.billedCost` | Billed cost | SUM | Currency | Total amount billed to your account during the reporting period. |
| `tiktokAds.cashSpend` | Cash spend | SUM | Currency | Amount of spend charged against your cash balance. |
| `tiktokAds.voucherSpend` | Voucher spend | SUM | Currency | Amount of spend charged against your voucher balance. |
| `tiktokAds.realTimeAppInstall` | Real-time app installs | SUM |  | App installs tracked in real time during the reporting period. |
| `tiktokAds.realTimeAppInstallCost` | Real-time app install cost | SUM | Currency | Real-time cost per app install: spend ÷ real-time app installs. |
| `tiktokAds.appInstall` | App installs | SUM |  | Number of app installs attributed to your ads. |
| `tiktokAds.costPerAppInstall` | Cost per app install | AVG | Currency | Average spend per app install: spend ÷ app installs. |
| `tiktokAds.registration` | Registrations (App) | SUM | Decimal (×) | Number of registration events in your app attributed to your ads. |
| `tiktokAds.costPerRegistration` | Cost per registration | AVG | Decimal (×) | Average spend for each registration event: spend ÷ registrations. |
| `tiktokAds.registrationRate` | Registration rate (App) | AVG | Decimal (×) | Percentage of app installs that resulted in a registration: registrations ÷ app installs. |
| `tiktokAds.totalRegistration` | Total registrations (App) | AVG | Decimal (×) | Total number of registration events in your app attributed to your ads. |
| `tiktokAds.costPerTotalRegistration` | Cost per total registration (App) | AVG | Decimal (×) | Average spend per registration in your app: spend ÷ total registrations. |
| `tiktokAds.purchase` | Purchases (App) | SUM |  | Number of purchase events in your app attributed to your ads. |
| `tiktokAds.costPerPurchase` | Cost per purchase | AVG | Currency | Average spend for each purchase: spend ÷ purchases. |
| `tiktokAds.purchaseRate` | Purchase rate (App) | AVG | Percentage | Percentage of app installs that resulted in a purchase: purchases ÷ app installs. |
| `tiktokAds.totalPurchase` | Total purchases (App) | SUM |  | Total number of purchase events in your app attributed to your ads. |
| `tiktokAds.costPerTotalPurchase` | Cost per total purchase (App) | AVG | Currency | Average spend per purchase in your app: spend ÷ total purchases. |
| `tiktokAds.valuePerTotalPurchase` | Value per purchase (App) | AVG | Currency | Average value per purchase in your app. |
| `tiktokAds.totalPurchaseValue` | Total purchase value (App) | SUM | Currency | Total revenue attributed to purchases in your app. |
| `tiktokAds.totalActivePayRoas` | Total active pay ROAS | AVG | Decimal (×) | Return on ad spend for active pay events: total active pay value ÷ spend. |
| `tiktokAds.appEventAddToCart` | Add to cart (App) | SUM |  | Number of add-to-cart events in your app attributed to your ads. |
| `tiktokAds.costPerAppEventAddToCart` | Cost per add to cart (App) | AVG | Currency | Average spend per add-to-cart event in your app: spend ÷ add to carts. |
| `tiktokAds.appEventAddToCartRate` | Add to cart rate (App) | AVG | Percentage | Percentage of app installs that resulted in an add-to-cart: add to carts ÷ app installs. |
| `tiktokAds.totalAppEventAddToCart` | Total add to cart (App) | SUM |  | Total number of add-to-cart events in your app attributed to your ads. |
| `tiktokAds.costPerTotalAppEventAddToCart` | Cost per total add to cart (App) | AVG | Currency | Average spend per total add-to-cart in your app: spend ÷ total add to carts. |
| `tiktokAds.valuePerTotalAppEventAddToCart` | Value per add to cart (App) | AVG | Currency | Average value per add-to-cart event in your app. |
| `tiktokAds.totalAppEventAddToCartValue` | Total add to cart value (App) | SUM | Currency | Total revenue attributed to add-to-cart events in your app. |
| `tiktokAds.checkout` | Checkouts (App) | SUM |  | Number of checkout events in your app attributed to your ads. |
| `tiktokAds.costPerCheckout` | Cost per checkout | AVG | Currency | Average spend for each checkout event: spend ÷ checkouts. |
| `tiktokAds.checkoutRate` | Checkout rate (App) | AVG | Percentage | Percentage of app installs that resulted in a checkout: checkouts ÷ app installs. |
| `tiktokAds.totalCheckout` | Total checkouts (App) | SUM |  | Total number of checkout events in your app attributed to your ads. |
| `tiktokAds.costPerTotalCheckout` | Cost per total checkout (App) | AVG | Currency | Average spend per checkout in your app: spend ÷ total checkouts. |
| `tiktokAds.valuePerCheckout` | Value per checkout (App) | AVG | Currency | Average value per checkout event in your app. |
| `tiktokAds.totalCheckoutValue` | Total checkout value (App) | SUM | Currency | Total revenue attributed to checkout events in your app. |
| `tiktokAds.viewContent` | Content views (App) | SUM |  | Number of content view events in your app attributed to your ads. |
| `tiktokAds.costPerViewContent` | Cost per content view (App) | AVG | Currency | Average spend per content view event in your app: spend ÷ content views. |
| `tiktokAds.viewContentRate` | Content view rate (App) | AVG | Percentage | Percentage of app installs that resulted in a content view: content views ÷ app installs. |
| `tiktokAds.totalViewContent` | Total content views (App) | SUM |  | Total number of content view events in your app attributed to your ads. |
| `tiktokAds.costPerTotalViewContent` | Cost per total content view (App) | AVG | Currency | Average spend per content view in your app: spend ÷ total content views. |
| `tiktokAds.valuePerTotalViewContent` | Value per content view (App) | AVG | Currency | Average value per content view event in your app. |
| `tiktokAds.totalViewContentValue` | Total content view value (App) | SUM | Currency | Total revenue attributed to content view events in your app. |
| `tiktokAds.totalCompletePaymentValue` | Total complete payment value | SUM | Currency | Total revenue attributed to complete payment events. |

### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `tiktokAds.date` | Date | time | Date when the ad metrics were recorded. |
| `tiktokAds.statTimeHour` | Hour | string | Hour when the ad metrics were recorded. |
| `tiktokAds.advertiserId` | Advertiser ID | string | Unique identifier for the TikTok Ads account. |
| `tiktokAds.advertiserName` | Advertiser name | string | Name of the TikTok Ads account. |
| `tiktokAds.currency` | Currency | string | Currency used by the TikTok Ads account. |
| `tiktokAds.campaignId` | Campaign ID | string | Unique identifier for the campaign in TikTok Ads. |
| `tiktokAds.campaignName` | Campaign | string | Name of the ad campaign. Shown in campaign reports and breakdowns. |
| `tiktokAds.adgroupId` | Ad group ID | string | Unique identifier for the ad group in TikTok Ads. |
| `tiktokAds.adgroupName` | Ad group | string | Name of the ad group. Shown in ad group reports and breakdowns. |
| `tiktokAds.adId` | Ad ID | string | Unique identifier for the ad in TikTok Ads. |
| `tiktokAds.adName` | Ad | string | Name of the ad. Shown in ad-level reports and breakdowns. |
| `tiktokAds.campaignBudgetMode` | Campaign budget mode | string | How the campaign budget is set (daily or lifetime). |
| `tiktokAds.campaignObjectiveType` | Campaign objective type | string | Advertising objective type for the campaign (e.g. Traffic, Conversions, App Install). |
| `tiktokAds.campaignBudget` | Campaign budget | string | Daily or lifetime budget amount set for the campaign. |
| `tiktokAds.campaignObjective` | Campaign objective | string | Advertising objective for the campaign (e.g. Reach, Traffic, App Promotion). |
| `tiktokAds.campaignType` | Campaign type | string | Type of campaign (e.g. Auction, Reach & Frequency). |
| `tiktokAds.campaignOperationStatus` | Campaign status | string | Current delivery status of the campaign (e.g. Active, Paused, Deleted). |
| `tiktokAds.campaignAutomationType` | Campaign automation type | string | Type of automation applied to the campaign (e.g. Smart Plus, standard). |
| `tiktokAds.countryCode` | Country code | string | Country code of the audience who saw your ads. |
| `tiktokAds.gender` | Gender | string | Gender of the audience who saw your ads. |
| `tiktokAds.age` | Age | string | Age range of the audience who saw your ads. |
| `tiktokAds.ac` | Audience network | string | Audience network placement where the ad was shown. |
| `tiktokAds.adgroupScheduleStartTime` | Ad group schedule start | string | Start time of the ad group delivery schedule. |
| `tiktokAds.adgroupScheduleEndTime` | Ad group schedule end | string | End time of the ad group delivery schedule. |
| `tiktokAds.adgroupScheduledBudget` | Ad group scheduled budget | string | Scheduled budget for the ad group. |
| `tiktokAds.adgroupSecondaryStatus` | Ad group secondary status | string | Secondary delivery status of the ad group (e.g. Ad group not started, In review). |
| `tiktokAds.adgroupPlacements` | Ad group placements | string | Ad placements selected for the ad group (e.g. TikTok, Pangle). |
| `tiktokAds.adgroupBidType` | Bid type | string | Bid strategy type for the ad group (e.g. Bid Cap, Lowest Cost). |
| `tiktokAds.adgroupScheduleType` | Schedule type | string | Whether the ad group runs continuously or on a set schedule. |
| `tiktokAds.adgroupIsSmartPerformanceCampaign` | Smart Performance Campaign | string | Whether the ad group belongs to a Smart Performance Campaign. |
| `tiktokAds.adgroupBudget` | Ad group budget | string | Budget set for the ad group. |
| `tiktokAds.adgroupOperationStatus` | Ad group status | string | Current delivery status of the ad group (e.g. Active, Paused, Deleted). |
| `tiktokAds.adgroupOptimizationEvent` | Optimization event | string | Conversion event the ad group is optimizing towards. |
| `tiktokAds.adgroupOptimizationGoal` | Optimization goal | string | Goal the ad group is optimized for (e.g. Clicks, Conversions, Reach). |
| `tiktokAds.adgroupBillingEvent` | Billing event | string | The event you are charged for in the ad group (e.g. CPC, CPM, CPV). |
| `tiktokAds.adgroupBudgetMode` | Ad group budget mode | string | Whether the ad group runs on a daily or lifetime budget. |
| `tiktokAds.adgroupPromotionType` | Promotion type | string | Promotion type for the ad group (e.g. App, Website, TikTok video). |
| `tiktokAds.adCallToAction` | Call to action | string | Call-to-action text shown on the ad (e.g. Shop Now, Learn More). |
| `tiktokAds.adLandingPageUrl` | Landing page URL | string | Destination URL for the ad's landing page. |
| `tiktokAds.adIdentityType` | Identity type | string | Type of identity used for the ad (e.g. TikTok account, Custom Identity). |
| `tiktokAds.adAppName` | App name | string | Name of the app being promoted by the ad. |
| `tiktokAds.adAdFormat` | Ad format | string | Format of the ad (e.g. In-Feed, TopView, Brand Takeover). |
| `tiktokAds.adOperationStatus` | Ad status | string | Current delivery status of the ad (e.g. Active, Paused, Deleted). |
| `tiktokAds.adIsAco` | ACO ad | string | Whether the ad uses Automated Creative Optimization (ACO). |
| `tiktokAds.adDisplayName` | Ad display name | string | Display name shown for the ad in TikTok Ads Manager. |
| `tiktokAds.adSecondaryStatus` | Ad secondary status | string | Secondary delivery status of the ad (e.g. Ad not started, In review). |
| `tiktokAds.adOptimizationEvent` | Ad optimization event | string | Conversion event the ad is optimizing towards. |
| `tiktokAds.adVastMoatEnabled` | Ad VAST MOAT enabled | string | Whether the ad has VAST MOAT viewability tracking enabled. |
| `tiktokAds.adText` | Ad text | string | Text or caption copy used in the ad creative. |
| `tiktokAds.adTiktokItemId` | TikTok item ID | string | Unique identifier for the TikTok organic post used in a Spark Ad. |
| `tiktokAds.creativeFormat` | Creative format | string | Format of the ad creative (e.g. image, video). |
| `tiktokAds.creativeDuration` | Creative duration | string | Duration of the video creative in seconds. |
| `tiktokAds.adType` | Ad type | string | Type of TikTok ad (e.g. TopView, Brand Takeover, In-Feed Ad, Spark Ad). |
| `tiktokAds.searchTerms` | Search terms | string | Search keyword entered by users that triggered your ad. |
| `tiktokAds.placement` | Placement | string | Placement where your ad was shown (e.g. TikTok In-Feed, Pangle). |

## Cross-platform fields

`unified.*` fields add TikTok Ads to totals across all your ad platforms, so one question can cover Google Ads, Meta, Microsoft, LinkedIn and TikTok together.

| Unified field | Label | From |
|---|---|---|
| `unified.spend` | Spend | `tiktokAds.spend` |
| `unified.impressions` | Impressions | `tiktokAds.impressions` |
| `unified.reach` | Reach | `tiktokAds.reach` |
| `unified.clicks` | Clicks | `tiktokAds.clicks` |
| `unified.ctr` | CTR | `tiktokAds.ctr` |
| `unified.cpm` | CPM | `tiktokAds.cpm` |
| `unified.cpc` | CPC | `tiktokAds.cpc` |
| `unified.frequency` | Frequency | `tiktokAds.frequency` |
| `unified.conversions` | Conversions | `tiktokAds.conversion` |
| `unified.costPerConversion` | Cost per conversion | `tiktokAds.costPerConversion` |
| `unified.roas` | ROAS | `tiktokAds.completePaymentRoas` |
| `unified.date` | Date | `tiktokAds.date` |
| `unified.campaignName` | Campaign | `tiktokAds.campaignName` |
| `unified.campaignId` | Campaign ID | `tiktokAds.campaignId` |
| `unified.adGroupName` | Ad group | `tiktokAds.adgroupName` |
| `unified.adGroupId` | Ad group ID | `tiktokAds.adgroupId` |
| `unified.adName` | Ad | `tiktokAds.adName` |
| `unified.adId` | Ad ID | `tiktokAds.adId` |
| `unified.placement` | Placement | `tiktokAds.placement` |
| `unified.country` | Country | `tiktokAds.countryCode` |

## Links

- [TikTok Ads on jepto.com](https://www.jepto.com/data-sources/tiktok-ads)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
