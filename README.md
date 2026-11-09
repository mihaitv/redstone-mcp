# Redstone Markets MCP server

Performance, risk, comparisons, backtests and screening for US stocks and ETFs, with charts.

[Redstone Markets](https://redstonemarkets.com) has free return and risk analytics for about 2,900 US-listed stocks and 1,200 US ETFs. Its MCP server gives AI assistants the same figures: a ticker's returns and risk, side-by-side comparisons, portfolio backtests, screens, and the portfolios people have shared on Redstone.

The server runs at redstonemarkets.com, so there is nothing to install or host. This repository holds its docs, its connection config and the Redstone agent skill.

| | |
| --- | --- |
| Endpoint | `https://redstonemarkets.com/api/mcp` |
| Transport | Streamable HTTP |
| Access | Public and read-only, with no account or API key |
| Registry name | `com.redstonemarkets/redstone` |
| Server card | https://redstonemarkets.com/.well-known/mcp/server-card.json |
| Docs | https://redstonemarkets.com/about#ai-assistants |
| Logo | https://redstonemarkets.com/web-app-manifest-512x512.png |

## Add the server

### Claude

Connect [Redstone from Claude's connector directory](https://claude.ai/directory/redstone), then turn it on in a chat under **+ → Connectors**.

In Claude Code:

```sh
claude mcp add --transport http redstone https://redstonemarkets.com/api/mcp
```

### ChatGPT

Connect [Redstone from ChatGPT's plugin directory](https://chatgpt.com/plugins/plugin_asdk_app_6ad2bd32b6308191a4282552c1e7b098).

### Cursor

Add this to `~/.cursor/mcp.json` for every project, or `.cursor/mcp.json` for one:

```json
{
  "mcpServers": {
    "redstone": {
      "url": "https://redstonemarkets.com/api/mcp"
    }
  }
}
```

This repository is also a Cursor plugin (`.cursor-plugin/plugin.json`), which adds the server and the skill together.

### Other MCP clients

Add `https://redstonemarkets.com/api/mcp` as a remote MCP server over Streamable HTTP, with no sign-in. VS Code, for example, reads this from `.vscode/mcp.json`:

```json
{
  "servers": {
    "redstone": {
      "type": "http",
      "url": "https://redstonemarkets.com/api/mcp"
    }
  }
}
```

A client that only runs local (stdio) servers can reach it through [`mcp-remote`](https://www.npmjs.com/package/mcp-remote): `npx mcp-remote https://redstonemarkets.com/api/mcp`.

## Tools

| Tool | What it does |
| --- | --- |
| `get_security_metrics` | One stock or ETF: YTD, 1-year and 5-year annualized returns, max drawdown, volatility, Sharpe, Sortino, beta, alpha and dividend yield, with a 1-year and 5-year chart. |
| `compare_securities` | 2 to 5 stocks or ETFs side by side: returns, drawdown, volatility, Sharpe, beta, dividend yield and how correlated they are, on one chart. |
| `backtest_portfolio` | A weighted portfolio of 2 to 20 stocks and ETFs, rebalanced quarterly: returns over YTD, 1 and 5 years, max drawdown, volatility, Sharpe, beta, dividend yield and the correlation between holdings, charted against SPY, with a link that opens it on Redstone. |
| `screen_securities` | Stocks or ETFs ranked by a return, risk or dividend yield metric, optionally within a sector or ETF category and within bounds on other metrics. |
| `search_portfolios` | The portfolios Redstone's users built and shared, and Redstone's model portfolios, found by what they hold, their name, or their returns, drawdown, Sharpe and yield. |
| `get_portfolio` | One shared or model portfolio: its holdings with each one's figures, and its returns and risk, charted against SPY. |

Each answer gives the market date it's based on and links to the matching page on redstonemarkets.com. In clients that show MCP Apps, such as ChatGPT and Claude, answers about a ticker, a comparison or a portfolio come with a chart.

## Try it

- "Compare VOO and QQQ over the last 5 years"
- "Backtest 60% VTI and 40% BND"

## Agent skill

[`skills/redstone-markets/SKILL.md`](skills/redstone-markets/SKILL.md) tells an agent how to use the server and Redstone's pages. It's a copy of the live skill at https://redstonemarkets.com/.well-known/agent-skills/redstone-markets/SKILL.md.

Install it with [skills](https://skills.sh):

```sh
npx skills add mihaitv/redstone-mcp
```

## Data

The data is end-of-day, not real-time. Returns are total returns, split-adjusted with dividends reinvested on the ex-date, and risk figures cover the past year of daily closes. The [About page](https://redstonemarkets.com/about) explains how each figure is calculated.

Nothing the server returns is investment advice, and past performance doesn't predict future results.

## Contact

support@redstonemarkets.com

## License

[MIT](LICENSE)
