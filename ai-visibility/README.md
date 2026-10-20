# Jepto AI Visibility MCP server

Connect Jepto AI Visibility to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask how ChatGPT, Gemini, Perplexity, Claude, Copilot and Google AI Mode answer the prompts you track: whether they name you, which sources they cite, the searches they run, what they claim about you and which competitors they name instead.

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

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

AI Visibility data is read-only through the MCP. You add and change tracked prompts in Jepto.

## Example query

This is the kind of `Analytics_Query` call an assistant makes for you. Field names are the same as in Jepto Reports and Data Chat.

```json
{
  "clientId": "<clientId from Client_List>",
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

137 fields (50 metrics, 87 dimensions), from version 2.7.0 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Answers

Each AI engine answer to a tracked prompt, with whether it named or cited you.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `aiVisibility.answers.answerCount` | Answer count | SUM |  | Number of AI engine answers for the selected filters. |
| `aiVisibility.answers.mentionedCount` | Mentioned answers | SUM |  | Answers that mention the brand (numerator for namedRate rollups). |
| `aiVisibility.answers.citedCount` | Cited answers | SUM |  | Answers citing owned domains (numerator for citedRate rollups). |
| `aiVisibility.answers.namedRate` | Named rate | SUM |  | Share of answers mentioning the brand (0–100). Weighted ratio: sum(mentionedCount)/sum(answerCount). |
| `aiVisibility.answers.citedRate` | Cited rate | SUM |  | Share of answers citing owned domains (0–100). Weighted ratio: sum(citedCount)/sum(answerCount). |
| `aiVisibility.answers.avgCitedPosition` | Avg cited position | AVG |  | Average citation position when the brand is cited (lower is better). |
| `aiVisibility.answers.distinctSources` | Distinct sources | SUM |  | Distinct citation domains across answers for the prompt. |
| `aiVisibility.answers.promptCount` | Prompt count | COUNT_DISTINCT |  | Distinct tracked prompts in the filtered answers. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.answers.date` | Date | time | Run date (warehouse partition key). |
| `aiVisibility.answers.id` | Answer ID | string | Stable 16-character hex id derived from the run key (promptId#platform#date). Join claims via claim.answerId = answers.id. |
| `aiVisibility.answers.promptId` | Prompt ID | string | Tracked prompt that produced this answer. |
| `aiVisibility.answers.platform` | Platform | string | AI engine that generated the answer (for example ChatGPT, Claude, or Gemini). |
| `aiVisibility.answers.ranAt` | Ran at | time | Timestamp when the answer was fetched. |
| `aiVisibility.answers.country` | Country | string | Country context used when the prompt was run. |
| `aiVisibility.answers.excerpt` | Excerpt | string | Short preview of the answer text shown in list views. |
| `aiVisibility.answers.answerMarkdown` | Answer markdown | string | Full answer content in markdown format. |
| `aiVisibility.answers.mentioned` | Mentioned | boolean | Whether the brand was mentioned in the answer. |
| `aiVisibility.answers.cited` | Cited | boolean | Whether owned brand domains were cited as sources in the answer. |
| `aiVisibility.answers.citedPosition` | Cited position | integer | Position of the brand citation in the answer source list (1-based; 0 when not cited). |
| `aiVisibility.answers.sourceDomains` | Source domains | string | Domains cited as sources in the answer. |
| `aiVisibility.answers.competitorsNamed` | Competitors named | string | Competitor brands named in the answer. |
| `aiVisibility.answers.sources` | Sources JSON | string | JSON array of citations in display order: position, label, url (Answers tab Sources accordion). |
| `aiVisibility.answers.entities` | Entities JSON | string | JSON array of brand/entity names also named in the answer (Also Named accordion). |
| `aiVisibility.answers.map` | Map JSON | string | JSON array of local/map listings: name, lat, lng, rating, reviews, category, open, address, image, url. |
| `aiVisibility.answers.relatedQueries` | Related queries JSON | string | JSON array of related follow-up query strings (e.g. Perplexity). |
| `aiVisibility.answers.citationPills` | Citation pills JSON | string | JSON array of ChatGPT citation pill metadata when present. |
| `aiVisibility.answers.clientName` | Client name | string | Client display name (account-level queries only). |

### Sources

The domains and pages AI engines cite, and how often.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `aiVisibility.sources.citations` | Citations | SUM |  | Total citation count for the domain in the period. |
| `aiVisibility.sources.citationShare` | Citation share | AVG |  | Share of all citations attributed to this domain (0–100). |
| `aiVisibility.sources.citationChange` | Citation change | AVG |  | Period-over-period change in citation share. |
| `aiVisibility.sources.promptsInfluenced` | Prompts influenced | MAX |  | Distinct tracked prompts where this domain was cited (per domain snapshot). |
| `aiVisibility.sources.lastSeenMax` | Last seen | MAX |  | Most recent citation time for the domain (use instead of grouping by lastSeen). |
| `aiVisibility.sources.avgPosition` | Avg position | AVG |  | Average citation position for this domain in AI answers (lower is better). |
| `aiVisibility.sources.pageCount` | Pages cited | MAX |  | Distinct canonical cited page URLs for this domain (pages JSON length after merge; UTM and text fragments deduped). |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.sources.date` | Date | time | Run date for citation aggregates. |
| `aiVisibility.sources.id` | Source ID | string | Stable 16-character hex id derived from the canonical citation domain. Use domain for display. |
| `aiVisibility.sources.domain` | Domain | string | Citation domain influencing AI answers. |
| `aiVisibility.sources.ownership` | Ownership | string | Whether the domain is owned by the brand or an earned third-party source. |
| `aiVisibility.sources.category` | Category | string | Domain category (for example news, review, or social). |
| `aiVisibility.sources.platforms` | Platforms | string | AI engines where this domain was cited. |
| `aiVisibility.sources.pages` | Pages JSON | string | JSON list of cited pages with URL, title, and position details. |
| `aiVisibility.sources.claimIds` | Claim IDs | string | Claim IDs linked to this citation source. |
| `aiVisibility.sources.recommendedAction` | Recommended action | string | Suggested action to improve visibility for this source. |
| `aiVisibility.sources.lastSeen` | Last seen | time | Most recent time this domain was cited. |
| `aiVisibility.sources.clientName` | Client name | string | Client display name (account-level queries only). |

### Searches

The searches AI engines ran while writing their answers (fan-out queries).

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `aiVisibility.retrieval.occurrences` | Occurrences | SUM |  | How many times this query was used during AI answer generation. |
| `aiVisibility.retrieval.searchConsolePosition` | Search Console position | AVG |  | Average Google Search Console position for this query when GSC is connected. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.retrieval.date` | Date | time | Run date for retrieval query aggregates. |
| `aiVisibility.retrieval.id` | Retrieval ID | string | Stable 16-character hex id derived from the canonical (lowercase) retrieval query text. Use query for display. |
| `aiVisibility.retrieval.query` | Query | string | Search or retrieval query the AI engine used when answering. |
| `aiVisibility.retrieval.promptIds` | Prompt IDs | string | Tracked prompts that triggered this retrieval query. |
| `aiVisibility.retrieval.platforms` | Platforms | string | AI engines that issued this retrieval query. |
| `aiVisibility.retrieval.ownedPageRetrieved` | Owned page retrieved | boolean | Whether an owned brand page appeared in retrieval results. |
| `aiVisibility.retrieval.clientName` | Client name | string | Client display name (account-level queries only). |

### Claims

Statements AI engines make about you, checked for accuracy.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `aiVisibility.claim.claimCount` | Claim count | COUNT |  | Number of claim rows. |
| `aiVisibility.claim.answerCount` | Answer count | COUNT_DISTINCT |  | Distinct answers that made the claim. |
| `aiVisibility.claim.promptCount` | Prompt count | COUNT_DISTINCT |  | Distinct prompts whose answers made the claim. |
| `aiVisibility.claim.claimsOk` | Accurate claims | SUM |  | Claims graded accurate vs the client profile. |
| `aiVisibility.claim.claimsConflict` | Conflict claims | SUM |  | Claims that conflict with the client profile (wrong claims). |
| `aiVisibility.claim.claimsUnverified` | Unverified claims | SUM |  | Claims that could not be verified against the profile. |
| `aiVisibility.claim.accuracyRate` | Accuracy rate | SUM |  | Share of claims graded accurate (0–100). |
| `aiVisibility.claim.coveragePrompts` | Coverage prompts | COUNT_DISTINCT |  | Distinct prompts where this claim group appeared. |
| `aiVisibility.claim.coverageUnsolicited` | Unsolicited coverage | COUNT_DISTINCT |  | Distinct prompts where the claim appeared and no row for that prompt shows prompt influence (prompts minus prompt-driven). |
| `aiVisibility.claim.coveragePromptDriven` | Prompt-driven coverage | COUNT_DISTINCT |  | Distinct prompts where the claim appeared because the prompt asked about the claim group. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.claim.date` | Date | time | Answer run date (shared AI Visibility time dimension). |
| `aiVisibility.claim.answerId` | Answer ID | string | Answer that made the claim. |
| `aiVisibility.claim.promptId` | Prompt ID | string | Tracked prompt for the answer. |
| `aiVisibility.claim.platform` | Platform | string | AI engine. |
| `aiVisibility.claim.claimId` | Claim ID | string | Stable claim id per answer and statement. |
| `aiVisibility.claim.subject` | Subject | string | brand, competitor, or category. |
| `aiVisibility.claim.claimGroupId` | Claim group ID | string | Group id for paraphrases of the same claim. |
| `aiVisibility.claim.claimGroupLabel` | Claim group label | string | Display label for the claim group. |
| `aiVisibility.claim.claimType` | Claim type | string | identity, capability, audience, pricing, or limitation. |
| `aiVisibility.claim.statement` | Statement | string | This answer's wording of the claim. |
| `aiVisibility.claim.excerpt` | Excerpt | string | Quote from the answer prose. |
| `aiVisibility.claim.accuracy` | Accuracy | string | accurate or conflict for brand claims. |
| `aiVisibility.claim.promptInfluence` | Prompt influence | boolean | Whether the prompt asked about this claim group. |
| `aiVisibility.claim.comparisonGroupId` | Comparison group ID | string | Head-to-head comparison group id. |
| `aiVisibility.claim.comparisonDimension` | Comparison dimension | string | What is compared with a competitor. |
| `aiVisibility.claim.competitor` | Competitor | string | Competitor name for comparison claims. |
| `aiVisibility.claim.competitorDomain` | Competitor domain | string | Competitor primary domain. |
| `aiVisibility.claim.brandComparison` | Brand comparison | string | stronger, weaker, or neutral. |
| `aiVisibility.claim.clientName` | Client name | string | Client display name (account-level queries only). |

### Claim sources

The pages and passages behind each claim.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `aiVisibility.claimSources.claimCount` | Claim count | COUNT_DISTINCT |  | Distinct claims the pages support. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.claimSources.date` | Date | time | Answer run date. |
| `aiVisibility.claimSources.answerId` | Answer ID | string | Answer for the claim. |
| `aiVisibility.claimSources.promptId` | Prompt ID | string | Prompt for the answer. |
| `aiVisibility.claimSources.platform` | Platform | string | AI engine. |
| `aiVisibility.claimSources.claimId` | Claim ID | string | Claim this passage supports. |
| `aiVisibility.claimSources.claimGroupId` | Claim group ID | string | Claim group id. |
| `aiVisibility.claimSources.claimGroupLabel` | Claim group label | string | Claim group label. |
| `aiVisibility.claimSources.claimType` | Claim type | string | Claim type for the group. |
| `aiVisibility.claimSources.url` | URL | string | Canonical cited page URL. |
| `aiVisibility.claimSources.domain` | Domain | string | Page domain. |
| `aiVisibility.claimSources.title` | Title | string | Page title. |
| `aiVisibility.claimSources.passage` | Passage | string | Words on the page that support the claim. |
| `aiVisibility.claimSources.highlightUrl` | Highlight URL | string | URL opening the page with passage highlighted. |
| `aiVisibility.claimSources.owned` | Owned | boolean | Page is on a client-owned domain. |
| `aiVisibility.claimSources.via` | Via | string | page, engine, or engine_and_page. |
| `aiVisibility.claimSources.rank` | Rank | integer | 1 = best supporting page for the claim. |
| `aiVisibility.claimSources.clientName` | Client name | string | Client display name (account-level queries only). |

### Competitors

Which competitors are named and cited, and where they beat you.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `aiVisibility.competitors.answerShare` | Answer share | AVG |  | Share of answer runs that named this entity (0–100). |
| `aiVisibility.competitors.answerShareChange` | Answer share change | AVG |  | Period-over-period change in answer share. |
| `aiVisibility.competitors.mentionRate` | Mention rate | AVG |  | Share of answers mentioning this entity (0–100). |
| `aiVisibility.competitors.citationRate` | Citation rate | AVG |  | Share of answers citing this entity's domains (0–100). |
| `aiVisibility.competitors.avgPosition` | Avg position | AVG |  | Average citation position when this entity is cited (lower is better). |
| `aiVisibility.competitors.citedSources` | Cited sources | SUM |  | Number of distinct domains cited for this entity. |
| `aiVisibility.competitors.winningPrompts` | Winning prompts | SUM |  | Number of prompts where this entity's mention rate exceeds the brand's. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.competitors.date` | Date | time | Run date for competitor aggregates. |
| `aiVisibility.competitors.id` | Competitor ID | string | Stable 16-character hex id derived from the canonical (lowercase) competitor or brand name. Use name for display. |
| `aiVisibility.competitors.name` | Name | string | Competitor or brand name mentioned in AI answers. |
| `aiVisibility.competitors.domain` | Domain | string | Primary domain associated with the competitor. |
| `aiVisibility.competitors.isBrand` | Is brand | boolean | Whether this row represents the tracked brand rather than a competitor. |
| `aiVisibility.competitors.exclusiveSources` | Exclusive sources | string | Domains cited for this entity but not for the brand. |
| `aiVisibility.competitors.byPlatform` | By platform JSON | string | JSON breakdown of mention rate by AI engine. |
| `aiVisibility.competitors.clientName` | Client name | string | Client display name (account-level queries only). |

### Overview

Headline totals: named rate, cited rate, share of voice and claim accuracy.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `aiVisibility.overview.answerCount` | Answer count | SUM |  | Number of AI engine answers for the selected filters. |
| `aiVisibility.overview.mentionedCount` | Mentioned answers | SUM |  | Answers that mention the brand (numerator for namedRate rollups). |
| `aiVisibility.overview.citedCount` | Cited answers | SUM |  | Answers citing owned domains (numerator for citedRate rollups). |
| `aiVisibility.overview.namedRate` | Named rate | SUM |  | Share of answers mentioning the brand (0–100). Weighted ratio: sum(mentionedCount)/sum(answerCount). |
| `aiVisibility.overview.citedRate` | Cited rate | SUM |  | Share of answers citing owned domains (0–100). Weighted ratio: sum(citedCount)/sum(answerCount). |
| `aiVisibility.overview.distinctSources` | Distinct sources | SUM |  | Distinct citation domains across answers. |
| `aiVisibility.overview.claimsNeedingAttention` | Claims needing attention | SUM |  | Brand claims with accuracy conflict in the selected date range (alias of wrongClaims). |
| `aiVisibility.overview.wrongClaims` | Wrong claims | SUM |  | Count of claims with accuracy conflict in the selected date range. |
| `aiVisibility.overview.wrongClaimAnswers` | Wrong claim answers | SUM |  | Distinct answers containing at least one conflict claim in range. |
| `aiVisibility.overview.claimsOk` | Accurate claims | SUM |  | Claims graded accurate in the selected date range. |
| `aiVisibility.overview.claimsConflict` | Conflict claims | SUM |  | Claims with accuracy conflict in the selected date range. |
| `aiVisibility.overview.claimsUnverified` | Unverified claims | SUM |  | Unverified claims in the selected date range. |
| `aiVisibility.overview.accuracyRate` | Accuracy rate | SUM |  | Share of claims graded accurate in range (0–100). |
| `aiVisibility.overview.shareOfVoice` | Share of voice | SUM |  | Brand mention slots as a share of all brand and competitor mention slots in answers (0–100). |
| `aiVisibility.overview.citations` | Citations | SUM |  | Total citation count across all domains. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `aiVisibility.overview.date` | Date | time | Run date for overview aggregates. |
| `aiVisibility.overview.platform` | Platform | string | AI engine for platform-level overview breakdowns. |
| `aiVisibility.overview.ownership` | Ownership | string | Citation ownership (owned vs earned) for overview breakdowns. |
| `aiVisibility.overview.category` | Category | string | Citation domain category for overview breakdowns. |
| `aiVisibility.overview.clientId` | Client ID | string | Client id (account-level portfolio rollups). |
| `aiVisibility.overview.clientName` | Client name | string | Client display name (account-level queries only). |

## Links

- [Jepto AI Visibility on jepto.com](https://www.jepto.com/data-sources/ai-visibility)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
