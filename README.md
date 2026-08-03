<p align="center">
  <img src="https://www.kotakneo.com/assets/kotak-logo.svg" alt="Kotak Neo" width="220">
</p>

<h2 align="center">Official developer resources for the Kotak Neo Trading APIs</h2>

Build trading and market-data applications on the Kotak Securities Neo platform —
place orders, stream live market data, manage positions, and more.

[![Website](https://img.shields.io/badge/Kotak_Neo-Trade_API-0a66c2)](https://www.kotaksecurities.com/platform/kotak-neo-trade-api/)
[![PyPI](https://img.shields.io/badge/pypi-kotakneoapi-green.svg)](https://pypi.org/project/kotakneoapi/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/Kotak-Neo/kotak-neo-python/blob/main/LICENSE)

---

## 🚀 Official Python SDK: kotak-neo-python

**[kotak-neo-python](https://github.com/Kotak-Neo/kotak-neo-python)** is the
official, actively maintained Python SDK for the Kotak Neo Trading APIs.
Install it from PyPI as [`kotakneoapi`](https://pypi.org/project/kotakneoapi/).

```bash
pip install kotakneoapi
```

### Features

- ✅ **Authentication** — TOTP-based secure login with 2FA
- ✅ **Order Management** — Place, modify, cancel orders (Regular/AMO)
- ✅ **Portfolio & Positions** — Real-time holdings, positions, and limits
- ✅ **Market Data** — Live quotes, scrip master, search functionality
- ✅ **SFeed WebSocket Streaming** — Modern async/await live market feed with
  typed messages, enriched with `trading_symbol`
- ✅ **Order & Position Feed** — Dedicated async WebSocket for real-time
  order-lifecycle and position updates (`create_order_feed()`)
- ✅ **HTTP/2 Transport** — REST calls use HTTP/2 (via `httpx`) with automatic
  HTTP/1.1 fallback
- ✅ **Strict Input Validation** — Invalid exchange segments, products, and
  order types are rejected client-side before a network call
- ✅ **Optional Reliability Utilities** — Opt-in rate limiting, plus retry and
  circuit-breaker helpers
- ✅ **Comprehensive Error Handling** — Structured exception hierarchy
  (`AuthenticationError`, `ValidationError`, `RateLimitError`, `OrderError`, …)
- ✅ **Type Safety** — Full mypy type checking support
- ✅ **Extensive Testing** — 100% test coverage (unit, integration, and E2E tests)

### Get started

- 📖 [README & Quick Start](https://github.com/Kotak-Neo/kotak-neo-python#readme)
- 📚 [Full API Documentation](https://github.com/Kotak-Neo/kotak-neo-python/blob/main/docs/functions/README.md)
- 🔀 [Migration Guide & Scanner](https://github.com/Kotak-Neo/kotak-neo-python/blob/main/docs/guides/MIGRATION.md) — moving from `kotak-neo-api-v2`

---

## 📦 Legacy repository

**[kotak-neo-api-v2](https://github.com/Kotak-Neo/kotak-neo-api-v2)** is now a
legacy repository. New projects should use `kotak-neo-python` instead.

---

## 💬 Support

- 🐛 **Bugs & feature requests:** [open an issue](https://github.com/Kotak-Neo/kotak-neo-python/issues)
- 📧 **Email:** support@kotakneo.com

---

<sub>Kotak Neo Trading APIs · Built for developers · MIT licensed</sub>
