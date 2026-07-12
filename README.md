<table border="0" cellborder="0">
  <tr border="0">
    <td border="0" valign="middle"><img src="https://www.kotakneo.com/assets/kotak-logo.svg" alt="Kotak Neo" width="200"></td>
    <td border="0" valign="middle"><h2>Official developer resources for the Kotak Neo Trading APIs</h2></td>
  </tr>
</table>

Build trading and market-data applications on the Kotak Securities Neo platform —
place orders, stream live market data, manage positions, and more.

[![Website](https://img.shields.io/badge/Kotak_Neo-Trade_API-0a66c2)](https://www.kotaksecurities.com/platform/kotak-neo-trade-api/)
[![PyPI](https://img.shields.io/badge/pypi-kotakneoapi-green.svg)](https://pypi.org/project/kotakneoapi/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/Kotak-Neo/kotak-neo-api-v2/blob/main/LICENSE)

---

## 🚀 Official Python SDK

**[kotak-neo-api-v2](https://github.com/Kotak-Neo/kotak-neo-api-v2)** — the official,
modern Python SDK for the Kotak Neo Trading APIs.

```bash
pip install kotakneoapi
```

```python
from neo_api_client import NeoAPI

client = NeoAPI(consumer_key="your-consumer-key", environment="prod")
client.totp_login(mobile_number="+919876543210", ucc="YOUR_UCC", totp="123456")
client.totp_validate(mpin="1234")

# Place an order
client.place_order(
    exchange_segment="nse_cm", product="CNC", price="1500",
    order_type="L", quantity="1", validity="DAY",
    trading_symbol="RELIANCE-EQ", transaction_type="B",
)
```

### Highlights
- 🔐 **Secure auth** — TOTP-based login with 2FA
- 📈 **Full trading** — place / modify / cancel orders, positions, holdings, limits, margins
- ⚡ **Async WebSocket streaming** — live market data (LTP, depth, indices) with typed messages
- 🧾 **Order & position feed** — real-time order-lifecycle and position updates
- 🌐 **HTTP/2 transport** with automatic HTTP/1.1 fallback
- 🐍 **Python 3.10 – 3.14**, fully type-checked and tested

---

## 📚 Documentation

| Resource | Link |
|----------|------|
| 📖 API reference | [docs/functions](https://github.com/Kotak-Neo/kotak-neo-api-v2/blob/main/docs/functions/README.md) |
| 🔌 WebSocket guide | [docs/guides/websocket.md](https://github.com/Kotak-Neo/kotak-neo-api-v2/blob/main/docs/guides/websocket.md) |
| ⬆️ Migration guide | [docs/guides/MIGRATION.md](https://github.com/Kotak-Neo/kotak-neo-api-v2/blob/main/docs/guides/MIGRATION.md) |
| 🛠️ Installation | [docs/installation](https://github.com/Kotak-Neo/kotak-neo-api-v2/blob/main/docs/installation/README.md) |
| 🌐 Trade API portal | [kotaksecurities.com](https://www.kotaksecurities.com/platform/kotak-neo-trade-api/) |

**Upgrading from an older version?** See the
[Migration Guide](https://github.com/Kotak-Neo/kotak-neo-api-v2/blob/main/docs/guides/MIGRATION.md).

---

## 💬 Support

- 🐛 **Bugs & feature requests:** [open an issue](https://github.com/Kotak-Neo/kotak-neo-api-v2/issues)
- 📧 **Email:** support@kotakneo.com

---

## 🤝 Getting started

1. **Get your Consumer Key** — Kotak Neo app/web → **Invest** → **Trade API** → generate application.
2. **Register for TOTP** — [Kotak Neo Trade API portal](https://www.kotaksecurities.com/platform/kotak-neo-trade-api/) → scan the QR with an authenticator app.
3. `pip install kotakneoapi` and follow the [Quick Start](https://github.com/Kotak-Neo/kotak-neo-api-v2#quick-start).

---

<sub>Kotak Neo Trading APIs · Built for developers · MIT licensed</sub>
