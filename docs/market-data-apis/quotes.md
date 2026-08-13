# Quotes

## 1. Introduction

The **Quotes API** retrieves live and last traded market data for one or more instruments (including stocks, ETFs, and indices) from supported exchanges. It allows the use of advanced filters to fetch specific, detailed market values including depth, OHLC, circuit limits, and more.

## 2. API Endpoint

```jsx
GET <Base URL>/script-details/1.0/quotes/neosymbol/<query>[,<query>][/<filter_name>]
```

**Replace `<Base URL>` with the relevant Kotak environment base URL provided in response from the `/tradeApiValidate` API.**

**Key points about endpoint structure:**

- `<query>` is formatted as `<exchange_segment>|<instrument>`.
- Multiple queries are separated by commas, e.g. `nse_cm|Nifty 50,bse_cm|SENSEX`.
- Instrument (except indices): Use `pSymbol` from the instrument/scrip master file.
- Indices: Use the **exact case-sensitive name** (e.g. `Nifty 50`, `BANKEX`). Refer the whole list at the bottom.
- **Expected values for `exchange_segment` are (string):**
    - `nse_cm` (NSE Cash)
    - `bse_cm` (BSE Cash)
    - `nse_fo` (NSE F&O)
    - `bse_fo` (BSE F&O)
    - `cde_fo` (CDS F&O)

## 3. Headers

| Name | Type | Description |
| --- | --- | --- |
| Authorization | string | Token provided in your NEO API dashboard - use plain token |
| Content-Type | string | `application/json` |

## 4. Request

**Example Request:**

```jsx
curl --location --request GET '<Base URL>/script-details/1.0/quotes/neosymbol/nse_cm|Nifty 50,nse_cm|Nifty Bank/all' \
--header 'Content-Type: application/json' \
--header 'Authorization: xxxxx-your-api-token-xxxx'
```

## Filter Options

After all queries, you may append a filter with `/filter_name`.

**Allowed filter values (total 8 including default 'all'):**

- `all` (default, returns all fields)
- `52W` (52-week high/low)
- `scrip_details` (scrip basics)
- `circuit_limits` (circuit limits)
- `ohlc` (Open, High, Low, Close)
- `oi` (open interest, if applicable)
- `depth` (order book, top 5 each side)
- `ltp` (last traded price)

## 5. Response

## Example Success Response

```jsx
[
    {
        "exchange_token": "SENSEX",
        "display_symbol": "SENSEX-IN",
        "exchange": "bse_cm",
        "lstup_time": "1757915078",
        "ltp": "81809.3400",
        "last_traded_quantity": "0",
        "total_buy": "0",
        "total_sell": "0",
        "last_volume": "0",
        "change": "-95.3600",
        "per_change": "-0.1200",
        "year_high": "0",
        "year_low": "0",
        "ohlc": {
            "open": "81925.5100",
            "high": "81998.5100",
            "low": "81779.8200",
            "close": "81904.7000"
        },
        "depth": {
            "buy": [
                {"price": "0", "quantity": "0", "orders": "0"},
                ...
            ],
            "sell": [
                {"price": "0", "quantity": "0", "orders": "0"},
                ...
            ]
        }
    }
]
```

## Response Field Mapping

| Field | Type | Description |
| --- | --- | --- |
| exchange_token | string | Instrument token or index name |
| display_symbol | string | UI display symbol |
| exchange | string | Exchange segment (e.g. nse_cm, bse_cm, ...) |
| lstup_time | string | Last update time (Unix timestamp) |
| ltp | string | Last traded price |
| last_traded_quantity | string | Last traded quantity |
| total_buy | string | Top bid quantity |
| total_sell | string | Top offer quantity |
| last_volume | string | Most recent trade volume |
| change | string | Net price change from previous close |
| per_change | string | Percent price change |
| year_high | string | 52-week high |
| year_low | string | 52-week low |
| ohlc | object | Object: open, high, low, close prices |
| depth | object | Top 5 bid/ask levels (arrays ‘buy’ & ‘sell’) |

**OHLC Object**

| Field | Type | Description |
| --- | --- | --- |
| open | string | Day’s open price |
| high | string | Day’s high price |
| low | string | Day’s low price |
| close | string | Previous close price |

**Depth Object**

| Field | Type | Description |
| --- | --- | --- |
| price | string | Price level |
| quantity | string | Quantity at level |
| orders | string | Order count at level |

## Example Error Response

```jsx
{
  "stat": "Not_Ok",
  "emsg": "Invalid instrument/code",
  "stCode": 1009
}
```

| Field | Type | Description |
| --- | --- | --- |
| stat | string | "Not_Ok" for errors |
| emsg | string | Error message |
| stCode | int | Error code |

## 6. Notes

- All fields are returned as strings.
- When using indices, pass the correct case-sensitive index name.
- For stocks/ETFs, use `pSymbol` from instrument/scrip master.
- Multi-instrument queries are comma-separated.
- Valid exchange segments are: **nse_cm, bse_cm, nse_fo, bse_fo, cde_fo** (must be passed as string).
- By default (`/all` or blank) returns all quote data; filters allow more targeted queries.

## 7. Glossary: Index search values

| **exchange** | **instrument query name** |
| --- | --- |
| nse_cm | NIFTY AlphaLowVol |
| nse_cm | Nifty Commodities |
| nse_cm | Nifty Consumption |
| nse_cm | Nifty Div Opps 50 |
| nse_cm | Nifty Energy |
| nse_cm | Nifty Infra |
| nse_cm | Nifty Media |
| nse_cm | Nifty Metal |
| nse_cm | Nifty MNC |
| nse_cm | Nifty Serv Sector |
| nse_cm | NIFTY SMLCAP 100 |
| nse_cm | Nifty100 Liq 15 |
| nse_cm | NIFTY LARGEMID250 |
| nse_cm | NIFTY MIDSML 400 |
| nse_cm | Nifty FinSrv25 50 |
| nse_cm | NIFTY100 EQL Wgt |
| nse_cm | NIFTY100 LowVol30 |
| nse_cm | NIFTY500 MULTICAP |
| nse_cm | NIFTY Alpha 50 |
| nse_cm | NIFTY CONSR DURBL |
| nse_cm | NIFTY HEALTHCARE |
| nse_cm | Nifty GrowSect 15 |
| nse_cm | Nifty200Momentm30 |
| nse_cm | Nifty Mid Liq 15 |
| nse_cm | Nifty Pvt Bank |
| nse_cm | NIFTY OIL AND GAS |
| nse_cm | Nifty 100 |
| nse_cm | Nifty 200 |
| nse_cm | Nifty Auto |
| nse_cm | Nifty FMCG |
| nse_cm | NIFTY MIDCAP 100 |
| nse_cm | Nifty Next 50 |
| nse_cm | Nifty Pharma |
| nse_cm | Nifty PSU Bank |
| nse_cm | NIFTY100 Qualty30 |
| nse_cm | NIFTY MIDCAP 150 |
| nse_cm | NIFTY200 QUALTY30 |
| nse_cm | NIFTY SMLCAP 250 |
| nse_cm | NIFTY SMLCAP 50 |
| nse_cm | Nifty Realty |
| nse_cm | Nifty 500 |
| nse_cm | Nifty 50 |
| nse_cm | Nifty IT |
| nse_cm | Nifty Bank |
| nse_cm | Nifty Midcap 50 |
| nse_cm | INDIA VIX |
| nse_cm | Nifty PSE |
| nse_cm | Nifty Fin Service |
| nse_cm | Nifty CPSE |
| nse_cm | NIFTY MID SELECT |
| nse_cm | NIFTY MICROCAP250 |
| bse_cm | SNSX50 |
| bse_cm | SENSEX |
| bse_cm | BANKEX |
| bse_cm | BSE100 |
| bse_cm | BSE200 |
| bse_cm | BSE500 |
| bse_cm | BSE CG |
| bse_cm | BSE CD |
| bse_cm | BSEPSU |
| bse_cm | TECK |
| bse_cm | AUTO |
| bse_cm | OILGAS |
| bse_cm | DOL30 |
| bse_cm | DOL100 |
| bse_cm | DOL200 |
| bse_cm | REALTY |
| bse_cm | POWER |
| bse_cm | BSEIPO |
| bse_cm | SMEIPO |
| bse_cm | INFRA |
| bse_cm | CPSE |
| bse_cm | MIDCAP |
| bse_cm | SMLCAP |
| bse_cm | BSEFMC |
| bse_cm | BSE HC |
| bse_cm | BSE IT |
| bse_cm | MFG |
| bse_cm | ALLCAP |
| bse_cm | COMDTY |
| bse_cm | CONDIS |
| bse_cm | ENERGY |
| bse_cm | FINSER |
| bse_cm | INDSTR |
| bse_cm | LRGCAP |
| bse_cm | MIDSEL |
| bse_cm | SMLSEL |
| bse_cm | TELCOM |
| bse_cm | UTILS |
| bse_cm | SNXT50 |
| bse_cm | BHRT22 |
| bse_cm | ESG100 |
| bse_cm | MID150 |
| bse_cm | SML250 |
| bse_cm | LMI250 |
| bse_cm | MSL400 |
| bse_cm | BSEDSI |
| bse_cm | BSEEVI |
| bse_cm | BSELVI |
| bse_cm | BSEMOI |
| bse_cm | BSEQUI |
| bse_cm | DFRGRI |
| bse_cm | LCTMCI |
| bse_cm | BSEPBI |
| bse_cm | BSESER |
| bse_cm | SNXN30 |
| bse_cm | SNSX60 |
| bse_cm | SS6535 |
| bse_cm | POWENE |
| bse_cm | 200EQW |
| bse_cm | INTECO |
| bse_cm | CAPINS |
| bse_cm | FOCIT |
| bse_cm | PRECON |
| bse_cm | FOCMID |
| bse_cm | BBGEFS |
| bse_cm | SENEQW |
| bse_cm | SELIPO |
| bse_cm | PSUBNK |
| bse_cm | INSLDR |
| bse_cm | BS1000 |
| bse_cm | NXT500 |
| bse_cm | BSM250 |
| bse_cm | NXT250 |
| bse_cm | 1000EQ |
| bse_cm | IND150 |
| bse_cm | BSE5S |
| bse_cm | BN5TIP |
| bse_cm | BSLMIP |
| bse_cm | BSHOIP |
| bse_cm | BS5TIP |
| bse_cm | BDISB |
| bse_cm | BSEBIP |
| bse_cm | BSREIT |
| bse_cm | BSMSIP |