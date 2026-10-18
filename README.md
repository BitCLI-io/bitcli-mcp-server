# 🤖 BitCLI Model Context Protocol (MCP) Server

[![Model Context Protocol](https://img.shields.io/badge/MCP-Supported-blue?style=for-the-badge)](https://modelcontextprotocol.io/)
[![Glama TDQS Grade](https://img.shields.io/badge/Glama_TDQS-4.6%2F5.0%20(A)-10b981?style=for-the-badge)](https://glama.ai)
[![BitCLI](https://img.shields.io/badge/Powered_by-BitCLI_HFT_Engine-151c2c?style=for-the-badge)](https://bitcli.io)

The official **Streamable HTTP & SSE MCP server** for [BitCLI](https://bitcli.io). 

Connect your autonomous AI trading agents, **Cursor**, or **Claude Desktop** to sub-millisecond crypto order flow, liquidation cascades, and CVD anomalies. 

> 🛑 **Stop burning millions of LLM tokens on raw OHLCV data.**  
> Our C++ engine computes complex micro-structural metrics (RVOL, Cumulative Volume Delta, Open Interest, Normalized Funding Rates) in RAM `<500ns` and delivers **token-optimized, ultra-dense JSON summaries** directly into your agent's context window.

## 🚀 Available Tools

- `get_market_snapshot`: Scan 1,650+ perpetual futures across Binance, Bybit, and Hyperliquid. Get Top N coins sorted by `rvol_5m` (Whale spikes), `funding_rate` (Squeezes), or `oi_change_1h`.
- `get_ta_consensus`: Retrieve cross-exchange aggregated Trend Bias (EMA20), RSI14, 1h ATR volatility, and CVD order flow for a target asset.
- `create_alert`: Register an in-memory high-frequency alert using unified BitCLI syntax (`btc 95k`, `* vol 15m x4`, `* liq z3`). Pushes an asynchronous event via SSE when triggered.
- `list_alerts`: Audit all active in-memory triggers owned by the agent with remaining TTL.
- `cancel_alert`: Unregisters and purges an active trigger from the C++ HFT engine memory.

## 🌐 Quickstart: Cursor IDE & Modern Clients

BitCLI MCP is cloud-hosted. No local installation required.

- **Endpoint URL:** `https://mcp.bitcli.io/mcp`

### In Cursor:
1. Open **Cursor Settings** -> **Features** -> **MCP**.
2. Click **+ Add New MCP Server**.
3. Set Type to `sse` or `streamable` and use URL: `https://mcp.bitcli.io/mcp`

## 📦 Quickstart: Claude Desktop (Local Bridge)

Add the following to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "bitcli": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/client-sse",
        "--url",
        "https://mcp.bitcli.io/mcp/sse"
      ]
    }
  }
}
```

## 💸 Pricing & L402

Currently in **Free Public Alpha**.  
In Q4, BitCLI MCP will transition to a Machine-to-Machine (M2M) micro-monetization model using the **HTTP 402 Payment Required** (x402 protocol via USDC on Base). Your agents will be able to self-fund their API calls for fractions of a cent per request.

## 🌐 Links
- **Telegram Alert Terminal:** [@bitclibot](https://t.me/bitclibot)
- **Documentation:** [docs.bitcli.io](https://docs.bitcli.io)
- **Contact:** [team@bitcli.io](mailto:team@bitcli.io)
