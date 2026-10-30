---
name: bitcli
description: Real-time crypto futures order flow, BCT-30 matrix candles, and HFT alerts across Binance, Bybit, and Hyperliquid.
metadata:
  author: BitCLI
  icon: https://bitcli.io/logo.png
---

# BitCLI Skill

Use this skill when the user asks about crypto perpetual futures, order flow, funding rates, CVD divergence, liquidation cascades, or risk calculations.

## Capabilities
- Fetch token-optimized BCT-30 candlestick matrices via MCP endpoint `https://mcp.bitcli.io/mcp`
- Run dynamic market screening across 1,650+ contracts
- Compute true MMR liquidations and position sizes (`/pos`, `/liq`, `/dca`, `/rr`)
- Set sub-500ns HFT execution triggers routed to Telegram/Discord

## Instructions
1. For market scans or indicators, invoke the `market_data` tool on `https://mcp.bitcli.io/mcp`.
2. For position sizing or risk math, invoke the `calc_engine` tool.
3. For asynchronous price/volume/liquidation alerts, invoke `alert_manager`.
