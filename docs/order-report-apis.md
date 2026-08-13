# Order Report APIs

## Covers: Order Book, Orderbook by Order ID, Order History, Trade Book APIs

## 1. Introduction

Kotak Securities offers APIs to fetch your:

- **Order Book** (all open, completed, and rejected orders)
    - order book by order ID
- **Order History** (all order status changes/updates for a particular order)
- **Trade Book** (details of completed trades for the trading day)

These APIs allow you to fetch and monitor all relevant trading activity programmatically.

## 2. API Endpoints

| API | Endpoint (after <Base URL>) | Method |
| --- | --- | --- |
| Order Book | `/quick/user/orders` | GET |
| Orderbook by order id | `/quick/user/orders/<order_no>` | GET |
| Order History | `/quick/order/history` | POST |
| Trade Book | `/quick/user/trades` | GET |

> *Replace `<Base URL>` with the relevant Kotak environment base URL provided in response from /tradeApiValidate api.*
> 

## 3. Headers

Applicable to all three APIs:

| Name | Type | Description |
| --- | --- | --- |
| accept | string | Should always be `application/json` |
| Sid | string | session sid generated on login |
| Auth | string | session token generated on login |
| neo-fin-key | string | static value: neotradeapi |
| Content-Type | string | Always `application/x-www-form-urlencoded` |

## 4. Request

## 4.1. GET Order Book

**Example Request:**

```jsx
curl -X GET "<baseUrl>/quick/user/orders" \
  -H "Auth: <session_token>" \
  -H "Sid: <session_sid>" \
  -H "neo-fin-key: neotradeapi"
```

### 4.2. GET Order book by Order ID

```jsx
curl -X GET "<baseUrl>/quick/user/orders/<order_no>" \
  -H "Auth: <session_token>" \
  -H "Sid: <session_sid>" \
  -H "neo-fin-key: neotradeapi"
```

> No request body or parameters required.
> 

## 4.3. POST Order History

**Request Body:**

Send `jData` as url-encoded JSON.

Example:

```jsx
curl -X GET "<baseUrl>/quick/user/order/history" \
  -H "Auth: <session_token>" \
  -H "Sid: <session_sid>" \
  -H "neo-fin-key: neotradeapi"
--data-urlencode 'jData={"nOrdNo":"250720000007242"}'
```

| Field | Type | Description | Required |
| --- | --- | --- | --- |
| nOrdNo | string | Nest order number for which history is required | Yes |

## 4.4. GET Trade Book

**Example Request:**

```jsx
# Trade Book
curl -X GET "<baseUrl>/quick/user/trades" \
  -H "Auth: <session_token>" \
  -H "Sid: <session_sid>" \
  -H "neo-fin-key: neotradeapi"No request body or parameters required.
```

## 5. Response

## 5.1. GET Order Book Response

**Success Example (truncated):**

```jsx
{
  "stat": "Ok",
  "data": [
    {
      "nOrdNo": "250720000007242",
      "ordSt": "rejected",
      "trdSym": "ITBEES-EQ",
      "qty": 1,
      "prc": "0.00",
      "avgPrc": "0.00",
      "trnsTp": "B",
      "prcTp": "L",
      "vldt": "DAY",
      ...
    },
    {
      "nOrdNo": "250720000007588",
      "ordSt": "after market order req received",
      "trdSym": "ITBEES-EQ",
      "qty": 1,
      "prc": "0.00",
      "avgPrc": "0.00",
      ...
    }
  ],
  "stCode": 200
}
```

**Important Fields (Truncated and Simplified)**

| Field | Type | Description |
| --- | --- | --- |
| nOrdNo | string | Nest order number |
| ordSt / stat | string | Order status ("open", "rejected", etc.) |
| trdSym | string | Trading symbol e.g., "ITBEES-EQ" |
| qty | int | Quantity |
| prc | string | Placed price |
| avgPrc | string | Average traded price |
| trnsTp | string | Transaction type - "B"=Buy, "S"=Sell |
| prcTp | string | Order type: "L"=Limit, "MKT"=Market, etc. |
| vldt | string | Validity (DAY/IOC) |
| rejRsn | string | Rejection reason if any |
| exSeg | string | Exchange segment e.g., "nse_cm" |
| ordGenTp | string | "AMO" for after-market orders, else blank |
| ordDtTm | string | Order date/time |
| stat | string | Overall status at top level: "Ok" for success |

*The order book contains all available fields per order as per [API glossary](https://www.hypersync.in/apidoc_neo/#api-BOOKS-OrderBook).*

## 5.2. POST Order History Response

**Success Example (truncated):**

```jsx
{
  "stat": "Ok",
  "stCode": 200,
  "data": [
    {
      "nOrdNo": "250720000007242",
      "flDtTm": "20-Jul-2025 20:21:42",
      "ordSt": "rejected",
      "rejRsn": "ADAPTER is down",
      "qty": 1,
      "prc": "0.00",
      "avgPrc": "0.00",
      "prod": "MIS",
      "trnsTp": "B",
      "prcTp": "L",
      ...
    },
    {
      "nOrdNo": "250720000007242",
      "flDtTm": "20-Jul-2025 20:21:42",
      "ordSt": "open pending",
      ...
    }
  ]
}
```

| Field | Type | Description |
| --- | --- | --- |
| nOrdNo | string | Nest order number |
| ordSt | string | Status at this stage |
| flDtTm | string | Date/time for the update |
| qty | int | Order quantity at this status |
| prc | string | Order price at this status |
| avgPrc | string | Average price at this stage |
| prod | string | Product type ("MIS", "CNC") |
| trnsTp | string | Transaction type ("B"=Buy, "S"=Sell) |
| prcTp | string | Order type ("L", "MKT", etc.) |
| rejRsn | string | Rejection reason if applicable |

*Full list of all response fields is available in [Order Book API Glossary](https://www.hypersync.in/apidoc_neo/#api-BOOKS-OrderBook), as most fields are common.*

## 5.3. GET Trade Book Response

**Success Example (truncated):**

```jsx
{
  "stat": "Ok",
  "stCode": 200,
  "data": [
    {
      "nOrdNo": "221007000000354",
      "trdSym": "TCS-EQ",
      "qty": 11,
      "avgPrc": "3194.00",
      "fldQty": 11,
      "flDt": "07-Oct-2022",
      "exOrdId": "1100000000047870",
      "exTm": "07-Oct-2022 13:04:14",
      "prcTp": "L",
      "prod": "CNC",
      "ordDur": "DAY",
      "trnsTp": "B",
      "usrId": "PRABHAT",
      ...
    }
  ]
}
```

| Field | Type | Description |
| --- | --- | --- |
| nOrdNo | string | Nest order number |
| trdSym | string | Trading symbol |
| qty | int | Trade quantity |
| avgPrc | string | Average execution price |
| fldQty | int | Filled quantity |
| flDt | string | Trade date |
| exOrdId | string | Exchange order ID |
| exTm | string | Trade execution datetime |
| prcTp | string | Order type (L/MKT/SL/etc.) |
| prod | string | Product code ("CNC","MIS", etc.) |
| ordDur | string | Order validity (DAY/IOC) |
| trnsTp | string | Transaction type (B/S) |
| usrId | string | User/client ID |

*Additional fields are available and can be referenced from the [API glossary](https://www.hypersync.in/apidoc_neo/#api-BOOKS-OrderBook).*

## Common Response Fields

| Field | Type | Description |
| --- | --- | --- |
| stat | string | "Ok" for success, "Not_Ok" for errors |
| stCode | int | HTTP status code (200 = success, else error) |
| data | array | List of order/trade details objects |
| emsg | string | Present only for errors: error message |

## 6. Usage Notes

- Use correct headers with valid session and auth tokens.
- For **Order Book** and **Trade Book**: Access all your recent orders/trades for the day.
- For **Order History**: Always provide the correct `nOrdNo` (order number) to fetch its full lifecycle/status changes.
- Handle `"stat": "Not_Ok"` and use `"emsg"` for debugging API issues.
- Field meanings and data formats remain consistent across all order-related APIs.
- Reference scrip master for symbol/segment mapping as needed.