# Option Chain

## 1. Introduction

The **Option Chain API** returns the option chain (calls / puts) or the futures chain for an underlying, with per-strike quote and open-interest data. Open-interest change and change % are computed server-side so every caller gets a consistent value.

## 2. API Endpoint

```
GET <Base URL>/market-data/1.0/watchlist/option-chain?exchange=<exchange_segment>&underlying=<underlying>[&expiry=<YYYY-MM-DD>][&instrument_type=<type>][&count=<n>]
```

**Replace `<Base URL>` with the relevant Kotak environment base URL provided in response from the `/tradeApiValidate` API.**

**Key points about endpoint structure:**

- `exchange` and `underlying` are mandatory; `expiry`, `instrument_type` and `count` are optional.
- `underlying`: use `pSymbolName` from the instrument/scrip master file (e.g. `NIFTY`, `RELIANCE`).
- `expiry`: ISO date (`YYYY-MM-DD`) from the [Expiries API](./expiries.md). Defaults to the nearest expiry if omitted.
- `instrument_type`: `option` (default) or `fut`.
- `count`: number of strikes, in multiples of 10. Default `40` (80 instruments: 40 calls + 40 puts).
- **Expected values for `exchange` are (string):**
  * `nse_fo` (NSE F&O)
  * `bse_fo` (BSE F&O)
  * `mcx_fo` (MCX F&O)

With `instrument_type=fut` and `expiry` omitted, `fut[]` returns every available futures contract; with a specific `expiry`, it returns only that one contract.

## 3. Headers

| Name          | Type   | Description                                                |
| ------------- | ------ | ---------------------------------------------------------- |
| Authorization | string | Token provided in your NEO API dashboard - use plain token |
| Content-Type  | string | `application/json`                                         |

## 4. Request

**Example Request (mandatory parameters only — nearest expiry, default option chain):**

```
curl --location --request GET '<Base URL>/market-data/1.0/watchlist/option-chain?exchange=nse_fo&underlying=NIFTY' \
--header 'Content-Type: application/json' \
--header 'Authorization: xxxxx-your-api-token-xxxx'
```

**Example Request (with optional parameters):**

```
curl --location --request GET '<Base URL>/market-data/1.0/watchlist/option-chain?exchange=nse_fo&underlying=RELIANCE&expiry=2026-06-23&instrument_type=option&count=40' \
--header 'Content-Type: application/json' \
--header 'Authorization: xxxxx-your-api-token-xxxx'
```

**Example Request (futures chain):**

```
curl --location --request GET '<Base URL>/market-data/1.0/watchlist/option-chain?exchange=nse_fo&underlying=RELIANCE&instrument_type=fut' \
--header 'Content-Type: application/json' \
--header 'Authorization: xxxxx-your-api-token-xxxx'
```

## 5. Response

## Example Success Response - Option chain

```
{
  "data": {
    "common_data": {
      "mktLot": "65",
      "multiplier": "1",
      "unlSymbol": "NIFTY",
      "exSeg": "nse_fo",
      "expiryDt": "2026-06-23"
    },
    "call": [
      {
        "instrument": {
          "neoSymbol": "nse_fo|71472",
          "symbol": "NIFTY26JUN22250CE",
          "optionType": "CE",
          "strikePrice": "22250",
          "moneyness": "ATM"
        },
        "quote": {
          "ltp": "166.7500",
          "open": "89.8000",
          "high": "214.0000",
          "low": "77.1000",
          "prevClose": "99.7000",
          "close": null,
          "volume": 225431505
        },
        "openInterest": {
          "current": 10715645,
          "previous": 13647630,
          "change": -2931985,
          "changePct": -21.48
        }
      }
    ],
    "put": []
  }
}
```

## Example Success Response - Futures chain (`instrument_type=fut`)

```
{
  "data": {
    "common_data": {
      "mktLot": "65",
      "multiplier": "1",
      "unlSymbol": "NIFTY",
      "exSeg": "nse_fo",
      "expiryDt": null
    },
    "call": [],
    "put": [],
    "fut": [
      {
        "inst": {
          "neoSymbol": "nse_fo|53001",
          "symbol": "NIFTY26JULFUT",
          "expiryDt": "31-JUL-2026"
        },
        "quote": {
          "ltp": "24485.20",
          "o": "24455.00",
          "h": "24512.00",
          "l": "24428.00",
          "c": "24485.20",
          "pc": "24322.10",
          "vol": "9852340"
        },
        "oi": {
          "cur": "13930930",
          "prev": "13801200",
          "chg": "129730",
          "chgPct": "0.94"
        }
      }
    ]
  }
}
```

## Response Field Mapping

**`common_data` object**

| Field      | Type        | Description                                          |
| ---------- | ----------- | --------------------------------------------------- |
| mktLot     | string      | Market (lot) size                                   |
| multiplier | string      | Price multiplier                                    |
| unlSymbol  | string      | Underlying symbol                                   |
| exSeg      | string      | Exchange segment                                    |
| expiryDt   | string/null | Expiry date (`YYYY-MM-DD`); `null` for a full `fut` chain |

**`call[]` / `put[]` element**

| Field        | Type   | Description                        |
| ------------ | ------ | ----------------------------------- |
| instrument   | object | Instrument identity (see below)    |
| quote        | object | Quote fields (see below)           |
| openInterest | object | Open-interest fields (see below)   |

**`instrument` object**

| Field       | Type   | Description                              |
| ----------- | ------ | ----------------------------------------- |
| neoSymbol   | string | `{exchange_segment}\|{instrument_token}` |
| symbol      | string | Trading symbol                           |
| optionType  | string | `CE` or `PE`                             |
| strikePrice | string | Strike price                             |
| moneyness   | string | `ITM`, `ATM` or `OTM`                    |

**`quote` object**

| Field     | Type        | Description                                        |
| --------- | ----------- | --------------------------------------------------- |
| ltp       | string      | Last traded price                                  |
| open      | string      | Day open                                           |
| high      | string      | Day high                                           |
| low       | string      | Day low                                            |
| prevClose | string      | Previous close (stable for the session)            |
| close     | string/null | `null` intraday; populated at settlement           |
| volume    | number      | Traded volume                                      |

**`openInterest` object**

| Field     | Type   | Description                                    |
| --------- | ------ | ------------------------------------------------ |
| current   | number | Current open interest                          |
| previous  | number | Previous open interest                         |
| change    | number | Server-computed OI change (current - previous) |
| changePct | number | Server-computed OI change %                    |

**`fut[]` element** (futures chain)

| Field | Type   | Description                                                        |
| ----- | ------ | ------------------------------------------------------------------- |
| inst  | object | `neoSymbol`, `symbol`, `expiryDt`                                  |
| quote | object | `ltp`, `o`, `h`, `l`, `c`, `pc`, `vol` (abbreviated quote keys)    |
| oi    | object | `cur`, `prev`, `chg`, `chgPct` (abbreviated open-interest keys)    |

## Example Error Response

```
{
  "stat": "Not_Ok",
  "emsg": "Invalid instrument/code",
  "stCode": 1009
}
```

| Field  | Type   | Description          |
| ------ | ------ | -------------------- |
| stat   | string | "Not\_Ok" for errors |
| emsg   | string | Error message        |
| stCode | int    | Error code            |

## 6. Notes

- `exchange` and `underlying` must both be passed; `underlying` is matched against `pSymbolName` in the instrument/scrip master.
- `expiry` must be in `YYYY-MM-DD` format; omit it to default to the nearest expiry.
- `count` must be a multiple of 10 (default 40). The API returns `count` calls and `count` puts.
- `openInterest.change` / `changePct` are server-computed from current vs. previous OI.
- `close` stays `null` intraday and is populated at settlement; `prevClose` is stable for the session.
- Futures entries use the abbreviated `inst` / `quote` (`o`, `h`, `l`, `c`, `pc`, `vol`) / `oi` (`cur`, `prev`, `chg`, `chgPct`) keys, distinct from the option-strike shape.
- Valid exchange segments are: **nse_fo, bse_fo, mcx_fo** (must be passed as string).
- The `Authorization` header alone is sufficient for this endpoint; a completed 2FA (TOTP) session is not required.
