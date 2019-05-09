# Public Rest API for AraguaneyBits (2019-05-09)
# General API Information
* The base endpoint is: **https://api.araguaneybits.com**
* All endpoints return either a JSON object or array.
* Data is returned in **descending** order. Newest first, oldest last.
* All time and timestamp related fields are in milliseconds.
* HTTP `4XX` return codes are used for for malformed requests;
  the issue is on the sender's side.
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `418` return code is used when an IP has been auto-banned for continuing to send requests after receiving `429` codes.
* HTTP `5XX` return codes are used for internal errors; the issue is on
  Binance's side.
  It is important to **NOT** treat this as a failure operation; the execution status is
  **UNKNOWN** and could have been a success.
* Any endpoint can return an ERROR; the error payload is as follows:
```javascript
{
  "code": -1121,
  "msg": "Invalid symbol."
}
```
* Specific error codes and messages defined in another document.
* For `GET` endpoints, parameters must be sent as a `query string`.
* For `POST`, `PUT`, and `DELETE` endpoints, the parameters may be sent as a
  `query string` or in the `request body` with content type
  `application/x-www-form-urlencoded`. You may mix parameters between both the
  `query string` and `request body` if you wish to do so.
* Parameters may be sent in any order.
* If a parameter sent in both the `query string` and `request body`, the
  `query string` parameter will be used.
    
# Public API Endpoints

## General endpoints
### Test connectivity
```
GET /api/v1/version
```
Test connectivity to the Rest API.

**Weight:**
1

**Parameters:**
NONE

**Response:**
```javascript
{
    "version": "1.0.0"
}
```

### Check server time
```
GET /api/v1/time
```
Test connectivity to the Rest API and get the current server time.

**Weight:**
1

**Parameters:**
NONE

**Response:**
```javascript
{
    "server_time": 1557433979908
}
```

### Instruments
```
GET /api/v1/instruments
```
Get a list of valid symbol IDs and the pair details.

**Weight:**
1

**Parameters:**
NONE

**Response:**
```javascript
[
    {
        "symbol": "AREPA-BTC",
        "currency": "AREPA",
        "currency_code": "BTC",
        "state": "active"
    },
    {
        "symbol": "BOLI-BTC",
        "currency": "BOLI",
        "currency_code": "BTC",
        "state": "active"
    },
    {
        "symbol": "DASH-BTC",
        "currency": "DASH",
        "currency_code": "BTC",
        "state": "active"
    },
    {
        "symbol": "LKR-BTC",
        "currency": "LKR",
        "currency_code": "BTC",
        "state": "active"
    },
    {
        "symbol": "ONX-BTC",
        "currency": "ONX",
        "currency_code": "BTC",
        "state": "active"
    }
]
```


### Currencies
```
GET /api/v1/currencies
```
Get a list of supported currencies.

**Weight:**
1

**Parameters:**
NONE

**Response:**
```javascript
[
    {
        "name": "DASH",
        "code": "DASH",
        "is_crypto": true,
        "decimal_number": 8,
        "withdraw_fees": "40000",
        "state": "active",
        "deposit_confirmation": 10
    },
    {
        "name": "BITCOIN",
        "code": "BTC",
        "is_crypto": true,
        "decimal_number": 8,
        "withdraw_fees": "80000",
        "state": "active",
        "deposit_confirmation": 3
    }
]
```


### Tickers
```
GET /api/v1/ticker
```
The ticker is a high level overview of the state of the market. It shows you the current best bid and ask, as well as the last trade price. It also includes information such as daily volume and how much the price has moved over the last day.

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |


**Response:**
```javascript
{
    "symbol": "ONX-BTC",
    "mid": "0.00000008",
    "bid": "0.00000006",
    "ask": "0.00000010",
    "high": "0.00000010",
    "low": "0.00000008",
    "volume_pair": "292.03888889",
    "volume": "0.00002682",
    "last_price": "0.00000010",
    "price_close_yesterday": "0.00000007",
    "price_open_today": "0.00000009",
    "vwap": "0",
    "percent_change": "42.86",
    "date": "2019-05-09T20:49:46.579Z"
}
```

### Orderbook
```
GET /api/v1/orderbook
```
Get the full order book.

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |


**Response:**
```javascript
{
    "asks": [
        {
            "price": "0.00000010",
            "quantity": "210.22418254",
            "amount": "0.00002107",
            "acumulate": "210.22418254"
        },
        {
            "price": "0.00000011",
            "quantity": "345.00000000",
            "amount": "0.00003795",
            "acumulate": "555.22418254"
        }
    ],
    "bids": [
        {
            "price": "0.00000006",
            "quantity": "401.33333333",
            "amount": "0.00002408",
            "acumulate": "19534.04520421"
        },
        {
            "price": "0.00000005",
            "quantity": "500.00000000",
            "amount": "0.00002500",
            "acumulate": "19534.04522921"
        }
    ]
}
```

### Trades
```
GET /api/v1/trades
```
Get a list of the most recent trades for the given symbol.

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |


**Response:**
```javascript
[
    {
        "side": "buy",
        "symbol": "ONX-BTC",
        "created": "2019-05-09T20:00:01.979Z",
        "quantity": "6.60000000",
        "volume": "0.00000066",
        "price": "0.00000010"
    },
    {
        "side": "sell",
        "symbol": "ONX-BTC",
        "created": "2019-05-09T19:00:01.780Z",
        "quantity": "8.00000000",
        "volume": "0.00000080",
        "price": "0.00000010"
    }
]
```
