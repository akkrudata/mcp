# AkkruData MCP

SEC filings, financial statements, metrics, insider and institutional holdings as
structured data — a hosted, remote [MCP](https://modelcontextprotocol.io) server for
AI agents.

```
https://api.akkrudata.ai/mcp
```

Streamable HTTP · OAuth 2.0 · 50 read-only tools

This repository holds the connector manifest and the setup notes. The server itself is
hosted by us — there is nothing to install or run.

---

## What it gives an agent

Concept, unit, period and dimensions travel with every value, so a figure cannot be read
out of context.

- **One click from a number to its disclosure.** Every fact carries a source locator
  resolving to that figure in the original document.
- **As of a date, not as of today.** `as_of_date` returns only what was filed by then, so
  a backtest cannot see the future.
- **Restatements stay visible.** A figure an amendment changed comes back marked
  `restated`, with the earlier value beside it.
- **Calculated numbers show their work.** Metrics carry their derivation and the fact ids
  behind them.
- **One schema across five markets.** Facts and statements answer the same calls from
  SEC, DART, EDINET, ESEF and CSRC filings.
- **Caveats travel with the data.** Split adjustments, confidential or withdrawn 13F
  rows, missing comparison bases — flagged in the response itself.

| Area | Tools |
| --- | --- |
| **Financial facts** | `get_filing_facts`, `query_line_items`, `compare_facts`, `compare_line_items`, `get_dimensional_breakdown_by_fact_id`, `get_dimensional_breakdown_by_line_item` |
| **Statements** | `get_income_statement`, `get_balance_sheet`, `get_cash_flow_statement`, `get_equity_statement`, `get_comprehensive_income`, `get_filing_statement`, `list_filing_statements` |
| **Calculated metrics** | `get_metrics_bundle`, `get_metrics_subset`, `get_metrics_timeseries`, `get_profitability_metrics`, `get_valuation_metrics`, `get_growth_metrics`, `get_efficiency_metrics`, `get_cash_flow_metrics`, `get_financial_health_metrics`, and the filing-date variants |
| **Insider activity (Form 4)** | `list_insider_transactions`, `search_insider_trades`, `get_insider_transactions_by_id`, `get_insider_stats`, `list_company_insiders`, `list_insider_tickers`, `list_insider_transaction_types` |
| **Initial holdings (Form 3)** | `list_initial_holdings`, `search_initial_holdings`, `get_initial_holdings_by_id`, `get_initial_holding_stats` |
| **Institutional holdings (13F)** | `get_institutional_portfolio`, `get_institutional_manager_history`, `get_institutional_security_holders`, `search_institutional_holdings` |
| **Corporate events (8-K)** | `list_event_filings`, `get_event_filing`, `list_event_types`, `search_events` |
| **Discovery & screening** | `lookup_company`, `list_companies`, `list_filings`, `search_stocks`, `list_metric_snapshots`, `list_screener_filters`, `get_filing_excel` |

Every tool is read-only and annotated as such (`readOnlyHint`, `destructiveHint`,
`openWorldHint`), so a client can tell at a glance that nothing here writes.

Coverage spans the US (SEC EDGAR), Korea (DART), Japan (EDINET), Europe (ESEF) and China
A-share filings. What a given account can reach depends on its plan — see
[Access](#access).

---

## Connect

The server speaks streamable HTTP and authenticates with OAuth 2.0 (dynamic client
registration), so most clients only need the URL.

### Gemini CLI

```bash
gemini extensions install https://github.com/akkrudata/mcp
```

Then authenticate once:

```
/mcp auth akkrudata
```

### Claude

Add a custom connector under Settings → Connectors, paste the server URL, and complete
the sign-in prompt.

### ChatGPT

Enable developer mode, add a connector with the server URL, and complete the OAuth flow.

### Cursor and other MCP clients

Point the client at the same URL over streamable HTTP. Most take a `url`; some name the
field differently — Gemini CLI, above, uses `httpUrl`.

```json
{
  "mcpServers": {
    "akkrudata": {
      "url": "https://api.akkrudata.ai/mcp"
    }
  }
}
```

### Grok

Open Connectors — the **+** button on web, Settings → Connectors on iOS and Android —
choose **Bring Your Own MCP**, and paste the server URL.

---

## Access

A free AkkruData account is required. Sign-up is self-serve with email verification —
no approval, no invite, and no OAuth app of your own to register.

| Plan | Reaches |
| --- | --- |
| **Free** | S&P 500 issuers, three years of history |
| **Starter** | adds Russell 3000 and the calculated metrics |
| **Pro** | adds screening and the non-US markets (KR / JP / EU / CN) |

Usage is metered in credits. Current plans and limits are on the
[pricing page](https://www.akkrudata.ai/pricing).

Institutional holdings (13F) are organised by filing manager rather than by company, and
are not gated by market coverage.

---

## Free, no account needed

Two public pages serve the same ownership data without signing in — the last three years,
with everything linked back to its SEC source:

- [Insider trading and holders by company](https://www.akkrudata.ai/stocks)
- [13F portfolios by manager](https://www.akkrudata.ai/funds)

---

## Links

- [API and MCP documentation](https://www.akkrudata.ai/documents#11-mcp-access)
- [Full API reference (Markdown)](https://www.akkrudata.ai/FINANCIAL_API_DOCUMENTATION.md)
- [Official MCP Registry entry](https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.akkrudata) — `ai.akkrudata/akkrudata`
- [Glama listing](https://glama.ai/mcp/connectors/ai.akkrudata/akkrudata)
- [Smithery listing](https://smithery.ai/servers/akkrudata/akkrudata)
- [Privacy policy](https://www.akkrudata.ai/privacy) · [Beta terms](https://www.akkrudata.ai/beta-terms)
- Support: support@akkrudata.ai

---

Operated by Akkru Inc. Figures are as reported to the regulator; nothing here is
investment advice.
