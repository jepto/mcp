---
trigger: model_decision
description: "Use when the user asks about marketing or advertising performance, Jepto clients, KPIs or budgets, or data from Google Ads, GA4, Search Console, Meta Ads, Google Business Profile or other sources connected to Jepto."
---

# Jepto

The `jepto` MCP server gives you live marketing data from the user's Jepto account: Google Ads, Google Analytics 4, Search Console, Meta Ads, Microsoft Ads, LinkedIn Ads, TikTok Ads, Google Business Profile, Xero and more. It also reads and manages the account's KPIs and budgets.

## Signing in

The server uses OAuth. If Jepto tools are missing or return an authentication error, tell the user to sign in: in the CLI, open `/mcp`, select `jepto` and choose Authenticate; in the app, open Agent settings, go to Customizations and click Authenticate next to `jepto`. They sign in with their Jepto login, then paste the code shown back into Antigravity.

The user needs a Jepto account with at least one data source connected. Signing in doesn't connect new platforms; that happens in the Jepto app.

## Finding data

1. List the account's clients to get a `clientId`. One connection covers every client the user can see.
2. List the client's data sources to see which platforms are connected.
3. Read the analytics schema for that client before querying. Field names are camelCase and prefixed by source, for example `googleAds.spend` or `googleAds.campaignName`.
4. Query with measures, dimensions, filters and a date range. Date tokens such as `last30days`, `yesterday` and `30DaysAgo` work, as do ISO dates.

Ask which client, platform and date range the user means when it isn't clear.

## Making changes

KPI, budget and platform action tools that change data return a preview first. Nothing is saved in Jepto or changed in an ad platform until the user confirms. Show the user the preview and wait for their confirmation; don't confirm on their behalf.

## Links

- Documentation and field reference: https://github.com/jepto/mcp
- Help: https://www.jepto.com/help/mcp
