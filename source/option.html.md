---
title: API Reference

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

## Exchange information for options

`GET /api/v1/optionInfo`

Get the current trading rules and symbol information

**Weight:**
1

**Parameters:**
NONE

>**Response**  

```javascript
[
  {
    "symbol": "BTCCALLM",
    "status": "TRADING",
    "type": "LONGS",
    "exertionTime": 1551344400000,
    "optionDesc": "www.jex.com",
    "canRelease": true,
    "releaseOption": {
      "convertRatio": 0.1,
      "assetMarginName": "BTC",
      "rateMargin": 0.1,
      "rateBase": 1,
      "assetBaseName": "JEX"
    }
  },
  {
    "symbol": "BTCPUTM",
    "status": "TRADING",
    "type": "SHORTS",
    "exertionTime": 1551172087000,
    "optionDesc": "www.jex.com",
    "canRelease": false,
    "releaseOption": null
  }
]
```

## Depth information for options

`GET /api/v1/option/depth`

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



## Recent trades for options

`GET /api/v1/option/trades`

**Weight:1**  
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


## Old trade lookup for options (MARKET_DATA)

`GET /api/v1/option/historicalTrades`

**Weight:5**  
**Parameters:**  

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 200; max 500.
fromId | LONG | NO | TradeId to fetch from. Default gets most recent trades
  
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


## Kline data lookup for options

`GET /api/v1/option/klines`

**Weight:1**  
**Parameters:**  

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
interval | ENUM | YES | "1m":minute"3m": minute"5m": minute"15m":minute"30m":minute"1h":hour"2h":hour"4h":hour"6h":"12h":hour"1d":day"3d":day"1w":week
startTime | LONG | NO |
endTime | LONG | NO |
limit | INT | NO | Default 500; max 1000.

* 缺省返回最近的数据
  
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



## Look up current average price for options

`GET /api/v1/option/avgPrice`
**Weight:1**  
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


## Look up 24hr ticker price change statistics for options

`GET /api/v1/option/ticker/24hr`
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
  "closeTime": 1551173957144，
  "exertionTime": 1574841600000,    //Expiry Date
  "performPrice": "0.0000",         //Performance Price
  "referencePrice": "9279.5200",    //Spot Price
  "exertionPrice": "7958.7100"      //Strike Price
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
    "closeTime": 1551173957144，
    "exertionTime": 1574841600000,    //Expiry Date
    "performPrice": "0.0000",         //Performance Price
    "referencePrice": "9279.5200",    //Spot Price
    "exertionPrice": "7958.7100"      //Strike Price
  }
]
```


## Look up price ticker for options

`GET /api/v1/option/ticker/price`

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

## Look up Symbol order book ticker for options

`GET /api/v1/option/ticker/bookTicker`

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

## Place order in options transaction（TRADE）  (TRADE)

`POST /api/v1/option/order  (HMAC SHA256)`

**Weight:**  
1  
**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
side | ENUM | YES |
type | ENUM | YES | `LIMIT`
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

## Test placing order API of options transaction(TRADE) 

`POST /api/v1/option/order/test (HMAC SHA256)`

Used for test placing order request, won’t be submitted to matchmaking trading engine

**Weight:**
1

**Parameters:**

Reference   `POST /api/v1/option/order`


>**Response**  

```javascript
{}
```


## Check orders of options transaction(USER_DATA) 

`GET /api/v1/option/order (HMAC SHA256)`

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
  "symbol": "BTCCALLM",
  "orderId": "61292",
  "price": "2.00",
  "origQty": "1.00",
  "executedQty": "1.00",
  "cummulativeQuoteQty": "2.00",
  "status": "FILLED",
  "timeInForce": "GTC",
  "type": "LIMIT",
  "side": "BUY",
  "time": 1551184924000,
  "updateTime": 1551234873000,
  "working": true
}
```


## Cancel order for options transaction(TRADE) 

`DELETE /api/v1/option/order  (HMAC SHA256)`


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
  "symbol": "BTCCALLM",
  "orderId": 61299,
  "transactTime": 1551239198405,
  "price": "2.00",
  "origQty": "1",
  "executedQty": "0",
  "cummulativeQuoteQty": "0.00",
  "status": "CANCELED",
  "timeInForce": "GTC",
  "type": "LIMIT",
  "side": "BUY"
}
```

## Check entry orders of options transaction of this account (USER_DATA) 

`GET /api/v1/option/openOrders  (HMAC SHA256)`

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


## Historical options entry order of the account (USER_DATA)

`GET /api/v1/option/historyOrders  (HMAC SHA256)`

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
    "symbol": "BTCCALLM",
    "orderId": "7",
    "price": "0.02",
    "origQty": "2.00",
    "executedQty": "0.00",
    "cummulativeQuoteQty": "0.00",
    "status": "CANCELED",
    "timeInForce": "GTC",
    "type": "LIMIT",
    "side": "BUY",
    "time": 1550564707000,
    "updateTime": 1551085162000,
    "working": true
  }
]
```


## Users short options(TRADE)

`POST /api/v1/option/release  (HMAC SHA256)`

Short options of a specified trading pair

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
amount | INT | YES |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
{
  "assetMargin": "BTC",
  "assetBase": "JEX",
  "assetOption": "BTCCALLM",
  "releaseAmount": "17780",
  "backAmount": "0",
  "marginAmount": "177.8000",
  "baseAmount": "17780.000",
  "availableMargin": "999616.1163",
  "availableBase": "267400.836",
  "availableOption": "17785",
  "createdDate": 1551428811520
}
```


## Users call options(TRADE) 

`POST /api/v1/option/back  (HMAC SHA256)`

Users call options of a specified trading pair

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
amount | INT | YES |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
{
  "assetMargin": "BTC",
  "assetBase": "JEX",
  "assetOption": "BTCCALLM",
  "releaseAmount": "17780",
  "backAmount": "2",
  "marginAmount": "177.7800",
  "baseAmount": "17778.000",
  "availableMargin": "999616.1363",
  "availableBase": "267402.836",
  "availableOption": "17783",
  "createdDate": 1551668068964
}
```



## Options positions of the account(USER_DATA)

`GET /api/v1/option/position  (HMAC SHA256)`

Get all the positions of one specified options trading pair

**Weight:**
2

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
[
  {
    "name": "BTCCALLM",
    "amount": "5",
    "availAmount": "5",
    "profit": "15.99999996",
    "profitRate": "1.14285714",
    "cost": "2.80",
    "type": "CALL",
    "execTime": 1553763600000,
    "execrPrice": "5.00",
    "orderIndex": 0
  }
]
```



## Short & Call options information of the account(USER_DATA)

`GET /api/v1/option/info  (HMAC SHA256)`

Get the the short & call information of one option of the account 

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
{
  "assetMargin": "BTC",
  "assetBase": "JEX",
  "assetOption": "BTCCALLM",
  "releaseAmount": "17780",
  "backAmount": "2",
  "marginAmount": "177.78",
  "baseAmount": "17778.00",
  "availableMargin": "999616.1363",
  "availableBase": "267402.836",
  "availableOption": "17783"
}
```


## Historical records of Short & Call options of the account (USER_DATA)

`GET /api/v1/option/record  (HMAC SHA256)`

Short & Call Records of one specified options or all options of the account

**Weight:**
5

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
startTime | LONG | NO |
endTime | LONG | NO |
fromId | LONG | NO |Only orders after this orderID will be returned. Only partial recent orders will be returned
limit | INT | NO | Default 500; max 500.
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
[
  {
    "createdDate": 1551668069000,
    "name": "BTCCALLM",
    "status": 2,
    "amount": "2",
    "convertRatio": "0.10000000",
    "rateMargin": "0.10",
    "baseAmount": "2.000",
    "marginAmount": "0.0200",
    "assetMargin": "BTC",
    "assetBase": "JEX",
    "id": 21821
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

