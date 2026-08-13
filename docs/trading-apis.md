# Trading APIs - Place, modify and cancel order

# Place Order API

## 1. Introduction

The **Place Order API** allows you to place buy or sell orders across all supported exchange segments and order types. It supports product types like NRML, CNC, MIS, CO, and BO, and includes specialized fields for Bracket Orders (BO) and Cover Orders (CO).

## 2. API Endpoint

`POST <Base URL>/quick/order/rule/ms/place`

*Replace `<Base URL>` with the relevant Kotak environment base URL provided in response from /tradeApiValidate api.*

## 3. Headers

| Name | Type | Description |
| --- | --- | --- |
| accept | string | Should always be `application/json` |
| Sid | string | session sid generated on login |
| Auth | string | session token generated on login |
| neo-fin-key | string | static value: neotradeapi |
| Content-Type | string | Always `application/x-www-form-urlencoded` |

## 4. Request Body

The request body is sent as a single field named `jData`, which is a stringified JSON object and must be URL-encoded.

## Example Request

```jsx
curl -X POST "<baseUrl>/quick/order/rule/ms/place" \
  -H "Auth: <session_token>" \
  -H "Sid: <session_sid>" \
  -H "neo-fin-key: neotradeapi" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode 'jData={
    "am": "NO",
    "dq": "0",
    "es": "nse_cm",
    "mp": "0",
    "pc": "CNC",
    "pf": "N",
    "pr": "0",
    "pt": "MKT",
    "qt": "1",
    "rt": "DAY",
    "tp": "0",
    "ts": "ITBEES-EQ",
    "tt": "B"
  }'

```

## Example Request Body (`jData`)

```jsx
{
  "am": "NO",
  "dq": "0",
  "es": "nse_cm",
  "mp": "0",
  "pc": "MIS",
  "pf": "N",
  "pr": "0",
  "pt": "L",
  "qt": "1",
  "rt": "DAY",
  "tp": "0",
  "ts": "******-**",
  "tt": "B",
 
}
```

<aside>
💡

## **Note on bracket orders and cover orders:**

Bracket order and cover orders have been discontinued on Trade APIs since Apr 1, 2026.

</aside>

## Request Body Fields

| Name | Type | Description | Allowed / Example Values |
| --- | --- | --- | --- |
| am | string | After Market Order flag. | "NO" (normal), "YES" (AMO) |
| dq | string | Disclosed quantity. | "0" or a partial quantity |
| es | string | Exchange segment code. | "nse_cm", "bse_cm", "nse_fo", "bse_fo", "cde_fo", “mcx_fo” |
| mp | string | Market protection value (used in some market orders). | "0" or numerical value |
| pc | string | Product code. | "NRML", "CNC", "MIS", "CO", "BO", “MTF” |
| pf | string | Portfolio flag. | "N" |
| pr | string | Price for limit order, "0" for market order. | e.g. "0", "450.5" |
| pt | string | Order type. | "L" (Limit), "MKT" (Market), "SL" (Stoploss), "SL-M" (SL-Market) |
| qt | string | Order quantity. | e.g. "1", "100", etc. |
| rt | string | Validity or order duration. | "DAY", "IOC" |
| tp | string | Trigger price **(used for SL/SL-M/CO).** | "0" or actual trigger price.  |
| ts | string | Trading symbol (from scrip master file). | e.g., "ITBEES-EQ" |
| tt | string | Transaction type. | "B" (Buy), "S" (Sell) |

## 5. Response

## Example Success Response

```jsx
{
  "nOrdNo": "250720000007242",
  "stat": "Ok",
  "stCode": 200
}
```

## Success (200) Response Fields

| Name | Type | Description |
| --- | --- | --- |
| nOrdNo | string | Unique order number assigned to your request |
| stat | string | Status message, "Ok" if successful |
| stCode | int | HTTP status code, 200 for success |

## Example Error Response

```jsx
{
  "stat": "Not_Ok",
  "emsg": "Insufficient balance.",
  "stCode": 1004
}
```

## Error Response Fields

| Name | Type | Description |
| --- | --- | --- |
| stat | string | Status message, "Not_Ok" for errors |
| emsg | string | Error message in plain English |
| stCode | int | Error code (see below) |

## **Tips & Notes**

- Make sure all header tokens and session information are obtained using prior authentication flows.
- Use the latest Scrip Master file to get correct trading symbols and instrument details.
- Use appropriate BO/CO fields **only** when placing Bracket or Cover orders.
- Handle all non-200 status codes in your integration for robust error management.

# Modify Order API

## 1. Introduction

The **Modify Order API** allows you to modify an already placed order’s parameters—such as quantity, price, validity, product type, and more—across supported segments and order types before it is executed or fully filled.

## 2. API Endpoint

`POST <Base URL>/quick/order/vr/modify`

*Replace `<Base URL>` with the relevant Kotak environment base URL provided in response from /tradeApiValidate api.*

## 3. Headers

| Name | Type | Description |
| --- | --- | --- |
| accept | string | Should always be `application/json` |
| Sid | string | session sid generated on login |
| Auth | string | session token generated on login |
| neo-fin-key | string | static value: neotradeapi |
| Content-Type | string | Always `application/x-www-form-urlencoded` |

## 4. Request Body

The request body uses a single field named `jData`, which is a URL-encoded JSON object.

## Example Request

```jsx
curl -X POST "<baseUrl>/quick/order/vr/modify" \
  -H "Auth: <session_token>" \
  -H "Sid: <session_sid>" \
  -H "neo-fin-key: neotradeapi" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode 'jData={
    "am": "NO",
    "dq": "0",
    "es": "nse_cm",
    "mp": "0",
    "pc": "NRML",
    "pf": "N",
    "pr": "0",
    "pt": "MKT",
    "qt": "1",
    "rt": "DAY",
    "tp": "0",
    "ts": "TATAPOWER-EQ",
    "tt": "B",
    "no": "<orderNo>"
  }'

```

## Example Request Body (`jData`)

```jsx
{
  "tk": "*****",
  "mp": "0",
  "pc": "NRML",
  "dd": "NA",
  "dq": "0",
  "vd": "DAY",
  "ts": "******-**",
  "tt": "B",
  "pr": "3001",
  "tp": "0",
  "qt": "10",
  "no": "***************",
  "es": "nse_cm",
  "pt": "L"
}
```

## Request Body Fields

| Name | Type | Description | Allowed / Example Values |
| --- | --- | --- | --- |
| tk | string | Token (Instrument token from scrip master, as **pSymbol** column) | "11536", or as from the scrip master pSymbol column |
| fq | string | Filled Quantity (optional) | "10", "0" |
| mp | string | Market protection value | "0" |
| pc | string | Product code | "NRML", "CNC", "MIS", "CO", "BO" |
| dd | string | Date/Days (trailing validity, if applicable) | "NA" or as required |
| dq | string | Disclosed quantity | "0" or a partial quantity |
| vd | string | Validity (order duration) | "DAY", "IOC" |
| ts | string | Trading Symbol (from scrip master) | "TCS-EQ", etc. |
| tt | string | Transaction type | "B" (Buy), "S" (Sell) |
| pr | string | Price | e.g., "3001" |
| tp | string | Trigger price (for SL, SL-M) | "0" or actual trigger price |
| qt | string | Quantity | e.g., "10" |
| no | string | Nest Order Number (system order id for the original order) | e.g., "220106000000185" |
| es | string | Exchange Segment | "nse_cm", "bse_cm", "nse_fo", "bse_fo", "cde_fo" |
| pt | string | Order Type | "L" (Limit), "MKT" (Market), "SL" (Stoploss), "SL-M" (SL-Market) |

## 5. Response

## Example Success Response

```jsx
{
  "nOrdNo": "250720000007242",
  "stat": "Ok",
  "stCode": 200
}
```

## 200 Response Fields

| Name | Type | Description |
| --- | --- | --- |
| nOrdNo | string | New Order Number created or modified |
| stat | string | "Ok" if modification successful |
| stCode | int | HTTP status code, 200 for success |

## Example Error Response

```jsx
{
  "stat": "Not_Ok",
  "emsg": "Order cannot be modified as it is already executed.",
  "stCode": 1006
}
```

## Error Response Fields

| Name | Type | Description |
| --- | --- | --- |
| stat | string | "Not_Ok" for errors |
| emsg | string | Error message in English |
| stCode | int | Error code (see below) |

## Notes

- Only orders that are **not** yet executed or completed can be modified.
- Always use valid instrument tokens, symbols, and original order numbers.
- Headers and authorization must be handled securely as in the Place Order API.
- Use the latest scrip master data for token and symbol lookups.
- Use appropriate error handling for non-200 and failure responses.

# Cancel Order APIs

## 1. Introduction

Kotak Securities provides APIs for cancelling  of open orders.

## 2. API Endpoints

| Order Type | Endpoint (after <Base URL>) |
| --- | --- |
| Regular Order | `/quick/order/cancel` |

*Replace `<Base URL>` with the relevant Kotak environment base URL provided in response from /tradeApiValidate api.*

## 3. Headers

| Name | Type | Description |
| --- | --- | --- |
| accept | string | Should always be `application/json` |
| Sid | string | session sid generated on login |
| Auth | string | session token generated on login |
| neo-fin-key | string | static value: neotradeapi |
| Content-Type | string | Always `application/x-www-form-urlencoded` |

## 4. Request Body

The request body is a single URL-encoded field named `jData`, containing a JSON object.

## Example Request

```jsx
# Cancel
curl -X POST "<baseUrl>/quick/order/cancel" \
  -H "Auth: <session_token>" \
  -H "Sid: <session_sid>" \
  -H "neo-fin-key: neotradeapi" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode 'jData={"on":"<orderNo>","am":"NO"}'

```

## Example Request Body (`jData`)

```jsx
{
  "am": "NO",
  "on": "********************",
  "ts":""//optional - in case of AMO Yes
}
```

## Request Body Fields

| Name | Type | Description | Required | Example |
| --- | --- | --- | --- | --- |
| am | string | AMO flag ("YES" for AMO orders; omit/"NO" for others) | Optional | "YES", "NO" |
| on | string | Nest order number (unique order id) | Required | "2105199703091997" |
| ts | string | Trading symbol (mandatory for AMO orders) | Optional | "TCS-EQ" |

## 5. Response

## Example Success Response

```jsx
{
  "nOrdNo": "2105199703091997",
  "stat": "Ok",
  "stCode": 200
}
```

## Success (200) Response Fields

| Name | Type | Description |
| --- | --- | --- |
| nOrdNo | string | Nest order number of the cancelled order |
| stat | string | Status, "Ok" if cancellation successful |
| stCode | int | HTTP status code, 200 for success |

## Example Error Response

```jsx
{
  "stat": "Not_Ok",
  "emsg": "Order already cancelled or not found.",
  "stCode": 1006
}
```

## Error Response Fields

| Name | Type | Description |
| --- | --- | --- |
| stat | string | "Not_Ok" for errors |
| emsg | string | Error message in English |
| stCode | int | Error code (see below) |

## 6. Usage Notes

- For **AMO cancellation**, `"am": "YES"` and `"ts"` (trading symbol) are mandatory.
- Orders already fully executed or cancelled cannot be cancelled again.
- Use the exact `on` (Nest order number) as returned in the order placement or status queries.
- Always check for `"stat":"Ok"` in the response for a successful cancellation.