---
title: API Reference

language_tabs: 


toc_footers:
  - <a href='https://testnet.jexzh.com/cn/contract'>Binance jex Futures</a>

includesf:
  - BaseInfo_TESTNET
  - CHANGELOG
includes:
  - userDataStream_TESTNET
  - web-socket-streams_TESTNET
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



## Exchange information for futures

`GET /api/v1/contractInfo`

Get the current trading rules and symbol information

**Weight:**
1

**Parameters:**
NONE

>**Response**  

```javascript
[
  {
    "fundingRate": 0.000002,
    "fundingInterval": 600000,
    "nextFundingTime": 1551337200000,
    "predictedFundingRate": 0.000002,
    "turnover24": 212921.712,
    "markMethod": 2,
    "markPrice": 2300.0046,
    "lastPrice": 2300,
    "autoDeleveraging": "DISABLE",
    "symbol": "BTCUSDT"
  }
]
```



## Depth information for futures

`GET /api/v1/contract/depth`

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

## Recent trades for futures

`GET /api/v1/contract/trades`

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


## Old trade lookup for futures (MARKET_DATA)

`GET /api/v1/contract/historicalTrades`

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


## Kline data lookup for futures

`GET /api/v1/contract/klines`

**Weight:1**  
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




## Look up current average price for futures

`GET /api/v1/contract/avgPrice`

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



## Look up 24hr ticker price change statistics for futrues

`GET /api/v1/contract/ticker/24hr`

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



## Look up price ticker for futures

`GET /api/v1/contract/ticker/price`

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


## Look up Symbol order book ticker for futures

`GET /api/v1/contract/ticker/bookTicker`

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

## Index price and mark price for futures

`GET /api/v1/contract/ticker/indicesPrice`

Best price/qty on the order book for a symbol or symbols.

**Weight:**
1


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |

* If the symbol is not sent, prices for all symbols will be returned in an array.

>**Response**  

```javascript
{
  "symbol": "BTCUSDT",
  "price": "2600.0000",
  "indexPrice": "2300.0000",
  "markPrice": "2300.0023"
}
```
>OR

```javascript
[
  {
    "symbol": "EOSUSDT",
    "price": "2301.5491",
    "indexPrice": "2300.0000",
    "markPrice": "2300.7659"
  },
  {
    "symbol": "BTCUSDT",
    "price": "2600.0000",
    "indexPrice": "2300.0000",
    "markPrice": "2300.0023"
  }
]
```

# Account endpoints


## Place order in contract transaction(TRADE)

`POST /api/v1/contract/order  (HMAC SHA256)`

**Weight:**
1
**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
side | ENUM | YES | `BUY` or `SELL`
type | ENUM | YES | `LIMIT`,`stopLimit`,`profitLimit`
quantity | DECIMAL | YES |
price | DECIMAL | YES |
triggerType | ENUM | No | `lastPrice`,`markPrice`,`indexPrice`( Used with `stopLimit`, `profitLimit` orders)
triggerPrice | DECIMAL | No |  Used with `stopLimit`, `profitLimit` orders.
newOrderRespType | ENUM | NO | Specify response type   `ACK`, `RESULT`; Default is `ACK`. 
recvWindow | LONG | NO |
timestamp | LONG | YES |




Additional mandatory parameters based on `type`:

Type | Additional mandatory parameters
------------ | ------------
`LIMIT` | `quantity`, `price`
`stopLimit` | `quantity`, `price`,`triggerType`,`triggerPrice`
`profitLimit` |  `quantity`, `price`,`triggerType`,`triggerPrice`

Other info:

* User can preset the Take profit/Stop command with the trigger price, commission price, and commission quantity in advance. When the corresponding “indicator” satisfies the trigger price set by the user, the order will be triggered, then the system will submit an order according to the price and quantity set by the user


Trigger price of condition sheet must be:

* Take Profit

  * Long: when it's bullish, set the trigger price higher than the latest price (or mark price, index), you can set the sell Take profit order;

  * Short: If it's bearish, set the trigger price below the latest price (or mark price, index), you can set the buy Take profit order;

* Stop

   * Long: when it's bearish, set the trigger price below the latest price (or mark the price, index),  you can set the sell Stop order;

   *  Short: If it's bullish, set the trigger price higher than the latest price (or mark the price, index),you can set the buy Stop order;

*  When the trigger price = the latest price (or mark price, index), or no position, the Take profit/Stop order can not be set;


**Response ACK:**
Returning speed is fast, trading information not included, less information 

>**Response**  

```javascript
{
  "symbol": "BTCUSDT",
  "orderId": "4613019726031880200",
}
```

**Response RESULT:**
Returning speed is fast, returning some information on taking order transaction

>**Response**  

```javascript
{
  "symbol": "BTCUSDT",
  "orderId": "4613019726031880199",
  "side": "buy",
  "origQty": "1.0000",
  "executedQty": "0",
  "price": "3800",
  "status": "entrusting",
  "type": "limit"
}
```



## Test placing order API of contract transaction(TRADE)

`POST /api/v1/contract/order/test (HMAC SHA256)`

Used for test placing order request, won’t be submitted to matchmaking trading engine

**Weight:**
1

**Parameters:**

Reference   `POST /api/v1/contract/order`


>**Response**  

```javascript
{}
```



## Check orders of contract transaction(USER_DATA) 

`GET /api/v1/contract/order (HMAC SHA256)`

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
  "symbol": "BTCUSDT",
  "orderId": "4613019726031880200",
  "updateTime": 1551257935000,
  "side": "BUY",
  "origQty": "1.0000",
  "executedQty": "0.0000",
  "price": "3800",
  "executedPrice": null,
  "status": "",
  "time": 1551257976000,
  "reject": null,
  "type": "LIMIT"
}
```


## Cancel order for contract transaction (TRADE) 

`DELETE /api/v1/contract/order  (HMAC SHA256)`

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
  "symbol": "BTCUSDT",
  "orderId": "4613019726031880200",
  "side": "buy",
  "origQty": "1.00000000000000000000",
  "executedQty": "0.00000000000000000000",
  "price": "3800.00000000000000000000",
  "status": "entrusted",
  "type": "limit"
}
```

## Check entry orders of contract transaction of this account(USER_DATA) 

`GET /api/v1/contract/openOrders  (HMAC SHA256)`

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

## Close positions for contract(TRADE)

`POST /api/v1/contract/liquidation  (HMAC SHA256)`

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
  "symbol": "EOSUSDT",
  "orderId": "4613030721148158029",
  "side": "sell",
  "origQty": "4.0000",
  "status": "entrusting",
  "type": "market"
}
```


## Check orders of closed positions of the account (MARKET_DATA )

`GET /api/v1/contract/liquidationOrder`

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


>**Response**  

```javascript
[
  {
    "symbol": "EOSUSDT",
    "orderId": "2251799813685248011",
    "updateTime": 1551083505000,
    "side": "BUY",
    "origQty": "2.0515",
    "executedQty": "2.0515",
    "price": "2527.9188",
    "executedPrice": "2527.9188",
    "status": "FILLED",
    "time": 1551083505000,
    "reject": null,
    "type": "FLAT"
  }
]
```


## Check contract positions of the account(USER_DATA)

`GET /api/v1/contract/position  (HMAC SHA256)`

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
    "symbol": "EOSUSDT",
    "direction": "longs",
    "leverage": "10.00",
    "currentQuantity": "1.0000",
    "positionMargin": "725.9045",
    "positionCost": "2300.6803",
    "costPrice": "2300.6803",
    "liquidationPrice": "1594.9068",
    "profit": "-1.5161",
    "profitRate": "-0.0020885667",
    "risk": null,
    "initialMargin": "0.1000",
    "maintenanceMargin": "0.0050",
    "buyOrderCost": "0.0000",
    "sellOrderCost": "0.0000",
    "position": null
  }
]
```

## Adjust contract leverage of the account(USER_DATA)

`POST /api/v1/contract/position/leverage  (HMAC SHA256)`

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
leverage | STRING | YES | 
recvWindow | LONG | NO |
timestamp | LONG | YES |


>**Response**  

```javascript
{}
```


## Historical contract entry order of the account (USER_DATA)

`GET /api/v1/contract/historyOrders  (HMAC SHA256)`

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
    "symbol": "BTCUSDT",
    "orderId": "4613012029450485988",
    "updateTime": 1551246891000,
    "side": "BUY",
    "origQty": "2.0000",
    "executedQty": "2.0000",
    "price": "3800",
    "executedPrice": "3800",
    "status": "FILLED",
    "time": 1551185663000,
    "reject": null,
    "type": "LIMIT"
  }
]
```



## Contract bill of the account(USER_DATA)

`GET /api/v1/contract/bill  (HMAC SHA256)`

Contract bill of obtained account

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
fromId | LONG | NO |Only orders after this orderID will be returned. Only partial recent orders will be returned
limit | INT | NO | Default 100; max 100.
startTime | LONG | NO |
endTime | LONG | NO |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
[
  {
    "id": 854797,
    "amount": 0.662,
    "createdDate": "2019-02-27 13:54:46",
    "symbol": "BTCUSDT",
    "type": "positionSize"
  },
  {
    "id": 854875,
    "amount": 1.338,
    "createdDate": "2019-02-27 13:55:32",
    "symbol": "BTCUSDT",
    "type": "positionSize"
  }
]
```



## Contract bill of the account(USER_DATA)

`GET /api/v1/contract/userHistoricalTrades  (HMAC SHA256)`

Contract bill of obtained account

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
endId | LONG | NO |Only orders after this orderID will be returned
limit | INT | NO | Default 1000; max 1000.
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
[
  {
    "id": 854797,
    "orderId":"4612604110637448048",
    "price":"10000.0",
    "qty":"-0.1000",
    "time":1569575228000
    "feeRate":"0.00000",
    "buyerMaker":false

  },
  {
    "id":"461732",
    "orderId":"4612604110637448048",
    "price":"10000.0",
    "qty":"-1.0000",
    "time":1569575147000,
    "feeRate":"0.00000",
    "buyerMaker":false
  }
]
```


## cancel all orders(USER_DATA)

`DELETE /api/v1/contract/batchOrder`


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
ordersJsonArray | String | YES | json String (Request 1 time in 1 second, up to 10 orders in 1 time)
recvWindow | LONG | NO |
timestamp | LONG | YES |

**ordersJsonArray json String rule example：**

`[{"symbol":"btcusdt","orderId":"4612172002566865072"},{"symbol":"btcusdt","orderId":"4612170903055237327"},{"symbol":"EOSUSDT","orderId":"4612170903055237327"},{"symbol":"ETHUSDT","orderId":"4612168704031981750"}]`

>**Response**  

``` javascript
[{
	"symbol": "BTCUSDT",
	"orderId": "4612172002566865072",
	"updateTime": 1570696952000,
	"side": "buy",
	"origQty": "1.0000",
	"executedQty": "0.0000",
	"price": "7548.7",
	"executedPrice": "0.0",
	"status": "entrusted",
	"time": 1570696952000,
	"type": "limit"
}, {
	"msg": "Order does not exist.",
	"code": -2013
}, {
	"symbol": "EOSUSDT",
	"orderId": "4612170903055237327",
	"updateTime": 1570697170000,
	"side": "buy",
	"origQty": "1.0",
	"executedQty": "0.0",
	"price": "0.136",
	"executedPrice": "0.000",
	"status": "entrusted",
	"time": 1570697170000,
	"type": "limit"
}, {
	"symbol": "ETHUSDT",
	"orderId": "4612168704031981750",
	"updateTime": 1570697224000,
	"side": "buy",
	"origQty": "1.00",
	"executedQty": "0.00",
	"price": "90.96",
	"executedPrice": "0.00",
	"status": "entrusted",
	"time": 1570697224000,
	"type": "limit"
}]
```
**Precautions and instructions:**

- Signature and request notes:
  
  1. First Signature the parameters according to the general rules
  1. urlEncode on the parameters of ordersJsonArray
  1. Send a delete request

- Restrictions：
  1. Request 1 time at most 1 second
  1. 1 time up to 10 orders



  ## Capital fee rate of contract

`GET /api/v1/contract/historyRate`

Get capital fee of one specified contract

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
fromId | LONG | NO |Only orders after this orderID will be returned. Only partial recent orders will be returned
limit | INT | NO | Default 100; max 100.
startTime | LONG | NO |
endTime | LONG | NO |

>**Response**  

```javascript
[
  {
    "id": 14415,
    "fundingRate": 0.0001,
    "fundingRateDay": 0.0003,
    "symbol": "EOSUSDT",
    "date": 1550833084000
  },
  {
    "id": 14447,
    "fundingRate": 0.0001,
    "fundingRateDay": 0.0003,
    "symbol": "EOSUSDT",
    "date": 1550851200000
  }
]
```


## Contract protection fund

`GET /api/v1/contract/protectionFund `

Get contract protection fund of one specified contract

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
start | LONG | NO |Only orders after this orderID will be returned. Only partial recent orders will be returned
size | INT | NO | Default 100; max 100.
startTime | LONG | NO |
endTime | LONG | NO |

>**Response**  

```javascript
[
  {
    "id": 1879095,
    "createdDate": 1551083545000,
    "symbol": "EOSUSDT",
    "insuranceFund": 218.47009356847536
  },
  {
    "id": 1879097,
    "createdDate": 1551083545000,
    "symbol": "EOSUSDT",
    "insuranceFund": 661.414109510817
  }
]
```


## Transfer Margin(USER_DATA)

`POST /api/v1/contract/transferMargin  (HMAC SHA256)`

The margin transferred to one specified contract from coins account balance

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
amount | STRING | NO |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**Response**  

```javascript
{
  "asset": "USDT",
  "balance": 889486,
  "lockBalance": 1.6
}
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


