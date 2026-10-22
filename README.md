# 🤖 BitCLI Model Context Protocol (MCP) Server

[![Model Context Protocol](https://img.shields.io/badge/MCP-Supported-blue?style=for-the-badge)](https://modelcontextprotocol.io/)
[![Glama TDQS Grade](https://img.shields.io/badge/Glama_TDQS-4.6%2F5.0%20(A)-10b981?style=for-the-badge)](https://glama.ai)
[![BitCLI](https://img.shields.io/badge/Powered_by-BitCLI_HFT_Engine-151c2c?style=for-the-badge)](https://bitcli.io)

The official **Streamable HTTP & SSE MCP server** for [BitCLI](https://bitcli.io). 

Connect your autonomous AI trading agents, **Cursor**, or **Claude Desktop** to sub-millisecond crypto order flow, liquidation cascades, and CVD anomalies. 

> 🛑 **Stop burning millions of LLM tokens on raw OHLCV data.**  
> Our C++ engine computes complex micro-structural metrics (RVOL, Cumulative Volume Delta, Open Interest, Normalized Funding Rates) in RAM `<500ns` and delivers **token-optimized, ultra-dense JSON summaries** directly into your agent's context window.

The official Model Context Protocol (MCP) gateway for **BitCLI** — an ultra-fast High-Frequency Trading (HFT) crypto alerts and order flow analytics engine.

Connect your autonomous AI agents (Cursor, Claude Desktop, AutoGPT, LangChain) directly to live exchange order flow across Binance Futures, Bybit Linear, and Hyperliquid.

## 🚀 Features

- **Microsecond Order Flow (CVD & RVOL):** Access pre-calculated Cumulative Volume Delta and Relative Volume metrics computed in RAM via our C++20 core.
- **Global Market Scanners:** Scan 1,650+ perpetual contracts simultaneously for whale activity, liquidation cascades, and extreme funding rates.
- **Event-Driven HFT Alerts:** Register tick-by-tick triggers (`btc 95k`, `* vol 15m x4`). The server will push asynchronous SSE events to wake up your agent the millisecond the condition is met.
- **M2M x402 Micro-payments:** Fully autonomous Pay-Per-Request economy. Agents can fund their own sessions using USDC on the Base network.

## 📉 The BCT Data Standard (Token Optimization)

Traditional REST APIs (like Binance FAPI or CCXT) were designed for machines parsing JSON, not for Large Language Models (LLMs) paying per token. 

BitCLI introduces the **BCT (BitCLI Compact Candlesticks)** specification to deliver maximum trading intelligence (OHLCV, Order Flow, Meta Indicators) using the absolute minimum number of BPE tokens.

### Benchmark: BCT vs The Industry
*Fetching 100 live 5m candles for BTCUSDT (Measured via OpenAI `o200k_base` tokenizer)*

| Data Source | Payload Architecture | Tokens (100 bars) | Cost (10k reqs) | vs CCXT Baseline |
| :--- | :--- | :---: | :---: | :--- |
| **CCXT Library (Python)** | OHLC + BaseVol (6 fields per row) | 3,624 | $90.60 | *Industry Baseline* |
| **BitCLI MCP (Strict Match)** | OHLC + USDVol (5 fields + Relative TS) | **2,605** | **$65.12** | **🔥 28.1% less tokens** |
| **BitCLI MCP (Full Power)** | **OHLC + USDVol + CVD Delta + Meta** | **2,841** | **$71.03** | **🔥 21.6% less tokens** |
| **Bybit V5 REST API** | Named JSON Objects `{"open": ...}` | 6,406 | $160.15 | ⚠️ +76.8% token bloat |
| **Binance FAPI REST** | Raw 12-element arrays | 7,128 | $178.20 | ⚠️ +96.7% token bloat |

**How BCT achieves this:**
1. **Zero-Token Temporal Grids:** We strip absolute 13-digit Unix timestamps. The agent calculates time deterministically using a relative index (`-N+1 ... 0`) and a `@BCT` header base time.
2. **Kilo-Scaled Volumes:** Volumes and Deltas are converted to Quote Currency (USD) and scaled to thousands (`kUSD`), eliminating decimals and leading zeros.
3. **Server-Side Math:** Indicators (RSI, EMA, MACD, ATR, BB) are computed on our servers using a 3,000-bar warmup padding to guarantee 100% TradingView parity, saving your agent from writing Python scripts and doing internal math.

## 🔌 Connection Endpoints

BitCLI supports two transport modes. Configure your MCP client accordingly:

1. **Server-Sent Events (SSE)** — *Recommended for Trading Agents*
   ```json
   {
     "mcpServers": {
       "bitcli": {
         "url": "https://mcp.bitcli.io/mcp/sse"
       }
     }
   }
   ```
   *Required to receive asynchronous push notifications from `create_alert`.*

2. **Streamable HTTP (Stateless)**
   ```json
   {
     "mcpServers": {
       "bitcli": {
         "url": "https://mcp.bitcli.io/mcp"
       }
     }
   }
   ```
   *Best for lightweight clients doing read-only market snapshots.*

## 🛠 Available Tools

* `get_market_snapshot`: Find top outliers across all exchanges (RVOL, Funding, nATR).
* `get_bct_candles`: Fetch token-optimized historical OHLCV, CVD delta, and server-side calculated indicators.
* `get_ta_consensus`: Deep-dive technical consensus for a single asset (RSI, EMA bias, CVD slope).
* `create_alert`: Set an HFT condition in the C++ engine (e.g. `sol +3% 5m`, `eth cvd div 15m`).
* `list_alerts`: List active triggers for your session.
* `cancel_alert`: Remove an active trigger.
* `get_account_usage`: Check your current M2M balance and daily quota.
* `verify_payment`: Submit a Base L2 USDC transaction hash to top up your agent's balance via x402.

## 💸 Pricing & x402 Economy

All connections start on a **Free Tier**:
- Anonymous (Session/IP): 50 requests/day.
- Registered API Key: 100 requests/day.

When limits are reached, or when attempting to use premium tools (like global `*` wildcard scans), the server returns an `HTTP 402 Payment Required` payload. 

Your agent can automatically resolve this by sending a minimum of **0.10 USDC** on the Base network to the provided wallet and calling the `verify_payment` tool. The deposit is instantly credited to the session/token balance ($0.001 per standard query).

## 📚 Documentation
For complete syntax guides and API documentation, visit: **[docs.bitcli.io/mcp](https://docs.bitcli.io/mcp)**
