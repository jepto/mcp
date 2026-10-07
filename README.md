# Jepto MCP

Your marketing data in Claude, ChatGPT, Cursor and any other app that supports MCP (Model Context Protocol). Ask about campaigns, KPIs, budgets and clients in plain English, and get answers from your live data without leaving the chat.

```
https://mcp.jepto.com
```

Jepto runs this as a hosted, remote MCP server, so there's nothing to install or run. This repository is its documentation. You need a Jepto account with at least one data source connected; [start a free 14-day trial](https://app.jepto.com/sign-up) if you don't have one.

## Connect

| | |
|---|---|
| Server URL | `https://mcp.jepto.com` |
| Transport | Streamable HTTP |
| Authentication | OAuth with PKCE. You sign in to Jepto once when you connect. |
| Access | Your Jepto account: every client your login can see in the app |

### Claude

1. Go to **Settings → Connectors**.
2. Choose **Add custom connector** and paste `https://mcp.jepto.com`.
3. Click **Connect** and sign in to Jepto.

Full steps with screenshots: [Connect Jepto MCP to Claude](https://www.jepto.com/help/connect-to-claude).

### ChatGPT

Open the apps directory and search for "Jepto", or add a custom connector with the URL above. Then, in the composer, click **+ → More → Jepto**. The Jepto pill stays active for that conversation.

### Claude Code

```bash
claude mcp add --transport http jepto https://mcp.jepto.com
```

Then run `/mcp` to sign in.

### Cursor

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "jepto": {
      "url": "https://mcp.jepto.com"
    }
  }
}
```

### VS Code

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "jepto": {
      "type": "http",
      "url": "https://mcp.jepto.com"
    }
  }
}
```

### Any other MCP client

Look for "Add custom connector" or "remote server", paste the URL and sign in. The client must support OAuth with PKCE; there's no API-key option.

## What you can ask

You don't need to name tools. Ask a question and the assistant picks the right ones. Being clear about the client, platform and date range gets better answers.

**Check on everything**
- "Which KPIs or budgets are critical or at risk across my account?"
- "What plan am I on and how many clients do I have?"
- "Which clients run Google Ads and Facebook Ads?"

**Analyze performance**
- "Compare Homer's Donuts' conversion rate this quarter vs last, by channel."
- "Show me Homer's Donuts' daily spend vs target for this month."
- "Forecast ROAS for Homer's Donuts for the rest of the quarter."

**Prepare for a meeting**
- "Tell me about Homer's Donuts. What do they sell, and which channels are running?"
- "What's Homer's Donuts' target audience and brand tone?"
- "Pull up the latest strategy notes for Homer's Donuts."

**Make changes** (always previewed first)
- "Create a CPA KPI for Homer's Donuts: under $50, monthly."
- "Create a budget for Homer's Donuts: $1,500 over 65 days, all Google Ads campaigns."
- "Pause the brand campaign in Homer's Donuts' Google Ads account."

## Tools

Read tools return data and never change anything. Write tools return a preview first, and nothing is saved in Jepto or changed in an ad platform until you confirm it in the chat. Pending confirmations expire after about 10 minutes.

| Tool | What it does | Mode |
|---|---|---|
| `Account_Info` | Account name, plan, status, timezone, week start, and counts of clients, KPIs and budgets | Read |
| `Client_List` | Every client, with its `clientId`, name and domain | Read |
| `Client_Info` | A client's profile: industry, location, products and services, contacts and business model | Read |
| `Client_Knowledgebase` | Searches a client's stored notes, briefs and brand guidelines | Read |
| `Datasource_List` | A client's connected data sources, or every source type Jepto supports | Read |
| `Analytics_Schema` | The metrics and dimensions available for a client | Read |
| `Analytics_Query` | Queries live data with measures, dimensions, filters, date ranges, period comparisons and sorting | Read |
| `KPI_Forecasting` | Lists, reads, creates, updates and deletes KPIs and their forecasts | Read + write |
| `Budget_Tracker` | Lists, reads, creates, updates and deletes ad spend budgets | Read + write |
| `Actions_List` | The catalog of platform actions you can run, with inputs and risk level | Read |
| `Actions_Execute` | Runs a platform action after you confirm it | Write |
| `Skills_List` | Jepto's catalog of skills: task guides the assistant can follow | Read |
| `Skills_Get` | One skill's full instructions | Read |

### Platform actions

Today the catalog has two Google Ads actions. Both are medium risk and reversible:

- `googleAdsPauseCampaign`: pauses campaigns. Ad delivery stops immediately.
- `googleAdsEnableCampaign`: re-enables paused campaigns. Ad delivery resumes immediately.

More actions will be added. `Actions_List` always shows what's available.

### How the assistant finds your data

The usual order is `Client_List` → `Datasource_List` → `Analytics_Schema` → `Analytics_Query`. Fields use the same names as Jepto Reports and Data Chat, for example `googleAds.spend` or `googleAds.campaignName`. Dates accept tokens like `last30days`, `yesterday` and `30DaysAgo`, as well as ISO dates. See [Data Model](https://www.jepto.com/help/data-model) and [Data Query](https://www.jepto.com/help/data-query).

## Data sources

The MCP sees the sources connected in your Jepto account:

- **Advertising:** Google Ads, Facebook Ads, Instagram Ads, Microsoft Ads, LinkedIn Ads, TikTok Ads, X (Twitter) Ads
- **Analytics and search:** Google Analytics 4, Google Search Console
- **Local:** Google Business Profile
- **Ecommerce:** Shopify (beta)
- **Finance:** Xero
- **AI search:** Jepto AI Visibility
- **Your own data:** BigQuery tables you connect to Jepto

Signing in to the MCP doesn't connect new platforms. [Connect a data source](https://www.jepto.com/help/connect-a-data-source) in Jepto first.

## Security and permissions

- You sign in with your Jepto login. Every tool call runs with the same permissions you have in the app.
- The connection covers your whole Jepto account, not one client. Without a `clientId`, list tools return everything your login can see.
- Access tokens last 60 minutes and refresh tokens 30 days, with rotation on each refresh.
- Your Google, Meta and other platform credentials stay in Jepto. They're never sent to the AI client or the conversation.
- Anything that changes data goes through a preview and confirmation step.
- In Claude, you can require approval before each tool call.

More detail: [OAuth & Authentication](https://www.jepto.com/help/oauth-authentication).

## Links

- [Jepto MCP](https://www.jepto.com/integration/mcp)
- [Jepto MCP Server Overview](https://www.jepto.com/help/mcp)
- [Connect Jepto MCP to Claude](https://www.jepto.com/help/connect-to-claude)
- [OAuth & Authentication](https://www.jepto.com/help/oauth-authentication)
- [Pricing](https://www.jepto.com/pricing)
- [Get support](https://www.jepto.com/help/submit-a-ticket)

## License

The documentation in this repository is MIT licensed. Use of the Jepto MCP server is subject to the [Jepto Terms of Service](https://www.jepto.com/terms).
