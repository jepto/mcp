# AI Visibility MCP server

Connect AI Visibility to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask how ChatGPT, Google AI Overviews, Google AI Mode, Perplexity, Gemini, Copilot and Claude answer the prompts you track: whether they name you, which sources they cite, the searches they run, what they claim about you and which competitors they name instead.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. The data comes from the prompts you track with AI Visibility in Jepto, with the same 50 metrics and 87 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "What share of AI answers named my brand last month, by platform?"
- "Which domains do AI engines cite most for my prompts?"
- "Which competitors are named more often than me?"
- "What searches did AI engines run when answering my prompts?"
- "Which claims about my brand are wrong?"

You don't need to name fields or tools. The assistant looks up the fields below with `analytics_schema` and runs the query with `analytics_query`.

AI Visibility data is read-only through the MCP. You add and change tracked prompts in Jepto.

## Example query

This is the kind of `analytics_query` call an assistant makes for you. Field names are the same as in Jepto Reports and Data Chat.

```json
{
  "clientId": "<clientId from client_list>",
  "measures": [
    "aiVisibility.sources.citations",
    "aiVisibility.sources.promptsInfluenced"
  ],
  "dimensions": [
    "aiVisibility.sources.domain",
    "aiVisibility.sources.ownership"
  ],
  "dateRange": [
    "last30days"
  ],
  "order": {
    "aiVisibility.sources.citations": "desc"
  },
  "limit": 20
}
```

## Fields

137 fields (50 metrics, 87 dimensions), from version 2.7.3 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Answers

Each AI engine answer to a tracked prompt, with whether it named or cited you.

#### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `aiVisibility.answers.answerCount` | Tracked answers | SUM |  | Number of AI engine answers for the selected filters. |
| `aiVisibility.answers.mentionedCount` | Brand-mentioned answers | SUM |  | Answers that mention the brand (numerator for namedRate rollups). |
| `aiVisibility.answers.citedCount` | Answers citing owned domains | SUM |  | Answers citing owned domains (numerator for citedRate rollups). |
| `aiVisibility.answers.namedRate` | Brand mention rate | Calculated: (Brand-mentioned answers ÷ Tracked answers) × 100 |  | Share of answers mentioning the brand (0–100). Weighted ratio: sum(mentionedCount)/sum(answerCount). |
| `aiVisibility.answers.citedRate` | Owned-domain citation rate | Calculated: (Answers citing owned domains ÷ Tracked answers) × 100 |  | Share of answers citing owned domains (0–100). Weighted ratio: sum(citedCount)/sum(answerCount). |
| `aiVisibility.answers.avgCitedPosition` | Avg cited position | AVG |  | Average citation position when the brand is cited (lower is better). |
| `aiVisibility.answers.distinctSources` | Distinct cited domains (answers) | SUM |  | Distinct citation domains across answers for the prompt. |
| `aiVisibility.answers.promptCount` | Distinct prompts answered | COUNT_DISTINCT |  | Distinct tracked prompts in the filtered answers. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.answers.date` | Answer run date | time | Run date (warehouse partition key). |
| `aiVisibility.answers.id` | Answer row ID | string | Stable 16-character hex id derived from the run key (promptId#platform#date). Join claims via claim.answerId = answers.id. |
| `aiVisibility.answers.promptId` | Tracked prompt | string | Tracked prompt that produced this answer. |
| `aiVisibility.answers.platform` | Answer AI engine | string | AI engine that generated the answer (for example ChatGPT, Claude, or Gemini). |
| `aiVisibility.answers.ranAt` | Ran at | time | Timestamp when the answer was fetched. |
| `aiVisibility.answers.country` | Country | string | Country context used when the prompt was run. |
| `aiVisibility.answers.excerpt` | Answer preview excerpt | string | Short preview of the answer text shown in list views. |
| `aiVisibility.answers.answerMarkdown` | Answer markdown | string | Full answer content in markdown format. |
| `aiVisibility.answers.mentioned` | Mentioned | boolean | Whether the brand was mentioned in the answer. |
| `aiVisibility.answers.cited` | Cited | boolean | Whether owned brand domains were cited as sources in the answer. |
| `aiVisibility.answers.citedPosition` | Cited position | integer | Position of the brand citation in the answer source list (1-based; 0 when not cited). |
| `aiVisibility.answers.sourceDomains` | Source domains | string | Domains cited as sources in the answer. |
| `aiVisibility.answers.competitorsNamed` | Competitors named | string | Competitor brands named in the answer. |
| `aiVisibility.answers.sources` | Sources JSON | string | JSON array of citations in display order: position, label, url (Answers tab Sources accordion). |
| `aiVisibility.answers.entities` | Entities JSON | string | JSON array of brand/entity names also named in the answer (Also Named accordion). |
| `aiVisibility.answers.map` | Map JSON | string | JSON array of local/map listings: name, lat, lng, rating, reviews, category, open, address, image, url. |
| `aiVisibility.answers.relatedQueries` | Related queries JSON | string | JSON array of related follow-up query strings, where the platform returns them. |
| `aiVisibility.answers.citationPills` | Citation pills JSON | string | JSON array of ChatGPT citation pill metadata when present. |
| `aiVisibility.answers.clientName` | Client on answer rows | string | Client display name (account-level queries only). |

### Sources

The domains and pages AI engines cite, and how often.

#### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `aiVisibility.sources.citations` | Citations to domain | SUM |  | Total citation count for the domain in the period. |
| `aiVisibility.sources.citationShare` | Citation share | AVG |  | Share of all citations attributed to this domain (0–100). |
| `aiVisibility.sources.citationChange` | Citation change | AVG |  | Period-over-period change in citation share. |
| `aiVisibility.sources.promptsInfluenced` | Prompts influenced | MAX |  | Distinct tracked prompts where this domain was cited (per domain snapshot). |
| `aiVisibility.sources.lastSeenMax` | Latest domain citation (max) | MAX |  | Most recent citation time for the domain (use instead of grouping by lastSeen). |
| `aiVisibility.sources.avgPosition` | Avg citation rank (domain) | AVG |  | Average citation position for this domain in AI answers (lower is better). |
| `aiVisibility.sources.pageCount` | Pages cited | MAX |  | Distinct canonical cited page URLs for this domain (pages JSON length after merge; UTM and text fragments deduped). |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.sources.date` | Sources rollup date | time | Run date for citation aggregates. |
| `aiVisibility.sources.id` | Source ID | string | Stable 16-character hex id derived from the canonical citation domain. Use domain for display. |
| `aiVisibility.sources.domain` | Citation domain | string | Citation domain influencing AI answers. |
| `aiVisibility.sources.ownership` | Domain ownership | string | Whether the domain is owned by the brand or an earned third-party source. |
| `aiVisibility.sources.category` | Domain category | string | Domain category (for example news, review, or social). |
| `aiVisibility.sources.platforms` | Engines citing domain | string | AI engines where this domain was cited. |
| `aiVisibility.sources.pages` | Pages JSON | string | JSON list of cited pages with URL, title, and position details. |
| `aiVisibility.sources.claimIds` | Claim IDs | string | Claim IDs linked to this citation source. |
| `aiVisibility.sources.recommendedAction` | Recommended action | string | Suggested action to improve visibility for this source. |
| `aiVisibility.sources.lastSeen` | Domain last cited at | time | Most recent time this domain was cited. |
| `aiVisibility.sources.clientName` | Client on source domains | string | Client display name (account-level queries only). |

### Searches

The searches AI engines ran while writing their answers (fan-out queries).

#### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `aiVisibility.retrieval.occurrences` | Occurrences | SUM |  | How many times this query was used during AI answer generation. |
| `aiVisibility.retrieval.searchConsolePosition` | Search Console position | AVG |  | Average Google Search Console position for this query when GSC is connected. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.retrieval.date` | Retrieval query date | time | Run date for retrieval query aggregates. |
| `aiVisibility.retrieval.id` | Retrieval ID | string | Stable 16-character hex id derived from the canonical (lowercase) retrieval query text. Use query for display. |
| `aiVisibility.retrieval.query` | Query | string | Search or retrieval query the AI engine used when answering. |
| `aiVisibility.retrieval.promptIds` | Prompt IDs | string | Tracked prompts that triggered this retrieval query. |
| `aiVisibility.retrieval.platforms` | Engines on retrieval query | string | AI engines that issued this retrieval query. |
| `aiVisibility.retrieval.ownedPageRetrieved` | Owned page retrieved | boolean | Whether an owned brand page appeared in retrieval results. |
| `aiVisibility.retrieval.clientName` | Client on retrieval queries | string | Client display name (account-level queries only). |

### Claims

Statements AI engines make about you, checked for accuracy.

#### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `aiVisibility.claim.claimCount` | Claim rows | COUNT |  | Number of claim rows. |
| `aiVisibility.claim.answerCount` | Distinct answers making claim | COUNT_DISTINCT |  | Distinct answers that made the claim. |
| `aiVisibility.claim.promptCount` | Distinct prompts with claim | COUNT_DISTINCT |  | Distinct prompts whose answers made the claim. |
| `aiVisibility.claim.claimsOk` | Accurate claim rows | SUM |  | Claims graded accurate vs the client profile. |
| `aiVisibility.claim.claimsConflict` | Conflict claim rows | SUM |  | Claims that conflict with the client profile (wrong claims). |
| `aiVisibility.claim.claimsUnverified` | Unverified claim rows | SUM |  | Claims that could not be verified against the profile. |
| `aiVisibility.claim.accuracyRate` | Claim accuracy rate | Calculated: (Accurate claim rows ÷ Claim rows) × 100 |  | Share of claims graded accurate (0–100). |
| `aiVisibility.claim.coveragePrompts` | Coverage prompts | COUNT_DISTINCT |  | Distinct prompts where this claim group appeared. |
| `aiVisibility.claim.coverageUnsolicited` | Unsolicited coverage | COUNT_DISTINCT |  | Distinct prompts where the claim appeared and no row for that prompt shows prompt influence (prompts minus prompt-driven). |
| `aiVisibility.claim.coveragePromptDriven` | Prompt-driven coverage | COUNT_DISTINCT |  | Distinct prompts where the claim appeared because the prompt asked about the claim group. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.claim.date` | Claim run date | time | Answer run date (shared AI Visibility time dimension). |
| `aiVisibility.claim.answerId` | Answer that made claim | string | Answer that made the claim. |
| `aiVisibility.claim.promptId` | Prompt for claim answer | string | Tracked prompt for the answer. |
| `aiVisibility.claim.platform` | Claim answer engine | string | AI engine. |
| `aiVisibility.claim.claimId` | Claim row ID | string | Stable claim id per answer and statement. |
| `aiVisibility.claim.subject` | Subject | string | brand, competitor, or category. |
| `aiVisibility.claim.claimGroupId` | Claim group ID | string | Group id for paraphrases of the same claim. |
| `aiVisibility.claim.claimGroupLabel` | Claim group title | string | Display label for the claim group. |
| `aiVisibility.claim.claimType` | Claim type | string | identity, capability, audience, pricing, or limitation. |
| `aiVisibility.claim.statement` | Statement | string | This answer's wording of the claim. |
| `aiVisibility.claim.excerpt` | Claim quote excerpt | string | Quote from the answer prose. |
| `aiVisibility.claim.accuracy` | Accuracy | string | accurate or conflict for brand claims. |
| `aiVisibility.claim.promptInfluence` | Prompt influence | boolean | Whether the prompt asked about this claim group. |
| `aiVisibility.claim.comparisonGroupId` | Comparison group ID | string | Head-to-head comparison group id. |
| `aiVisibility.claim.comparisonDimension` | Comparison dimension | string | What is compared with a competitor. |
| `aiVisibility.claim.competitor` | Competitor | string | Competitor name for comparison claims. |
| `aiVisibility.claim.competitorDomain` | Comparison competitor domain | string | Competitor primary domain. |
| `aiVisibility.claim.brandComparison` | Brand comparison | string | stronger, weaker, or neutral. |
| `aiVisibility.claim.clientName` | Client on claim rows | string | Client display name (account-level queries only). |

### Claim sources

The pages and passages behind each claim.

#### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `aiVisibility.claimSources.claimCount` | Claim source rows | COUNT_DISTINCT |  | Distinct claims the pages support. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.claimSources.date` | Claim source date | time | Answer run date. |
| `aiVisibility.claimSources.answerId` | Answer for source passage | string | Answer for the claim. |
| `aiVisibility.claimSources.promptId` | Prompt for source passage | string | Prompt for the answer. |
| `aiVisibility.claimSources.platform` | Claim source engine | string | AI engine. |
| `aiVisibility.claimSources.claimId` | Linked claim ID | string | Claim this passage supports. |
| `aiVisibility.claimSources.claimGroupId` | Source claim group ID | string | Claim group id. |
| `aiVisibility.claimSources.claimGroupLabel` | Source claim group title | string | Claim group label. |
| `aiVisibility.claimSources.claimType` | Source claim type | string | Claim type for the group. |
| `aiVisibility.claimSources.url` | URL | string | Canonical cited page URL. |
| `aiVisibility.claimSources.domain` | Passage domain | string | Page domain. |
| `aiVisibility.claimSources.title` | Title | string | Page title. |
| `aiVisibility.claimSources.passage` | Passage | string | Words on the page that support the claim. |
| `aiVisibility.claimSources.highlightUrl` | Highlight URL | string | URL opening the page with passage highlighted. |
| `aiVisibility.claimSources.owned` | Owned | boolean | Page is on a client-owned domain. |
| `aiVisibility.claimSources.via` | Via | string | page, engine, or engine_and_page. |
| `aiVisibility.claimSources.rank` | Rank | integer | 1 = best supporting page for the claim. |
| `aiVisibility.claimSources.clientName` | Client on claim sources | string | Client display name (account-level queries only). |

### Competitors

Which competitors are named and cited, and where they beat you.

#### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `aiVisibility.competitors.answerShare` | Answer share | AVG |  | Share of answer runs that named this entity (0–100). |
| `aiVisibility.competitors.answerShareChange` | Answer share change | AVG |  | Period-over-period change in answer share. |
| `aiVisibility.competitors.mentionRate` | Mention rate | AVG |  | Share of answers mentioning this entity (0–100). |
| `aiVisibility.competitors.citationRate` | Citation rate | AVG |  | Share of answers citing this entity's domains (0–100). |
| `aiVisibility.competitors.avgPosition` | Avg citation rank (competitor) | AVG |  | Average citation position when this entity is cited (lower is better). |
| `aiVisibility.competitors.citedSources` | Cited sources | SUM |  | Number of distinct domains cited for this entity. |
| `aiVisibility.competitors.winningPrompts` | Winning prompts | SUM |  | Number of prompts where this entity's mention rate exceeds the brand's. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.competitors.date` | Competitor metric date | time | Run date for competitor aggregates. |
| `aiVisibility.competitors.id` | Competitor ID | string | Stable 16-character hex id derived from the canonical (lowercase) competitor or brand name. Use name for display. |
| `aiVisibility.competitors.name` | Name | string | Competitor or brand name mentioned in AI answers. |
| `aiVisibility.competitors.domain` | Competitor domain | string | Primary domain associated with the competitor. |
| `aiVisibility.competitors.isBrand` | Is brand | boolean | Whether this row represents the tracked brand rather than a competitor. |
| `aiVisibility.competitors.exclusiveSources` | Exclusive sources | string | Domains cited for this entity but not for the brand. |
| `aiVisibility.competitors.byPlatform` | By platform JSON | string | JSON breakdown of mention rate by AI engine. |
| `aiVisibility.competitors.clientName` | Client on competitor rows | string | Client display name (account-level queries only). |

### Overview

Headline totals: named rate, cited rate, share of voice and claim accuracy.

#### Metrics

| Field | Label | Aggregation | Format | Description |
|---|---|---|---|---|
| `aiVisibility.overview.answerCount` | Overview answer total | SUM |  | Number of AI engine answers for the selected filters. |
| `aiVisibility.overview.mentionedCount` | Overview mentioned-answer total | SUM |  | Answers that mention the brand (numerator for namedRate rollups). |
| `aiVisibility.overview.citedCount` | Overview owned-citation answer total | SUM |  | Answers citing owned domains (numerator for citedRate rollups). |
| `aiVisibility.overview.namedRate` | Overview brand mention rate | Calculated: (Overview mentioned-answer total ÷ Overview answer total) × 100 |  | Share of answers mentioning the brand (0–100). Weighted ratio: sum(mentionedCount)/sum(answerCount). |
| `aiVisibility.overview.citedRate` | Overview owned-citation rate | Calculated: (Overview owned-citation answer total ÷ Overview answer total) × 100 |  | Share of answers citing owned domains (0–100). Weighted ratio: sum(citedCount)/sum(answerCount). |
| `aiVisibility.overview.distinctSources` | Distinct cited domains (overview) | SUM |  | Distinct citation domains across answers. |
| `aiVisibility.overview.claimsNeedingAttention` | Claims needing attention | SUM |  | Brand claims with accuracy conflict in the selected date range (alias of wrongClaims). |
| `aiVisibility.overview.wrongClaims` | Wrong claims | SUM |  | Count of claims with accuracy conflict in the selected date range. |
| `aiVisibility.overview.wrongClaimAnswers` | Wrong claim answers | SUM |  | Distinct answers containing at least one conflict claim in range. |
| `aiVisibility.overview.claimsOk` | Accurate claims in period | SUM |  | Claims graded accurate in the selected date range. |
| `aiVisibility.overview.claimsConflict` | Conflict claims in period | SUM |  | Claims with accuracy conflict in the selected date range. |
| `aiVisibility.overview.claimsUnverified` | Unverified claims in period | SUM |  | Unverified claims in the selected date range. |
| `aiVisibility.overview.accuracyRate` | Period claim accuracy rate | AVG |  | Share of claims graded accurate in range (0–100). Overview entity does not expose claim numerators for weighted period rollup; value comes from the adapter aggregate. |
| `aiVisibility.overview.shareOfVoice` | Share of voice | AVG |  | Brand mention slots as a share of all brand and competitor mention slots in answers (0–100). |
| `aiVisibility.overview.citations` | All-domain citation total | SUM |  | Total citation count across all domains. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.overview.date` | Overview period date | time | Run date for overview aggregates. |
| `aiVisibility.overview.platform` | Overview breakdown engine | string | AI engine for platform-level overview breakdowns. |
| `aiVisibility.overview.ownership` | Overview ownership breakdown | string | Citation ownership (owned vs earned) for overview breakdowns. |
| `aiVisibility.overview.category` | Overview category breakdown | string | Citation domain category for overview breakdowns. |
| `aiVisibility.overview.clientId` | Client ID | string | Client id (account-level portfolio rollups). |
| `aiVisibility.overview.clientName` | Client on overview rollups | string | Client display name (account-level queries only). |

## Links

- [AI Visibility on jepto.com](https://www.jepto.com/data-sources/ai-visibility)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
