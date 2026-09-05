<p align="center">
  <img src="https://www.kotakneo.com/assets/kotak-logo.svg" alt="Kotak Neo" width="220">
</p>

<h2 align="center">Code your way to the markets 📈</h2>

**[Kotak Neo REST API documentation](docs/)** covers the REST and WebSocket APIs behind the SDK — auth, orders, portfolio, market data, and streaming. Use it directly for non-Python integrations, or to see exactly what the SDK does under the hood.

**[Kotak Neo Python SDK](https://github.com/Kotak-Neo/kotak-neo-python)** is our actively-maintained Python toolkit, with setup guides, a function reference, and migration notes.<br>
The fastest way to authenticate, trade, stream live prices, and manage your portfolio — all from Python.

<p align="center">
  <a href="https://github.com/Kotak-Neo/kotak-neo-python"><img src="https://img.shields.io/badge/Kotak%20Neo%20Python-SDK-0a66c2?style=for-the-badge&logo=github" alt="Kotak Neo Python SDK"></a>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="docs/"><img src="https://img.shields.io/badge/Kotak%20Neo%20REST%20API-Docs-0a66c2?style=for-the-badge&logo=readthedocs&logoColor=white" alt="Kotak Neo REST API Docs"></a>
</p>

The SDK is there to make integration easier, not to gatekeep it — if you're
tech-savvy and comfortable working with programming languages, you can call
the Kotak Neo REST APIs directly using the documentation linked above, in
whichever language you prefer.

Everything you need to build on the Kotak Neo trading platform — place trades,
watch prices tick in real time, track your positions, and build the trading
tools you've always wanted. All from Python.

---

## 🚀 Kotak Neo Python SDK

[![Website](https://img.shields.io/badge/Kotak_Neo-Trade_API-0a66c2)](https://www.kotakneo.com/platform/kotak-neo-trade-api/)
[![PyPI](https://img.shields.io/badge/pypi-kotakneoapi-green.svg)](https://pypi.org/project/kotakneoapi/)
[![Version](https://img.shields.io/badge/version-3.0.5-green.svg)](https://github.com/Kotak-Neo/kotak-neo-python/blob/main/CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/Kotak-Neo/kotak-neo-python/blob/main/LICENSE)

**kotak-neo-python** is our
one true, actively-maintained Python toolkit for the Kotak Neo Trading APIs.
Whether you're automating a trading strategy, building a dashboard, or just
tinkering on a weekend project — this is the one to pick up.

### What you'll get

- 🔐 **Log in in two steps** — TOTP + MPIN, and you're trading
- 📝 **Place, tweak, or cancel orders** without breaking a sweat
- 💼 **Know what you hold** — live positions, holdings, and limits, always up to date
- 📊 **Live prices, live everything** — a real-time market feed built on modern async Python, so your app reacts the instant the market moves
- 📈 **Go deeper on market data** — expiry lists, full option chains, and historical candles, whenever your strategy needs them
- 🔔 **Never miss an order/ position update** — a dedicated feed streams your order and position changes as they happen
- 🛡️ **Fewer surprises** — the SDK catches bad inputs before they ever reach the exchange, and speaks up clearly when something's wrong
- ⚡ **Built for speed** — modern HTTP/2 under the hood
- ✅ **Battle-tested** — thoroughly tested from top to bottom, so you can build with confidence

### 🆕 What's new in v3.0.5

Compared to the previous release (v3.0.1), the SDK added:

- **New market-data functions** — `expiries()`, `option_chain()`, and `historical_data()`
- **Order tagging** — `place_order(tag=...)`, echoed back in `order_report()`/`trade_report()` for tracking
- **Market status / CAS support** on the live feed — session open/close, pre-open, and Call Auction Session updates
- **Jupyter Notebook support** — install guide, compatibility CI, and a smoke-test notebook
- **A sync-integration guide** for using the async feeds from Django/Flask/Celery-style synchronous apps
- **A logging overhaul** — structured JSON logging, one log line per request instead of several, and auto-masked sensitive fields

See the SDK's **[CHANGELOG.md](https://github.com/Kotak-Neo/kotak-neo-python/blob/main/CHANGELOG.md)** for full details.

---

## 📦 About our previous repo

**[kotak-neo-api-v2](https://github.com/Kotak-Neo/kotak-neo-api-v2)** is our
retired, legacy SDK. It's not going anywhere overnight, but it won't get new
features — so start any new project with **[kotak-neo-python](https://github.com/Kotak-Neo/kotak-neo-python)** above instead.

---

## 💬 Need a hand?

- 🐛 **Found a bug, or have an idea?** [Open an issue](https://github.com/Kotak-Neo/kotak-neo-python/issues)
- 📧 **Prefer email?** support@kotakneo.com

---

<sub>Kotak Neo Trading APIs · Built for developers · MIT licensed</sub>
