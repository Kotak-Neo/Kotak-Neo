# Kotak Neo Trade REST API Documentation

Reference documentation for the Kotak Neo Trade REST APIs — authentication, orders,
portfolio, market data, and both WebSocket feeds.

These APIs can be called directly in any language if you're comfortable doing your
own integration work (auth, request signing, error handling, etc.) — this
documentation is all you need for that. Prefer not to build that integration
yourself? The **[kotak-neo-python](https://github.com/Kotak-Neo/kotak-neo-python)**
SDK wraps all of this into a typed, tested Python client (`pip install kotakneoapi`).

## 📚 Quick Navigation

🚀 [Getting Started (15 minutes)](getting-started.md) - Start here if you're new

🔐 [Authentication Reference](authentication.md) - Detailed authentication info

🌐 [Static IP Whitelisting](static-ip-whitelisting.md) - For retail algo trading

📈 [Market Data](market-data-apis.md) - Quotes and instrument master

📊 [Trading Operations](trading-apis.md) - Place, modify, cancel orders

💼 [Portfolio & Funds](portfolio-and-funds-apis.md) - Holdings, positions, limits, margin

📄 [Reports](order-report-apis.md) - Order history, orderbook, tradebook

🔌 [NEO Websocket](neo-websocket.md) - Live market feed (HSM) over WebSocket

📡 [Order & Position Streaming API (WebSocket)](order-position-streaming-api.md) - Live order/position updates

🐛 [Troubleshooting](troubleshooting.md) - Check here when stuck, common error codes

📖 [Complete API Reference](api-reference.md) - Downloadable reference and curl examples
