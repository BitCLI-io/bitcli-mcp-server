# 🤖 BitCLI Model Context Protocol (MCP) Server

[![Model Context Protocol](https://img.shields.io/badge/MCP-Supported-blue?style=for-the-badge)](https://modelcontextprotocol.io/)
[![Glama TDQS Grade](https://img.shields.io/badge/Glama_TDQS-4.6%2F5.0%20(A)-10b981?style=for-the-badge)](https://glama.ai)
[![BitCLI](https://img.shields.io/badge/Powered_by-BitCLI_HFT_Engine-151c2c?style=for-the-badge)](https://bitcli.io)




<div align="center">

# BitCLI Model Context Protocol (MCP) Server

**Institutional-grade crypto derivatives infrastructure, order flow analytics, and HFT triggers for AI Agents.**

[![MCP Protocol](https://img.shields.io/badge/MCP-2024--11--05-blue.svg?style=flat-square)](https://modelcontextprotocol.io/)
[![TDQS Score](https://img.shields.io/badge/Glama%20TDQS-4.8%2F5.0-success.svg?style=flat-square)](https://glama.ai/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=flat-square)](LICENSE)
[![Status: Production](https://img.shields.io/badge/Status-Live%20Production-emerald.svg?style=flat-square)](https://mcp.bitcli.io)

[Website](https://bitcli.io) • [Documentation](https://docs.bitcli.io) • [Web Dashboard](https://my.bitcli.io) • [Telegram Bot](https://t.me/bitclibot)

</div>

---

## 💡 The Economics of AI Trading: Why Raw APIs Bleed Capital

Most AI trading agents connect to crypto exchanges using standard REST endpoints (CCXT, Binance FAPI, or Bybit V5). **This burns massive token context on raw formatting boilerplate.**

A standard 100-candle JSON array from an exchange dumps 13-digit Unix timestamps, quote/base volume strings, and trade counts. The agent's LLM context window gets clogged with unparsed strings, while having to compute RSI, EMA, and MACD inside its own context—burning tokens and hallucinating math.

### The Token Benchmark (100 Candles, 5m Timeframe)

| Data Provider / Format | Payload Strategy | Tokens (100 Bars) | Inference Cost (10k Calls)* | Context Efficiency |
| :--- | :--- | :---: | :---: | :---: |
| **Bybit V5 REST API** | Named JSON Key-Value Objects | **~3,450** | $86.25 | Baseline (Bloated) |
| **CCXT Library (Python)** | Raw 6-element JSON arrays | **~2,680** | $67.00 | Industry Average |
| **BitCLI BCT-30 Matrix** | Kilo-scaled USD + Server-Side Indicators | **~790** | **$19.75** | **🔥 70% Less Tokens** |

> *\*Pricing based on standard OpenAI GPT-4o / Claude 3.5 Sonnet input token pricing ($2.50 / 1M).*

### 💰 The Bottom Line
BitCLI queries cost **$0.001 USD** (Pay-As-You-Go).  
By cutting **~1,900 tokens per call**, BitCLI **saves ~$0.0048 in model inference fees** on *every single request*.  
**You pay micro-cents for curated market data to save full dollars on LLM compute.**

---

## ⚡ The BCT-30 Candlestick Matrix

Instead of raw exchange JSON, BitCLI returns dense, mathematical **BCT-30 (BitCLI Compact Candlesticks)** matrices:

```text
@BCT: BYBIT:BTCUSDT|5m|1719705600|kUSD
@META: OI:$1.54B(+0.05%|+0.21%|-2.14%) | Fund_8h:+0.0100% | RVOL:2.4x | RSI:62.14 | EMA200:64,250.00
@FIELDS: idx o h l c v dv rsi:14 ema:200
-4 64120.5 64180.0 64100.0 64150.0 1250 +420 58.2 64210.0
-3 64150.0 64210.0 64140.0 64195.0 2100 +1150 61.4 64225.0
-2 64195.0 64230.0 64180.0 64210.0 1800 +350 62.1 64238.0
-1 64210.0 64260.0 64190.0 64245.0 3400 +2100 64.8 64248.0
 0 64245.0 64280.0 64220.0 64270.0 1450 +680 66.2 64250.0
```

*   **Zero-Token Temporal Grids:** No 13-digit Unix timestamps per row. Relative integer delta indices (`-N ... 0`) reconstruct exact time deterministically from the `@BCT` base timestamp.
*   **Normalized Metrics:** Volume (`v`) and Cumulative Volume Delta (`dv`) are converted to thousands of USD (`kUSD`).
*   **Zero Math Hallucination:** RSI, EMA, ATR, MACD, and Bollinger Bands are calculated in C++ on the server with 3,000 bars of historical warmup. The agent reads raw institutional truth.

---

## 📡 Dual Integration Architecture

### 1. Stateless Assistants (Cursor, Claude Desktop, ChatGPT)
*   **Endpoint:** `POST https://mcp.bitcli.io/mcp`
*   **Mode:** The agent queries market data or executes risk calculations on prompt.
*   **Asynchronous Alerting:** When setting a trigger via `alert_manager`, the agent does **not** need to hang open or poll. The trigger is injected into the C++ memory core. When hit, BitCLI dispatches an instant push to the user's **Telegram** or **Discord** channel.

### 2. Autonomous Trading Agents (Python SDK)
*   **Endpoint:** `wss://mcp.bitcli.io/stream`
*   **Mode:** Full duplex event stream with automatic reconnection and Replay API. Agents sleep in memory and wake up via sub-millisecond WebSocket frame pushes.

---

## 🛠️ The 6 Core MCP Tools

| Tool | Action | Pricing | Description |
| :--- | :--- | :---: | :--- |
| `system_docs` | `read` | **Free ($0.00)** | Access official BitCLI documentation, command grammar, and indicator formulas via RAG. |
| `market_data` | `read` | **$0.001** | BCT-30 Candlestick matrices, Cross-Exchange Quant TA consensus, and dynamic screening across 1,650+ contracts. |
| `calc_engine` | `compute` | **$0.001** | Mathematical risk models: Lot sizing by dollar risk (`/pos`), True liquidation with MMR (`/liq`), DCA break-even, multi-target R:R. |
| `alert_manager` | `write` | **$0.001** - **$0.002** | Injects sub-500ns triggers into C++ RAM. Dispatches notifications to Telegram / Discord when hit. |
| `send_notification` | `write` | **$0.001** | AI sends direct HTML messages to the human user's Telegram chat/channel or Discord webhook. |
| `user_account` | `read` | **Free ($0.00)** | Inspect USD balance, daily quota usage, and list verified Telegram/Discord destination IDs. |

---

## 🚀 Quickstart: Connecting Your AI Client

### 1. Generate an API Key
Log in to the [Web Dashboard](https://my.bitcli.io) (via Discord, Telegram, or Web3) or use `/profile` in [@bitclibot](https://t.me/bitclibot) to generate your developer key:
```text
bcli_your_api_key_here
```

### 2. Cursor IDE Integration
Add BitCLI to your project's `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "bitcli": {
      "url": "https://mcp.bitcli.io/mcp",
      "headers": {
        "Authorization": "Bearer bcli_your_api_key_here"
      }
    }
  }
}
```

### 3. Claude Desktop Configuration
Add BitCLI to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "bitcli": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.bitcli.io/mcp",
        "--header",
        "Authorization: Bearer bcli_your_api_key_here"
      ]
    }
  }
}
```

---

## ⌨️ Unified Command Syntax (Used in Chat & `alert_manager`)

The exact same syntax works whether typed into Telegram or passed into the MCP `command` parameter:

*   **Exact Price:** `btc 95k` or `binance eth 3150`
*   **Velocity (Rapid Moves):** `sol +3.5% 5m` (Surges >3.5% in 5 min)
*   **Whale Volume Spikes:** `* vol 15m x4` (Global scan: 4x above 20-SMA)
*   **Liquidation Cascades:** `* liq z3` (Liquidations exceed 3 standard deviations)
*   **Order Flow (CVD):** `btc cvd div 15m` (Cumulative Volume Delta Divergence)
*   **Risk Position Sizing:** `/pos btc 65k 63.7k 50` ($50 fixed risk calculation)

---

## 🛡 Security & Transparency

*   **No Exchange API Keys Needed:** BitCLI does not custody funds or request your private exchange trading keys.
*   **Isolated Routing:** The `send_notification` tool strictly enforces ownership validation—agents can only message destination IDs explicitly verified by your account.
*   **Zero Polling Overhead:** Avoid burning tokens on cron-loops. Set a trigger via `alert_manager` and let our infrastructure notify your channels.

## 📄 License
This MCP specification and client wrappers are open-source under the [MIT License](LICENSE).



## 📚 Documentation
For complete syntax guides and API documentation, visit: **[docs.bitcli.io/mcp](https://docs.bitcli.io/mcp)**
