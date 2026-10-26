# Jepto

The `jepto` MCP server gives you live marketing data from the user's Jepto account: Google Ads, Google Analytics 4, Search Console, Meta Ads, Microsoft Ads, LinkedIn Ads, TikTok Ads, Google Business Profile, Xero and more. It also reads and manages the account's KPIs and budgets, and builds and edits Jepto reports.

## Signing in

The server uses OAuth. If Jepto tools are missing or return an authentication error, tell the user to run `/mcp auth jepto` and sign in with their Jepto login in the browser window that opens. Sign-in needs a local browser, so it won't work over a plain SSH session.

The user needs a Jepto account with at least one data source connected. Signing in doesn't connect new platforms; that happens in the Jepto app.

## Finding data

1. List the account's clients to get a `clientId`. One connection covers every client the user can see.
2. List the client's data sources to see which platforms are connected.
3. Read the analytics schema for that client before querying. Field names are camelCase and prefixed by source, for example `googleAds.spend` or `googleAds.campaignName`.
4. Query with measures, dimensions, filters and a date range. Date tokens such as `last30days`, `yesterday` and `30DaysAgo` work, as do ISO dates.

Ask which client, platform and date range the user means when it isn't clear.

## Building reports

When the user asks for a report or dashboard, build it in Jepto:

1. Create the report with all its tabs in one call. This only makes empty tabs.
2. For each metric, query it first and make sure rows come back for the date range.
3. Add chart and table cards one at a time with complete configs that use exact field names from the schema, then place them with the tab's full layout.
4. Optionally write the analysis, then load the report to check every tab has cards.

Aim for 3 to 6 cards per tab, pairing number cards with trend or breakdown charts. Report changes save immediately; pass the latest `revisions` on updates and re-read on a conflict. Give the user the editor or preview link when you're done. The `dataChatReport` skill has the full workflow.

## Making changes

KPI, budget and platform action tools that change data return a preview first. Nothing is saved in Jepto or changed in an ad platform until the user confirms. Show the user the preview and wait for their confirmation; don't confirm on their behalf.

## Links

- Documentation and field reference: https://github.com/jepto/mcp
- Help: https://www.jepto.com/help/mcp
