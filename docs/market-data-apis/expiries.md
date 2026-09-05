# Expiries

## 1. Introduction

The **Expiries API** returns the list of available expiry dates for an exchange + underlying, in ISO (`YYYY-MM-DD`) format. It lets you populate an expiry selector without fetching a full option chain — pass any returned date straight into the [Option Chain API](./option-chain.md) as the `expiry` parameter.

## 2. API Endpoint

```
GET <Base URL>/market-data/1.0/watchlist/expiries?underlying=<underlying>&exchange=<exchange_segment>[&instrument_type=<type>]
```

**Replace `<Base URL>` with the relevant Kotak environment base URL provided in response from the `/tradeApiValidate` API.**

**Key points about endpoint structure:**

- `underlying` and `exchange` are mandatory; `instrument_type` is optional.
- `underlying`: use `pSymbolName` from the instrument/scrip master file (e.g. `NIFTY`, `RELIANCE`).
- **Expected values for `exchange` are (string):**
  * `nse_fo` (NSE F&O)
  * `bse_fo` (BSE F&O)
  * `mcx_fo` (MCX F&O)
- `instrument_type` (optional): `option` (default) or `fut`.

## 3. Headers

| Name          | Type   | Description                                                |
| ------------- | ------ | ---------------------------------------------------------- |
| Authorization | string | Token provided in your NEO API dashboard - use plain token |
| Content-Type  | string | `application/json`                                         |

## 4. Request

**Example Request:**

```
curl --location --request GET '<Base URL>/market-data/1.0/watchlist/expiries?underlying=NIFTY&exchange=nse_fo' \
--header 'Content-Type: application/json' \
--header 'Authorization: xxxxx-your-api-token-xxxx'
```

**Example Request (with optional `instrument_type`):**

```
curl --location --request GET '<Base URL>/market-data/1.0/watchlist/expiries?underlying=CRUDEOIL&exchange=mcx_fo&instrument_type=fut' \
--header 'Content-Type: application/json' \
--header 'Authorization: xxxxx-your-api-token-xxxx'
```

## 5. Response

## Example Success Response

```
{
  "exchange": "nse_fo",
  "underlying": "RELIANCE",
  "expiries": [
    "2026-06-25",
    "2026-06-30",
    "2026-07-31"
  ]
}
```

## Response Field Mapping

| Field      | Type   | Description                                              |
| ---------- | ------ | -------------------------------------------------------- |
| exchange   | string | Exchange segment, echoed back from the request           |
| underlying | string | Underlying name, echoed back from the request            |
| expiries   | array  | Flat array of ISO (`YYYY-MM-DD`) date strings, ascending |

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

- `underlying` and `exchange` must both be passed.
- `underlying` is matched against `pSymbolName` in the instrument/scrip master.
- `expiries` is always a flat array of ISO date strings, sorted ascending.
- Valid exchange segments are: **nse_fo, bse_fo, mcx_fo** (must be passed as string).
- The `Authorization` header alone is sufficient for this endpoint; a completed 2FA (TOTP) session is not required.
