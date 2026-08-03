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

## 🚀 Official Python SDK (COMING SOON)

**kotak-neo-python** — will be the official,
actively maintained Python SDK for the Kotak Neo Trading APIs.

### What's new vs. `kotak-neo-api-v2`

A ground-up rebuild focused on reliability, safety, and a modern developer
experience — not just a feature port.

| | `kotak-neo-api-v2` (current) | `kotak-neo-python` (new) |
|---|---|---|
| **WebSocket** | Callback-based (`on_message`, `subscribe(...)`) | **Async/await** — `async for message in ws`, typed Pydantic messages, auto-reconnect with re-subscription |
| **HTTP transport** | `requests` (HTTP/1.1 only) | **`httpx` with HTTP/2** + automatic HTTP/1.1 fallback |
| **Input validation** | Aliases/typos silently accepted or passed through (e.g. `"NSE"`, `"Limit"`) | **Strict, exact-match validation** client-side — invalid segments/products/order-types rejected before a network call |
| **Order types supported** | Bracket (`BO`) & Cover (`CO`) orders, `GTC`/`EOS`/`GTD` validity — no longer supported by the exchange but still silently accepted | Only exchange-supported values accepted: `CNC`/`NRML`/`MIS`/`MTF` products, `DAY`/`IOC` validity |
| **Type safety** | None — plain dicts everywhere | **Full mypy coverage**, typed method signatures, typed WebSocket message models |
| **Error handling** | Bare exceptions, inconsistent shapes | Structured exception hierarchy (`AuthenticationError`, `ValidationError`, `RateLimitError`, `OrderError`, …) |
| **Reliability** | None built in | **Opt-in** rate limiting, retry with backoff, and circuit-breaker utilities |
| **Testing** | Minimal/none | **100% test coverage** — unit, integration, and end-to-end |
| **`trading_symbol` on ticks** | Not available | Every WebSocket message enriched with its human-readable `trading_symbol` |
| **Order/position updates** | Not available as a dedicated stream | Separate **Order & Position Feed** WebSocket (`create_order_feed()`) |

---

## 💬 Support

- 🐛 **Bugs & feature requests:** [open an issue](https://github.com/Kotak-Neo/kotak-neo-api-v2/issues)
- 📧 **Email:** support@kotakneo.com

---

<sub>Kotak Neo Trading APIs · Built for developers · MIT licensed</sub>
