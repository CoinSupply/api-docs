# **Public Rest API**

### Api base path: https://api.coinsupply.com/ 

All time and timestamp related fields are in milliseconds.

**Security:**

Api token should be provided using header *X-MBX-APIKEY*.

Example: 

 *X-MBX-APIKEY*: rRdAV5DUQk5x9H9bUv8FLG7ZhDcyJHU3

**Errors:**

Any endpoint can return an ERROR; the error payload is as follows:

```json
{
  "code": -1121,
  "msg": "Invalid symbol."
}
```

**Classifiers:**

**Order side:**

* BUY

* SELL

**Order types:**

* LIMIT

* MARKET

### Check server time

GET /api/alpha/time

Test connectivity to the Rest API and get the current server time.

**Response:**
```json
{
  "serverTime": 1499827319559
}
```

### Order book

GET /api/alpha/depth

Cumulative values for higher prices includes also lower price amounts.

**Parameters:**

| Name    | Type     | Mandatory | Description   |
| ------- | -------- | --------- | ------------- |
| symbol  | STRING   | YES       |               |

**Response:**
```json
{
  "bids": [
    [
      "4.00000000",     // PRICE
      "431.00000000"   // QTY
    ]
  ],
  "asks": [
    [
      "4.00000200",
      "12.00000000"
    ]
  ]
}
```


### Recent trades list

GET /api/alpha/trades

**Parameters:**

| Name      | Type       | Mandatory         | Description          |
| --------- | ---------- | ----------------- | -------------------- |
| symbol    | STRING     | YES               |

**Response:**
```json
[
  {
    "id": 28457,
    "price": "4.00000100",
    "qty": "12.00000000",
    "time": 1499865549590,
    "isBuyerMaker": true
  }
]
```

### New order

POST /api/alpha/order 

Send in a new order.

**Parameters:**

| Name     | Type    | Mandatory | Description |
| -------- | ------- | --------- | ----------- |
| symbol   | STRING  | YES       |
| side     | ENUM    | YES       |
| type     | ENUM    | YES       |
| quantity | DECIMAL | YES       |
| price    | DECIMAL | NO        |



Additional mandatory parameters based on type:

| Type   | Additional mandatory parameters |
| ------ | ------------------------------- |
| LIMIT  | quantity, price                 |
| MARKET | quantity                        |


**Response RESULT:**
```json
{
  "symbol": "BTCETH",
  "orderId": 28,
  "clientOrderId": "6gCrw2kRUAF9CvJDGP16IP",
  "transactTime": 1507725176595,
  "price": "0.00000000",
  "origQty": "10.00000000",
  "executedQty": "10.00000000",
  "status": "PLACED",
  "type": "MARKET",
  "side": "SELL"
}
```

### Cancel order

DELETE /api/alpha/order  

Cancel an active order.

**Parameters:**

| Name    | Type | Mandatory | Description |
| ------- | ---- | --------- | ----------- |
| orderId | LONG | YES       |


**Response:**
```json
{
  "symbol": "LTCBTC",
  "origClientOrderId": "myOrder1",
  "orderId": 1,
  "clientOrderId": "cancelMyOrder1"
}
```

###Current open orders

GET /api/alpha/openOrders

**Parameters:**

| Name   | Type   | Mandatory | Description |
| ------ | ------ | --------- | ----------- |
| symbol | STRING | NO        |


* If the symbol is not sent, orders for all symbols will be returned in an array.

**Response:**
```json
[
  {
    "symbol": "LTCBTC",
    "orderId": 1,
    "clientOrderId": "myOrder1",
    "price": "0.1",
    "origQty": "1.0",
    "executedQty": "0.0",
    "status": "NEW",
    "timeInForce": "GTC",
    "type": "LIMIT",
    "side": "BUY",
    "stopPrice": "0.0",
    "icebergQty": "0.0",
    "time": 1499827319559,
    "isWorking": true
  }
]
```

### Account trade list

GET /api/alpha/myTrades

**Parameters:**

| Name   | Type   | Mandatory | Description |
| ------ | ------ | --------- | ----------- |
| symbol | STRING | YES       |

**Response:**
```json
[
  {
    "id": 28457,
    "orderId": 100234,
    "price": "4.00000100",
    "qty": "12.00000000",
    "commission": "10.10000000",
    "commissionAsset": "BNB",
    "time": 1499865549590,
    "isBuyer": true,
    "isMaker": false
  }
]
```
