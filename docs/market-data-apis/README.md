# Market Data APIs

Everything you need to know what's tradable and what it's doing right now — the
instrument master, live quotes, options/futures data, and a real-time WebSocket feed for
ticks, depth, and indices.

📇 [Instruments](instruments.md) - Direct download links to the latest scrip master
(instrument master) CSV files for every supported exchange segment — tokens, symbols,
and everything else you need for trading and symbol lookups.

💹 [Quotes](quotes.md) - Live and last-traded market data for one or more instruments at
once — depth, OHLC, circuit limits, and more, with filters to pull exactly what you
need.

📅 [Expiries](expiries.md) - The list of available expiry dates for an underlying, ready
to populate a selector or feed straight into the Option Chain API.

⛓️ [Option Chain](option-chain.md) - The full option chain (calls/puts) or futures chain
for an underlying, with per-strike quotes and server-computed open interest.

🕯️ [Historical Data](historical-data.md) - Historical OHLCV candle data for an
instrument, across intraday, daily, and weekly intervals.

📡 [Market Data WebSocket](websocket.md) - A single, language-agnostic WebSocket feed
for live LTP, OHLC, depth, and index data — everything you need to build a real-time
ticker or trading dashboard.
