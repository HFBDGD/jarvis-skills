# Skill: Market Radar

## What it is
A daily automated pipeline that collects market data, runs AI analysis, and delivers a prose briefing to Telegram at 11:30 AM AEST.

## Pipeline overview
Two workflows run in sequence:
1. **Market Radar AI — Daily Pipeline** (ID: oNkmYqyVH4AXyPIp) — runs at 11:00 AM AEST. Fetches data, writes to Google Sheets.
2. **Market Radar AI — Briefing** (ID: FiqGob5TugrIiQei) — runs at 11:30 AM AEST. Reads sheets, analyses, sends to Telegram.

## Data sources
- **Yahoo Finance**: equities, macro, FX, commodity tickers
- **CoinGecko**: crypto prices and global market stats
- **Alternative.me**: Fear & Greed index

## Tickers tracked
| Ticker | Name | Category |
|---|---|---|
| ^AXJO | ASX 200 | Equities |
| ^HSI | Hang Seng | Equities |
| MS | Morgan Stanley | Equities |
| ^VIX | VIX | Macro |
| GC=F | Gold | Macro |
| ^TNX | US 10Y Yield | Macro |
| CL=F | WTI Crude | Macro |
| AUDUSD=X | AUD/USD | FX |
| USDHKD=X | USD/HKD | FX |
| DX-Y.NYB | DXY | FX |
| TIO=F | Iron Ore | Commodity |
| BTC, ETH, BNB, XRP, SORA, TRUMP | Various | Crypto |

## Storage
Google Sheet ID: `1Paj_t7RxohP3yKxfRAochT-rB-dsYLyU4y61UF_E4-0`
- Sheet `RAW_PRICES`: all price rows, one per ticker per day
- Sheet `SENTIMENT`: fear/greed, BTC dominance, total crypto mcap

## Analysis chain
1. Gemini 2.5 Flash — returns structured JSON: top_movers, macro_theme, crypto_signal, risk_flag, one_liner
2. Claude Sonnet 4.6 — writes a 280-word plain text Telegram briefing from the JSON

## Cost
~$0.19/month Gemini + ~$0.50/month Claude

## Known issues
- Yahoo Finance returns stale or zero data for some tickers on weekends/public holidays
- Google OAuth token requires periodic re-auth (every few months)
- Briefing reads today's data by matching date string — if Daily Pipeline runs late, Briefing may read empty rows

## How to interpret the briefing
- Top movers: assets with largest 24h % change
- Macro theme: the dominant narrative across rates, FX, commodities
- Crypto signal: directional bias based on BTC dominance + Fear & Greed
- Risk flag: the single biggest thing to watch today
