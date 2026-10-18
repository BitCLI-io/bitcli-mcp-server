# 🤖 BitCLI Model Context Protocol (MCP) Server

[![Model Context Protocol](https://img.shields.io/badge/MCP-Supported-blue?style=for-the-badge)](https://modelcontextprotocol.io/)
[![Status](https://img.shields.io/badge/Status-Public_Alpha-10b981?style=for-the-badge)](#)
[![BitCLI](https://img.shields.io/badge/Powered_by-BitCLI_HFT_Engine-151c2c?style=for-the-badge)](https://bitcli.io)

The official **SSE-based MCP server** for [BitCLI](https://bitcli.io). 

Connect your autonomous AI trading agents, **Cursor**, or **Claude Desktop** to sub-millisecond crypto order flow, liquidation cascades, and CVD anomalies. 

> 🛑 **Stop burning millions of LLM tokens on raw OHLCV data.**  
> Our C++ engine computes complex micro-structural metrics (RVOL, Cumulative Volume Delta, Open Interest, Normalized Funding Rates) in RAM `<500ns` and delivers **token-optimized, ultra-dense JSON summaries** directly into your agent's context window.

## 🚀 Available Tools

- `get_market_snapshot`: Instantly scan 1,650+ perpetual futures across Binance, Bybit, and Hyperliquid. Get the Top N coins sorted by `rvol_5m` (Whale spikes), `funding_rate` (Squeezes), or `natr_1h` (Volatility).
- `get_ta_consensus`: Get aggregated Trend Bias, RSI, CVD Slope, and ATR for a specific coin without downloading a single candlestick.
- `create_alert` *(Beta)*: Register an in-memory high-frequency trigger. The server will push an asynchronous SSE event to wake up your agent when the condition is met (eliminates API polling).

## ⚡ Quickstart: Cursor IDE

BitCLI MCP is hosted on the cloud. You do not need to run anything locally.

1. Open **Cursor Settings** (gear icon) -> **Features** -> **MCP**.
2. Click **+ Add New MCP Server**.
3. Configure it as follows:
   - **Name:** `BitCLI`
   - **Type:** `sse`
   - **URL:** `https://mcp.bitcli.io/mcp/sse`
4. Click **Save**. Cursor will instantly fetch the tools.

Now, open Cursor Chat and type:
> *"Analyze the crypto market using BitCLI. Find the top 3 coins with the highest RVOL and tell me if their CVD is bullish or bearish."*

## ⚡ Quickstart: Claude Desktop

Add the following to your `claude_desktop_config.json` (located at `~/Library/Application Support/Claude/` on macOS or `%APPDATA%\Claude\` on Windows):

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
