---
title: API Reference

language_tabs: 


toc_footers:
  - <a href='https://testnet.jexzh.com/cn/contract'>Binance jex Futures</a>

includesf:
  - BaseInfo_CN
  - CHANGELOG_CN
includes:
  - userDataStream_CN
  - web-socket-streams_CN
  - errors_CN




search: true
---
# 行情接口





## 测试服务器连通性

`GET /api/v1/ping`

测试能否联通

**权重:**
1

**参数:**
NONE

> **响应:**

```javascript
{}
```

## 获取服务器时间

`GET /api/v1/time`


获取服务器时间

**权重:**
1

**参数:**
NONE

> **响应:**

```javascript

{
  "serverTime": 1499827319595
}
```

## 交易对信息

`GET /api/v1/exchangeInfo`

获取此时的限制信息和交易对信息

**权重:**
1

**Parameters:**
NONE

> **响应:**

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

## 期权交易对信息
`GET /api/v1/optionInfo`
获取此时的限制信息和交易对信息

**权重:**
1

**Parameters:**
NONE

> **响应:**

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




## 期权深度信息

`GET /api/v1/option/depth`
**权重:1**  
**参数:**  

名称 | 类型 | 是否必须 | 描述  
------------ | ------------ | ------------ | ------------  
symbol | STRING | YES |  
limit | INT | NO | 默认 60; 最大 60. 可选值:[5, 10, 20, 50, 60]  

>  响应: 

```javascript
{
  "lastUpdateId": 1027024,
  "bids": [
    [
      "4.00000000",     // 价位
      "431.00000000",   // 挂单量
      []                // 请忽略.
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


## 期权近期成交
`GET /api/v1/option/trades`
**权重:1**  
**参数:**  

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 60; max 60.
  
>**响应:**

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




## 查询期权历史成交(MARKET_DATA)

`GET /api/v1/option/historicalTrades`
**权重:5**  
**参数:**  

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 200; max 500.
fromId | LONG | NO | 从哪一条成交id开始返回. 缺省返回最近的成交记录
  
>**响应:**  

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



## 查询期权K线数据

`GET /api/v1/option/klines`

**权重:1**  
**参数:**  

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
interval | ENUM | YES | "1m":分钟"3m":分钟"5m":分钟"15m":分钟"30m":分钟"1h":小时"2h":小时"4h":小时"6h":小时"12h":小时"1d":天"3d":天"1w":星期  
startTime | LONG | NO |
endTime | LONG | NO |
limit | INT | NO | Default 500; max 1000.

* 缺省返回最近的数据
  
>**响应:**  

```javascript
[
  [
    1499040000000,      // 开盘时间
    "0.01634790",       // 开盘价
    "0.80000000",       // 最高价
    "0.01575800",       // 最低价
    "0.01577100",       // 收盘价(当前K线未结束的即为最新价)
    "148976.11427815",  // 成交量
    1499644799999,      // 收盘时间
    "2434.19055334",    // 成交额
    308,                // 成交笔数
    "1756.87402397",    // 主动买入成交量
    "28.46694368",      // 主动买入成交额
    "17928899.62484339" // 请忽略该参数
  ]
]
```


## 查询期权当前平均价格

`GET /api/v1/option/avgPrice`
**权重:1**  
**参数:**  

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |  

>**响应:**  

```javascript
{
  "mins": 5,
  "price": "9.35751834"
}
```


## 查询期权24hr价格变动情况

`GET /api/v1/option/ticker/24hr`

**权重:**  
带symbol为1
不带为40  
**参数:**  

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |
  
>**响应:**  

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
  "exertionTime": 1574841600000,    //行权时间
  "performPrice": "0.0000",         //预估履约价
  "referencePrice": "9279.5200",    //现货参考价
  "exertionPrice": "7958.7100"      //行权价
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
    "exertionTime": 1574841600000,    //行权时间
    "performPrice": "0.0000",         //预估履约价
    "referencePrice": "9279.5200",    //现货参考价
    "exertionPrice": "7958.7100"      //行权价
  }
]
```


## 查询期权最新价格接口

`GET /api/v1/option/ticker/price`
**权重:**  
单交易对1
无交易对2  
**参数:**  

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |

* 不发送交易对参数，则会返回所有交易对信息
  
>**响应:**  

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


## 查询期权最优挂单接口

`GET /api/v1/option/ticker/bookTicker`

返回当前最优的挂单(最高买单，最低卖单)
**权重:**  
单交易对1
无交易对2  
**参数:**  

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |

* 不发送交易对参数，则会返回所有交易对信息  

>**响应:**  

```javascript
{
  "symbol": "LTCBTC",
  "bidPrice": "4.00000000",//最优买单价
  "bidQty": "431.00000000",//挂单量
  "askPrice": "4.00000200",//最优卖单价
  "askQty": "9.00000000"//挂单量
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

# 交易接口

## 期权下单  (TRADE)

`POST /api/v1/option/order  (HMAC SHA256)`

**权重:**  
1  
**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
side | ENUM | YES |
type | ENUM | YES | `LIMIT`
quantity | DECIMAL | YES |
price | DECIMAL | YES |
newOrderRespType | ENUM | NO | 指定响应类型 `ACK`, `RESULT`; 默认为`ACK`. 
recvWindow | LONG | NO |
timestamp | LONG | YES |




关于 newOrderRespType的俩种选择

**Response ACK:**
返回速度快，不包含成交信息，信息量最少

>**响应:**  

```javascript
{
  "symbol": "JEXBTC",
  "orderId": 28,
  "transactTime": 1507725176595
}
```

**Response RESULT:**
返回速度慢，返回吃单成交的少量信息

>**响应:**  

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

## 期权测试下单接口 (TRADE)

`POST /api/v1/option/order/test (HMAC SHA256)`

用于测试订单请求，但不会提交到撮合引擎

**权重:**
1

**参数:**

参考 `POST /api/v1/option/order`


>**响应:**

```javascript
{}
```


## 期权查询订单 (USER_DATA)

`GET /api/v1/option/order (HMAC SHA256)`

查询订单状态

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
orderId | LONG | YES |
recvWindow | LONG | NO |
timestamp | LONG | YES |



>**响应:**

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

## 期权撤销订单 (TRADE)

`DELETE /api/v1/option/order  (HMAC SHA256)`

**权重:**
1

**Parameters:**  

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
orderId | LONG | YES |
recvWindow | LONG | NO |
timestamp | LONG | YES |





>**响应:**

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


## 查看账户当前期权挂单 (USER_DATA)

`GET /api/v1/option/openOrders  (HMAC SHA256)`

**权重:**
5

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
orderId | LONG | NO | 只返回此orderID之后的订单，缺省返回最近的订单
startTime | LONG | NO |
endTime | LONG | NO |
limit | INT | NO | Default 500; max 500.
recvWindow | LONG | NO |
timestamp | LONG | YES |


>**响应:**

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


## 账户期权历史委托 (USER_DATA)

`GET /api/v1/option/historyOrders  (HMAC SHA256)`
获取某交易对的成交历史

**权重:**
5

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
orderId | LONG | NO |返回该orderId之后的成交，缺省返回最近的成交
startTime | LONG | NO |
endTime | LONG | NO |
limit | INT | NO | Default 500; max 500.
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**响应:**

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



## 用户发行期权 (TRADE)


`POST /api/v1/option/release  (HMAC SHA256)`
发行某个交易对的期权

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
amount | INT | YES |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**响应:**

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

## 用户赎回期权 (TRADE)

`POST /api/v1/option/back  (HMAC SHA256)`
赎回某个交易对的期权

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
amount | INT | YES |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**响应:**

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

## 账户期权持仓 (USER_DATA)

`GET /api/v1/option/position  (HMAC SHA256)`
获取某个期权交易对或全部的持仓

**权重:**
2

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**响应:**

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


## 账户期权发行赎回信息 (USER_DATA)

`GET /api/v1/option/info  (HMAC SHA256)`
获取账户某个期权的发行赎回信息

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**响应:**

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

## 账户期权发行赎回记录 (USER_DATA)

`GET /api/v1/option/record  (HMAC SHA256)`
获取账户某个期权或者全部的发行赎回交易记录

**权重:**
5

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
startTime | LONG | NO |
endTime | LONG | NO |
fromId | LONG | NO |返回该orderId之后的成交，缺省返回最近的成交
limit | INT | NO | Default 500; max 500.
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**响应:**

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

## 账户资产信息(USER_DATA)

`GET /api/v1/account (HMAC SHA256)`

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**响应:**

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