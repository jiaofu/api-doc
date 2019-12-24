---
title: spot api

language_tabs: 


toc_footers:
  - <a href='https://testnet.jexzh.com/cn/contract'>Binance jex Futures</a>

includesf:
  - BaseInfo
  - CHANGELOG
includes:
  - userDataStream
  - web-socket-streams
  - errors

search: true
---


# General endpoints
## Test connectivity PING

`GET /api/v1/ping`

Test connectivity 

**Weight:**
1

**Parameters:**
NONE

>**Response**

```javascript
{}
```

## Check server time
```
GET /api/v1/time
```
Get the current server time.

**Weight:**
1

**Parameters:**
NONE

>**Response**  

```javascript
{
  "serverTime": 1499827319595
}
```

## Exchange information

`GET /api/v1/exchangeInfo`

Get the current trading rules and symbol information

**Weight:**
1

**Parameters:**
NONE

>**Response**  

```javascript
{
  "timezone": "UTC",
  "serverTime": 1551270414646,
  "rateLimits": [
    {
      "rateLimitType": "requestsWeight",
      "interval": "minute",
      "intervalNum": 1,
      "limit": 1200
    },
    {
      "rateLimitType": "orders",
      "interval": "second",
      "intervalNum": 1,
      "limit": 10
    },
    {
      "rateLimitType": "orders",
      "interval": "day",
      "intervalNum": 1,
      "limit": 100000
    },
    {
      "rateLimitType": "rawRequests",
      "interval": "minute",
      "intervalNum": 5,
      "limit": 5000
    }
  ],
  "optionSymbols": [
    {
      "symbol": "BTCCALLM",
      "status": "TRADING",
      "baseAsset": "月BTC看涨0226",
      "baseAssetPrecision": 0,
      "quoteAsset": "USDT",
      "quotePrecision": 2,
      "orderTypes": [
        "LIMIT"
      ],
      "icebergAllowed": false,
      "filters": [
        {
          "filterType": "PRICE_FILTER",
          "maxPrice": "100000"
        },
        {
          "filterType": "LOT_SIZE",
          "minQty": "0.00000000"
        }
      ]
    }
  ],
  "contractSymbols": [
    {
      "symbol": "BTCUSDT",
      "status": "TRADING",
      "baseAsset": "btc",
      "baseAssetPrecision": 4,
      "quoteAsset": "usdt",
      "quotePrecision": 0,
      "orderTypes": [
        "LIMIT",
        "MARKET"
      ],
      "icebergAllowed": false,
      "filters": [
        {
          "filterType": "PRICE_FILTER",
          "minPrice": "0.00000001000000000000",
          "maxPrice": "1000000.00000000000000000000"
        },
        {
          "filterType": "LOT_SIZE",
          "minQty": "0.00010000000000000000",
          "maxQty": "1000000.00000000000000000000"
        }
      ]
    }
  ],
  "spotSymbols": [
    {
      "symbol": "DASHUSDT",
      "status": "TRADING",
      "baseAsset": "DASH",
      "baseAssetPrecision": 4,
      "quoteAsset": "USDT",
      "quotePrecision": 2,
      "orderTypes": [
        "LIMIT"
      ],
      "icebergAllowed": false,
      "filters": [
        {
          "filterType": "PRICE_FILTER",
          "maxPrice": "100000"
        },
        {
          "filterType": "LOT_SIZE",
          "minQty": "1.00000000"
        }
      ]
    }
  ]
}
```



## Depth information for spot


`GET /api/v1/spot/depth`

**Weight:1**


**Parameters:**

Name | Type   | Mandatory  | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default  60; Max 60. Available:[5, 10, 20, 50, 60]


>**Response**  

```javascript
{
  "lastUpdateId": 1027024,
  "bids": [
    [
      "4.00000000",     // PRICE
      "431.00000000",   // QTY
      []                // IGNORE.
    ]
  ],
  "asks": [
    [
      "4.00000200",
      "12.00000000",
      []
    ]
  ]
}
```






## Recent trades for spot

`GET /api/v1/spot/trades`

Get recent trades

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 60; max 60.

>**Response**  

```javascript
[
  {
    "price": "81.230000000000000000",
    "qty": "0.001000000000000000",
    "time": 0
  },
  {
    "price": "81.230000000000000000",
    "qty": "0.000400000000000000",
    "time": 0
  },
  {
    "price": "81.240000000000000000",
    "qty": "0.001100000000000000",
    "time": 0
  }
]
```







## Old trade lookup (MARKET_DATA)

`GET /api/v1/spot/historicalTrades`


**Weight:**
5

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 200; max 500.
fromId | LONG | NO |TradeId to fetch from. Default gets most recent trades.

>**Response**  

```javascript
[
  {
    "id": "3489",
    "price": "0.00001457",
    "qty": "6",
    "time": 1551129538000,
    "buyerMaker": false
  },
  {
    "id": "3488",
    "price": "0.00001464",
    "qty": "2",
    "time": 1551129500000,
    "buyerMaker": true
  }
]
```


## Kline/Candlestick data

`GET /api/v1/spot/klines`

Kline/candlestick bars for a symbol. Klines are uniquely identified by their open time.

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
interval | ENUM | YES | "1m":minute"3m": minute"5m": minute"15m":minute"30m":minute"1h":hour"2h":hour"4h":hour"6h":"12h":hour"1d":day"3d":day"1w":week
startTime | LONG | NO |
endTime | LONG | NO |
limit | INT | NO | Default 500; max 1000.

* If startTime and endTime are not sent, the most recent klines are returned.

>**Response**  

```javascript
[
  [
    1499040000000,      // Open time
    "0.01634790",       // Open
    "0.80000000",       // High
    "0.01575800",       // Low
    "0.01577100",       // Close
    "148976.11427815",  // Volume
    1499644799999,      // Close time
    "2434.19055334",    // Quote asset volume
    308,                // Number of trades
    "1756.87402397",    // Taker buy base asset volume
    "28.46694368",      // Taker buy quote asset volume
    "17928899.62484339" // Ignore
  ]
]
```









## Current average price

`GET /api/v1/spot/avgPrice`

**Weight:**
1
**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |

>**Response**  

```javascript
{
  "mins": 5,
  "price": "9.35751834"
}
```







## 24hr ticker price change statistics

`GET /api/v1/spot/ticker/24hr`

24 hour rolling window price change statistics. Careful when accessing this with no symbol.

**Weight:**
1 for a single symbol; **40** when the symbol parameter is omitted


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |


>**Response**  

```javascript
{
  "symbol": "BNBBTC",
  "priceChange": "1.00000000",
  "priceChangePercent": "50.00000000",
  "weightedAvgPrice": "1.88",
  "lastPrice": "3.00000000",
  "bidPrice": "1.00000000",
  "bidQty": "5.00000000",
  "askPrice": "3.00000000",
  "askQty": "9.00000000",
  "openPrice": "2.00000000",
  "highPrice": "18.00000000",
  "lowPrice": "1.00000000",
  "volume": "8.00000000",
  "quoteVolume": "15.00000000",
  "openTime": 1551174049782,
  "closeTime": 1551173957144
}
```
>OR

```javascript
[
  {
    "symbol": "BNBBTC",
    "priceChange": "1.00000000",
    "priceChangePercent": "50.00000000",
    "weightedAvgPrice": "1.88",
    "lastPrice": "3.00000000",
    "bidPrice": "1.00000000",
    "bidQty": "5.00000000",
    "askPrice": "3.00000000",
    "askQty": "9.00000000",
    "openPrice": "2.00000000",
    "highPrice": "18.00000000",
    "lowPrice": "1.00000000",
    "volume": "8.00000000",
    "quoteVolume": "15.00000000",
    "openTime": 1551174049782,
    "closeTime": 1551173957144
  }
]
```










## Price ticker for spot

`GET /api/v1/spot/ticker/price`

Back to newest price

**Weight:**
1 for a single symbol; **2** when the symbol parameter is omitted


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |

* If the symbol is not sent, prices for all symbols will be returned in an array.

>**Response**  

```javascript
{
  "symbol": "LTCBTC",
  "price": "4.00000200"
}
```
>OR

```javascript
[
  {
    "symbol": "LTCBTC",
    "price": "4.00000200"
  },
  {
    "symbol": "ETHBTC",
    "price": "0.07946600"
  }
]
```








## Symbol order book ticker for spot

`GET /api/v1/spot/ticker/bookTicker`

Best price/qty on the order book for a symbol or symbols.

**Weight:**
1 for a single symbol; **2** when the symbol parameter is omitted

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |

* If the symbol is not sent, prices for all symbols will be returned in an array.

>**Response**  

```javascript
{
  "symbol": "LTCBTC",
  "bidPrice": "4.00000000",//Best price for buy
  "bidQty": "431.00000000",//QTY
  "askPrice": "4.00000200",//Best price for sell
  "askQty": "9.00000000"//QTY
}
```
>OR

```javascript
[
  {
    "symbol": "LTCBTC",
    "bidPrice": "4.00000000",
    "bidQty": "431.00000000",
    "askPrice": "4.00000200",
    "askQty": "9.00000000"
  },
  {
    "symbol": "ETHBTC",
    "bidPrice": "0.07946700",
    "bidQty": "9.00000000",
    "askPrice": "100000.00000000",
    "askQty": "1000.00000000"
  }
]
```





# Account endpoints
## Place order in coins transaction（TRADE）

`POST /api/v1/spot/order  (HMAC SHA256)`

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
side | ENUM | YES |
type | ENUM | YES |  `LIMIT`
quantity | DECIMAL | YES |
price | DECIMAL | YES |
newOrderRespType | ENUM | NO | Specify response type   `ACK`, `RESULT`; Default is `ACK`. 
recvWindow | LONG | NO |
timestamp | LONG | YES |



Two choices for newOrderRespType

**Response ACK:**
Returning speed is fast, trading information not included, less information 

>**Response**  

```javascript
{
  "symbol": "JEXBTC",
  "orderId": 28,
  "transactTime": 1507725176595
}
```

**Response RESULT:**
Returning speed is slow, returning some information on taking order transaction

>**Response**  

```javascript
{
  "symbol": "JEXBTC",
  "orderId": 2208,
  "transactTime": 1551184078060,
  "price": "0.00001464",
  "origQty": "1",
  "executedQty": "0",
  "cummulativeQuoteQty": "0.00000000",
  "status": "NEW",
  "timeInForce": "GTC",
  "type": "LIMIT",
  "side": "BUY"
}
```








## Test placing order API of coins transaction(TRADE)

`POST /api/v1/spot/order/test (HMAC SHA256)`

Used for test placing order request, won’t be submitted to matchmaking trading engine

**Weight:**
1

**Parameters:**

Reference   `POST /api/v1/spot/order`


>**Response**  

```javascript
{}
```







## Check orders of coins transaction(USER_DATA)

`GET /api/v1/spot/order (HMAC SHA256)`

Check order status

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
orderId | LONG | YES |
recvWindow | LONG | NO |
timestamp | LONG | YES |



>**Response**  

```javascript
{
  "symbol": "JEXBTC",
  "orderId": "2208",
  "price": "0.00001464",
  "origQty": "1.00000000",
  "executedQty": "1.00000000",
  "cummulativeQuoteQty": "0.00001464",
  "status": "FILLED",
  "timeInForce": "GTC",
  "type": "LIMIT",
  "side": "BUY",
  "time": 1551184037000,
  "updateTime": 1551184037000,
  "working": true
}
```








## Cancel order for coins transaction(TRADE) 

`DELETE /api/v1/spot/order  (HMAC SHA256)`


**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
orderId | LONG | YES |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
{
  "symbol": "JEXBTC",
  "orderId": 75,
  "transactTime": 1551238738296,
  "price": "0.00001407",
  "origQty": "4",
  "executedQty": "0",
  "cummulativeQuoteQty": "0.00000000",
  "status": "CANCELED",
  "timeInForce": "GTC",
  "type": "LIMIT",
  "side": "BUY"
}
```






## Check entry orders of coins transaction of this account(USER_DATA)

`GET /api/v1/spot/openOrders  (HMAC SHA256)`

**Weight:**  
 5


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
orderId | LONG | NO | Only orders after this orderID will be returned. Only partial recent orders will be returned
startTime | LONG | NO |
endTime | LONG | NO |
limit | INT | NO | Default 500; max 500.
recvWindow | LONG | NO |
timestamp | LONG | YES |


>**Response**  

```javascript
[
  {
    "symbol": "JEXBTC",
    "orderId": "76",
    "price": "0.00001406",
    "origQty": "5.00000000",
    "executedQty": "0.00000000",
    "cummulativeQuoteQty": "0.00000000",
    "status": "NEW",
    "timeInForce": "GTC",
    "type": "LIMIT",
    "side": "BUY",
    "time": 1550659007000,
    "updateTime": 1550659007000,
    "working": true
  }
]
```












## Account information(USER_DATA)

`GET /api/v1/account (HMAC SHA256)`

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
{
    "updateTime": 1523093528000
    "contractBalances": [
        {
            "asset": "usdt",
            "canDeposit": false,
            "canTrade": true,
            "canWithdraw": false,
            "free": "8144.0412",
            "locked": "1855.3288"
        }
    ],
    "optionBalances": [
        {
            "asset": "BCXBTC01",
            "canDeposit": false,
            "canTrade": true,
            "canWithdraw": false,
            "free": "0",
            "locked": "0"
        }
    ],
    "spotBalances": [
        {
            "asset": "BTC",
            "canDeposit": true,
            "canTrade": true,
            "canWithdraw": false,
            "free": "999616.1363",
            "locked": "0.7219"
        }
    ],
}
```


## Historical coins entry order of the account (USER_DATA) 

`GET /api/v1/spot/historyOrders  (HMAC SHA256)`

Obtain trading history of specified trading pair

**Weight:**
5

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
orderId | LONG | NO |Only orders after this orderID will be returned. Only partial recent orders will be returned
startTime | LONG | NO |
endTime | LONG | NO |
limit | INT | NO | Default 500; max 500.
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
[
  {
    "symbol": "JEXBTC",
    "orderId": "15",
    "price": "0.00001490",
    "origQty": "3.00000000",
    "executedQty": "3.00000000",
    "cummulativeQuoteQty": "0.00004470",
    "status": "FILLED",
    "timeInForce": "GTC",
    "type": "LIMIT",
    "side": "BUY",
    "time": 1550643800000,
    "updateTime": 1550644851000,
    "working": true
  }
]
```

# User data

## User’s assets (USER_DATA)

`GET /wapi/v1/assetDetail`

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
recvWindow |	LONG |	NO	
timestamp |	LONG |	YES	

>**Response**  

```javascript
{
  "ADC": {
    "minWithdrawAmount": 0,
    "withdrawFee": "0",
    "depositStatus": true,
    "withdrawStatus": false
  },
  "ATT": {
    "minWithdrawAmount": 10,
    "withdrawFee": "[0.01]",
    "depositStatus": false,
    "withdrawStatus": false
  }
}
```


## Depositing address of the user(USER_DATA)

`GET /wapi/v1/depositAddress`


**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset |	STRING |	YES	
recvWindow |	LONG |	NO	
timestamp |	LONG |	YES	

>**Response**  

```javascript
{
  "address": "chongzhidizhichongzhidizhi",
  "asset": "BTC",
  "success": true
}
```



## Historical depositing records of the user (USER_DATA)

`GET /wapi/v1/depositHistory`

**Weight:**
5

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset |	STRING |	NO	|
status |	int |	NO	| 0:pending,1:success
startTime | LONG | NO |
endTime | LONG | NO |
recvWindow |	LONG |	NO	
timestamp |	LONG |	YES	

>**Response**  

```javascript
[
  {
    "insertTime": 1530096910000,
    "amount": 1,
    "asset": "BTC",
    "address": "chongzhidizhichongzhidizhi",
    "txId": "g3jrglkelmrioqnfmkcmxcmkllfemfkmfmseme",
    "status": 2
  },
  {
    "insertTime": 1530096910000,
    "amount": 1,
    "asset": "BTC",
    "address": "chongzhidizhichongzhidizhi",
    "txId": "gjrglkel4mrioqnfmkcmxcmkllfemfkmfmseme",
    "status": 2
  }
]

```

## Service fee of the trading (USER_DATA)

`GET /wapi/v1/tradeFee`

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol |	STRING |	NO	|
recvWindow |	LONG |	NO	
timestamp |	LONG |	YES	

>**Response**  

```javascript
[
  {
    "symbol": "DASHUSDT",
    "maker": -0.001,
    "taker": -0.001
  },
  {
    "symbol": "BTCUSDT",
    "maker": -0.1,
    "taker": -0.1
  }
]
```

## Historical withdrawal records of the user(USER_DATA)

`GET /wapi/v1/withdrawHistory`

**Weight:**
5

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset |	STRING |	NO	|
status |	INT |	NO	| 0:Email Sent,1:Cancelled 2:Awaiting Approval 3:Rejected 4:Processing 5:Failure 6Completed
startTime | LONG | NO |
endTime | LONG | NO |
recvWindow |	LONG |	NO	
timestamp |	LONG |	YES	

>**Response**  

```javascript
{
  "withdrawRecordList": [
    {
      "id": "20F13F5291908F81",
      "amount": 1,
      "address": "swswsdgsdwesdfwsdw",
      "asset": "BTC",
      "txId": "g3jrglkelmrioqnfmkcmxcmkllfemfkmfmseme",
      "applyTime": 1551339300000,
      "status": 4
    },
    {
      "id": "AAEAAECD5F890E47",
      "amount": 1,
      "address": "swswsdgsdwesdfwsdw",
      "asset": "BTC",
      "txId": "g3jrglkelmrioqnfmkcmxcmkllfemfkmfmseme",
      "applyTime": 1551340680000,
      "status": 4
    }
  ],
  "success": true
}
```
