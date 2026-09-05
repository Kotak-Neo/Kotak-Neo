# Historical Data

## 1. Introduction

The **Historical Data API** returns historical candle (OHLCV) data for an instrument. The response is a single `candles` array — one fixed-order positional row per candle — with ISO 8601 timestamps.

## 2. API Endpoint

```
GET <Base URL>/market-data/1.0/historical/details?neosymbol=<exchange_segment>|<instrument_token>&fromdate=<YYYY-MM-DD>&todate=<YYYY-MM-DD>&interval=<interval>
```

**Replace `<Base URL>` with the relevant Kotak environment base URL provided in response from the `/tradeApiValidate` API.**

**Key points about endpoint structure:**

- `neosymbol`, `fromdate`, `todate` and `interval` are all mandatory.
- `neosymbol` is formatted as `<exchange_segment>|<instrument_token>`, e.g. `nse_cm|1333`. The pipe can be sent as a literal `|` (or percent-encoded as `%7C`).
- Instrument token: use the token from the instrument/scrip master file.
- **Supported exchange segments (string):** `nse_cm` (NSE Cash), `nse_fo` (NSE F&O), `bse_cm` (BSE Cash), `bse_fo` (BSE F&O) only.
- Only **active contracts with a valid neo symbol** return historical data — expired or delisted instruments will not.
- **Expected values for `interval` are (string):**
  * `1min`, `3min`, `5min`, `10min`, `15min`, `30min`, `60min`
  * `D` (daily)
  * `W` (weekly)

### Date Range Limits

The maximum date range per request depends on the interval. A request exceeding the limit is rejected by the backend.

| Interval               | Max Range | Example Use Case        |
| ---------------------- | --------- | ------------------------ |
| `1min`, `3min`, `5min` | 30 days   | Intraday trading charts |
| `10min`, `15min`       | 60 days   | Short-term analysis     |
| `30min`, `60min`       | 90 days   | Medium-term analysis    |
| `D` (daily)            | 180 days  | Long-term charts        |
| `W` (weekly)           | 180 days  | Trend analysis          |

## 3. Headers

| Name          | Type   | Description                                                |
| ------------- | ------ | ---------------------------------------------------------- |
| Authorization | string | Token provided in your NEO API dashboard - use plain token |
| Content-Type  | string | `application/json`                                         |

## 4. Request

**Example Request:**

```
curl --location --request GET '<Base URL>/market-data/1.0/historical/details?neosymbol=nse_cm|1333&fromdate=2026-08-20&todate=2026-09-01&interval=10min' \
--header 'Content-Type: application/json' \
--header 'Authorization: xxxxx-your-api-token-xxxx'
```

## 5. Response

## Example Success Response

```
{
  "status": "success",
  "interval": "1min",
  "data": {
    "candles": [
      ["2026-08-20T09:15:00+0530", 12009.9, 12019.35, 12001.25, 12001.5, 163275, 13667775],
      ["2026-08-20T09:16:00+0530", 12001, 12003, 11998.25, 12001, 105750, 13667775]
    ]
  }
}
```

Each entry in `candles` is a fixed-order positional row (an array, **not** an object) in the column order `[timestamp, open, high, low, close, volume, oi]`.

## Response Field Mapping

| Field         | Type   | Description                                            |
| ------------- | ------ | -------------------------------------------------------- |
| status        | string | `success` on success                                   |
| interval      | string | Interval, echoed back from the request                 |
| data          | object | Container for the candle data                          |
| data.candles  | array  | Array of candle rows; each row is a positional array   |

**Candle row (positional array)**

| Index | Field     | Type   | Description                                            |
| ----- | --------- | ------ | -------------------------------------------------------- |
| 0     | timestamp | string | Candle timestamp, ISO 8601 (e.g. `...T09:15:00+0530`) |
| 1     | open      | number | Open price                                             |
| 2     | high      | number | High price                                             |
| 3     | low       | number | Low price                                              |
| 4     | close     | number | Close price                                            |
| 5     | volume    | number | Traded volume                                          |
| 6     | oi        | number | Open interest — **Phase 1: not yet populated**         |

## Example Error Response

An unsupported `interval` is rejected by the backend:

```
{
  "status": "ERROR",
  "fault": {
    "code": 400,
    "message": "Invalid interval value"
  }
}
```

| Field         | Type   | Description                     |
| ------------- | ------ | -------------------------------- |
| status        | string | `ERROR` for errors              |
| fault.code    | int    | Error code                      |
| fault.message | string | Error message                   |

## 6. Notes

- `neosymbol`, `fromdate`, `todate` and `interval` are all mandatory; a missing parameter is rejected by the backend.
- Supported only for the **`nse_cm`, `nse_fo`, `bse_cm` and `bse_fo`** exchange segments.
- Only **active contracts with a valid neo symbol** return data; expired or delisted instruments do not.
- The pipe in `neosymbol` can be sent as a literal `|`; it may also be percent-encoded as `%7C`.
- `timestamp` in each candle row is ISO 8601.
- `interval` is echoed as a top-level key, just before `data`.
- **Phase 1 note:** the `oi` position in each candle row is not yet populated (planned for a later phase) — don't rely on it being present / non-null yet.
- A date range exceeding the interval's limit, or an unsupported `interval`, is rejected by the backend.
