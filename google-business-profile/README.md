# Google Business Profile MCP server

Connect Google Business Profile to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about every location you manage: calls, direction requests, website clicks, Search and Maps impressions, reviews, replies, posts and the keywords people used to find you.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your Business Profile data comes from the Business Profile account you've connected in Jepto, with the same 29 metrics and 70 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "Which locations got the most calls and direction requests last month?"
- "Show me unreplied reviews across all locations."
- "Which search keywords found my profiles this month?"
- "Compare Search and Maps impressions on mobile vs desktop for the last 90 days."

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

Google Business Profile is read-only through the MCP: the assistant can query your data but can't change anything in Business Profile.

## Fields

99 fields (29 metrics, 70 dimensions), from version 1.1.11 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Locations

Each location's profile details, categories, address and rating.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `googleBusinessProfile.profileRating` | Profile rating | Calculated | Decimal | Rating for the Profile as shown on Google. |
| `googleBusinessProfile.totalReviewCount` | Total reviews | Calculated |  | Total number of reviews on the location as reported by Google Business Profile (value is updated daily). |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `googleBusinessProfile.locationName` | Location name | string | Name of the business location. |
| `googleBusinessProfile.locationId` | Location ID | string | Unique identifier for the business location in Google Business Profile. |
| `googleBusinessProfile.locationResourceName` | Location resource name | string (calculated) | Google identifier for this location in the form accounts/{accountId}/locations/{locationId}. |
| `googleBusinessProfile.locationLanguageCode` | Location language code | string | Language code for this business location (e.g. en, fr, de). |
| `googleBusinessProfile.storeCode` | Store code | string | Custom identifier assigned to the location in your Business Profile. |
| `googleBusinessProfile.locationWebsite` | Website | string | Website URL for this business location. |
| `googleBusinessProfile.serviceArea` | Service area | string | Geographic area where the business provides services (for service-area businesses). |
| `googleBusinessProfile.locationStatus` | Status | string | Current status of the business location (e.g. Open, Closed, Suspended). |
| `googleBusinessProfile.locationDescription` | Business description | string (calculated) | Short description of the business shown on the profile. |
| `googleBusinessProfile.addressLines` | Street address | string (calculated) | Street address lines from the storefront address, joined for display. |
| `googleBusinessProfile.addressAdministrativeArea` | State / province | string (calculated) | State or province from the storefront address. |
| `googleBusinessProfile.addressLocality` | City | string (calculated) | City or town from the storefront address. |
| `googleBusinessProfile.addressPostalCode` | Postcode | string (calculated) | Postal code from the storefront address. |
| `googleBusinessProfile.addressRegionCode` | Country code | string (calculated) | CLDR region code for the storefront address country. |
| `googleBusinessProfile.addressSublocality` | Suburb | string (calculated) | Neighborhood or suburb from the storefront address when provided by Google Business Profile. |
| `googleBusinessProfile.addressLanguageCode` | Address language | string (calculated) | BCP-47 language code for the storefront address text. |
| `googleBusinessProfile.locationFormattedAddress` | Address | string (calculated) | Formatted storefront address combining street, city, state, and postcode. |
| `googleBusinessProfile.locationPrimaryPhone` | Primary phone | string (calculated) | Main phone number customers can call to reach this business location directly. |
| `googleBusinessProfile.locationAdditionalPhones` | Additional phones | string (calculated) | Additional phone numbers listed for this business location beyond the primary number. |
| `googleBusinessProfile.locationPrimaryCategory` | Primary category | string (calculated) | Category that best describes the core business this location engages in. |
| `googleBusinessProfile.locationAdditionalCategories` | Additional categories | string (calculated) | Additional categories used to further describe this business location. |
| `googleBusinessProfile.regularHours` | Regular hours | object | Standard weekly operating hours for the business location. |
| `googleBusinessProfile.specialHours` | Special hours | object | Special or holiday opening hours for the business location. |
| `googleBusinessProfile.adWordsLocationExtensions` | Ads location extensions | object | Google Ads location extension settings for this business location. |
| `googleBusinessProfile.locationAdPhone` | Ads phone | string (calculated) | Phone number surfaced in Google Ads location extensions for this business. |
| `googleBusinessProfile.latlng` | LatLng Coordinates | object | Geographic coordinates for the business location. |
| `googleBusinessProfile.relationshipData` | Relationship data | object | Related locations and chain data for this business location. |
| `googleBusinessProfile.locationFollowers` | Followers | number | Total number of followers for the location when reported by Google Business Profile. |
| `googleBusinessProfile.openInfo` | Open info | object | Business hours and open/closed status for the location. |
| `googleBusinessProfile.openInfoStatus` | Open status | string (calculated) | Whether the business location is currently open for business. |
| `googleBusinessProfile.attributes` | Business attributes | object[] | Attributes of the business (e.g. accessibility features, accepted payment methods). |
| `googleBusinessProfile.locationPrimaryServices` | Primary services | string (calculated) | Services listed for this business location, based on the primary category. |
| `googleBusinessProfile.locationAdditionalServices` | Additional services | string (calculated) | Services listed for this business location, based on the additional categories. |
| `googleBusinessProfile.locationMapsLink` | Maps Link | string (calculated) | Google Maps URL for this business location. |
| `googleBusinessProfile.locationReviewLink` | Review Link | string (calculated) | Google review URL for this business location. |

### Reviews

Customer reviews, star ratings and whether you've replied.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `googleBusinessProfile.reviewStarRating` | Star rating | AVG |  | Star rating from 1 to 5 given by the reviewer. |
| `googleBusinessProfile.reviewCount` | Review count | COUNT_DISTINCT |  | Number of distinct reviews in the queried date range. |
| `googleBusinessProfile.repliedReviewCount` | Replied reviews | COUNT_DISTINCT |  | Distinct reviews with an owner reply (reply date present). |
| `googleBusinessProfile.unrepliedReviewCount` | Unreplied reviews | Calculated: Review count − Replied reviews |  | Reviews in range without an owner reply. |
| `googleBusinessProfile.replyRate` | Reply rate | Calculated: Replied reviews ÷ Review count |  | Share of reviews in range that have an owner reply. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `googleBusinessProfile.date` | Date | time | Date for which the data was recorded. |
| `googleBusinessProfile.reviewComment` | Review comment | string | Text content of the customer's review. |
| `googleBusinessProfile.reviewReply` | Review reply | string (calculated) | Business owner's reply to the customer review. |
| `googleBusinessProfile.reviewId` | Review ID | string | Unique identifier for the customer review. |
| `googleBusinessProfile.reviewerName` | Reviewer name | string | Name of the reviewer who left the feedback. |
| `googleBusinessProfile.reviewerProfilePhotoUrl` | Reviewer photo URL | string | Profile photo URL of the reviewer who left the feedback. |
| `googleBusinessProfile.reviewReplyDate` | Review reply date | time (calculated) | Date and time when the business owner replied to the review. |
| `googleBusinessProfile.reviewUpdateTime` | Review updated | time | Date and time when the review was last updated. |

### Posts

Updates, offers and events published to your profiles.

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `googleBusinessProfile.postId` | Post ID | string | Unique identifier for the local post. |
| `googleBusinessProfile.postLanguageCode` | Post language code | string | Language code for the post (e.g. en, fr, de). |
| `googleBusinessProfile.postSummary` | Post Summary | string | Text content of the post. |
| `googleBusinessProfile.postTopicType` | Post topic type | string | Type of local post (e.g. Standard, Event, Offer). |
| `googleBusinessProfile.postScheduleDate` | Post schedule date | time | Date the post is scheduled to be published. |
| `googleBusinessProfile.postScheduleTime` | Post schedule time | string | Time the post is scheduled to be published. |
| `googleBusinessProfile.postMediaGoogleUrl` | Post media Google URL | string (calculated) | Google-hosted URL for the first media item attached to the post. |
| `googleBusinessProfile.postCreateTime` | Post created | time | Date and time the post was created. |
| `googleBusinessProfile.postUpdateTime` | Post last updated | time | Date and time the post was last updated. |
| `googleBusinessProfile.postState` | Post state | string | Lifecycle state of the local post. |
| `googleBusinessProfile.postSearchUrl` | Post search URL | string | Google Search URL for the local post. |
| `googleBusinessProfile.postAlertType` | Post alert type | string | Alert type when the post is an alert post. |
| `googleBusinessProfile.postOfferCouponCode` | Post offer coupon code | string (calculated) | Coupon code from the offer when the local post is an offer post. |
| `googleBusinessProfile.postEventTitle` | Post event title | string (calculated) | Title of the event for event-type local posts. |
| `googleBusinessProfile.postEventDetails` | Post event details | string (calculated) | Schedule and details for event-type local posts. |

### Photos & media

Photos and videos on each profile.

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `googleBusinessProfile.authorType` | Author type | string | Whether the media was uploaded by the business owner or a customer (owner, customer). |
| `googleBusinessProfile.mediaGoogleUrl` | Media URL | string | Google-hosted URL for the media asset. |
| `googleBusinessProfile.mediaFormat` | Media format | string | Format of the media item (e.g. Photo, Video). |
| `googleBusinessProfile.mediaThumbnailUrl` | Media thumbnail URL | string | URL for the thumbnail image of the media. |
| `googleBusinessProfile.mediaLocationAssociation` | Location association | string | How this media item is associated with the business location. |
| `googleBusinessProfile.mediaDescription` | Media description | string | Description text for the media item. |
| `googleBusinessProfile.mediaWidth` | Media width | number (calculated) | Width of the media item in pixels. |
| `googleBusinessProfile.mediaHeight` | Media height | number (calculated) | Height of the media item in pixels. |
| `googleBusinessProfile.mediaAttributionName` | Media attribution name | string (calculated) | Display name of the customer who uploaded the media item. |
| `googleBusinessProfile.mediaSourceUrl` | Media source URL | string | Public source URL where the media item can be retrieved. |

### Performance

Daily calls, direction requests, website clicks, bookings and Search and Maps impressions.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `googleBusinessProfile.searchImpressions` | Search impressions | SUM |  | Number of times the business profile appeared in Google Search results. (Desktop + Mobile) |
| `googleBusinessProfile.searchImpressionsDesktop` | Search impressions (desktop) | SUM |  | Number of times the business profile appeared in Google Search results on desktop. |
| `googleBusinessProfile.searchImpressionsMobile` | Search impressions (mobile) | SUM |  | Number of times the business profile appeared in Google Search results on mobile. |
| `googleBusinessProfile.mapsImpressions` | Maps impressions | SUM |  | Number of times the business profile appeared in Google Maps results. (Desktop + Mobile) |
| `googleBusinessProfile.mapsImpressionsDesktop` | Maps impressions (desktop) | SUM |  | Number of times the business profile appeared in Google Maps results on desktop. |
| `googleBusinessProfile.mapsImpressionsMobile` | Maps impressions (mobile) | SUM |  | Number of times the business profile appeared in Google Maps results on mobile. |
| `googleBusinessProfile.directionRequests` | Direction requests | SUM |  | Number of times users requested directions to the business location. |
| `googleBusinessProfile.webClicks` | Website clicks | SUM |  | Number of clicks on the website link from the business profile. |
| `googleBusinessProfile.callClicks` | Call clicks | SUM |  | Number of clicks on the phone number from the business profile. |
| `googleBusinessProfile.foodOrders` | Food orders | SUM |  | Number of food orders placed through the business profile. |
| `googleBusinessProfile.conversations` | Messages | SUM |  | Number of messages sent to the business through the profile. |
| `googleBusinessProfile.bookings` | Bookings | SUM |  | Number of bookings made through the business profile. |
| `googleBusinessProfile.clicks` | Clicks | Calculated: (Website clicks + Call clicks) + Direction requests |  | Number of clicks on the business profile. (Website + Call + Direction Requests) |
| `googleBusinessProfile.impressions` | Impressions | Calculated: Search impressions + Maps impressions |  | Number of times the business profile appeared in Google Search and Maps results. (Desktop + Mobile) |
| `googleBusinessProfile.desktopImpressions` | Impressions (desktop) | Calculated: Search impressions (desktop) + Maps impressions (desktop) |  | Number of times the business profile appeared in Google Search and Maps on desktop. Multiple impressions by a unique user within a single day are counted once. |
| `googleBusinessProfile.mobileImpressions` | Impressions (mobile) | Calculated: Search impressions (mobile) + Maps impressions (mobile) |  | Number of times the business profile appeared in Google Search and Maps on mobile. Multiple impressions by a unique user within a single day are counted once. |
| `googleBusinessProfile.searchClickThroughRate` | Search CTR | Calculated: Website clicks ÷ Search impressions | Percentage | Percentage of Search impressions that resulted in a website click: web clicks ÷ search impressions. |
| `googleBusinessProfile.mapsClickThroughRate` | Maps CTR | Calculated: Website clicks ÷ Maps impressions | Percentage | Percentage of Maps impressions that resulted in a website click: web clicks ÷ maps impressions. |
| `googleBusinessProfile.callClickThroughRateSearch` | Call CTR (Search) | Calculated: Call clicks ÷ Search impressions | Percentage | Percentage of Search impressions that resulted in a call click: call clicks ÷ search impressions. |
| `googleBusinessProfile.callClickThroughRateMaps` | Call CTR (Maps) | Calculated: Call clicks ÷ Maps impressions | Percentage | Percentage of Maps impressions that resulted in a call click: call clicks ÷ maps impressions. |

### Search keywords

The search terms people used to find each profile, by month.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `googleBusinessProfile.searchVolume` | Search Keyword Volume | SUM |  | Number of times users searched with this keyword and saw your business profile. Only keywords with a search volume greater than the threshold are included. |
| `googleBusinessProfile.searchVolumeThreshold` | Search Keyword Threshold | SUM |  | Minimum search volume threshold at which keyword impressions are reported by Google. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `googleBusinessProfile.searchKeyword` | Search Keyword | string | Search keyword that triggered an impression of your business profile. |
| `googleBusinessProfile.isBranded` | Branded search | boolean | True when the search keyword contains the location title (case-insensitive). Requires locationName in the same query. |

## Cross-platform fields

`unified.*` fields add Google Business Profile to totals across all your ad platforms, so one question can cover Google Ads, Meta, Microsoft, LinkedIn and TikTok together.

| Unified field | Label | From |
|---|---|---|
| `unified.impressions` | Impressions | `googleBusinessProfile.impressions` |
| `unified.clicks` | Clicks | `googleBusinessProfile.clicks` |
| `unified.date` | Date | `googleBusinessProfile.date` |

## Links

- [Google Business Profile on jepto.com](https://www.jepto.com/data-sources/google-business-profile)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
