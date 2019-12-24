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
```javascript
GET /api/v1/ping
```
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


## 合约交易对信息

`GET /api/v1/contractInfo`

获取此时的限制信息和交易对信息

**权重:**
1

**Parameters:**
NONE

> **响应:**

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


## 合约深度信息
`GET /api/v1/contract/depth`
**权重:1**  
**参数:**  

名称 | 类型 | 是否必须 | 描述
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | 默认 60; 最大 60. 可选值:[5, 10, 20, 50, 60]  
>**响应:**  

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

## 合约近期成交

`GET /api/v1/contract/trades`
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


## 查询合约历史成交(MARKET_DATA)

`GET /api/v1/contract/historicalTrades`
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




## 查询合约K线数据

`GET /api/v1/contract/klines`

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



## 查询合约当前平均价格

`GET /api/v1/contract/avgPrice`
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





## 查询合约24hr价格变动情况

`GET /api/v1/contract/ticker/24hr`
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



## 查询合约最新价格接口

`GET /api/v1/contract/ticker/price`
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

## 查询合约最优挂单接口

`GET /api/v1/contract/ticker/bookTicker`
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


## 合约的指数价格，标记价格

`GET /api/v1/contract/ticker/indicesPrice`
返回当前最优的挂单(最高买单，最低卖单)

**权重:**
1


**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |

* 不发送交易对参数，则会返回所有交易对信息

>**响应:**

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

# 交易接口


## 合约下单  (TRADE)

`POST /api/v1/contract/order  (HMAC SHA256)`

**权重:**
1
**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
side | ENUM | YES |
type | ENUM | YES |`LIMIT`,`stopLimit`,`profitLimit`
quantity | DECIMAL | YES |
price | DECIMAL | YES |
triggerType | ENUM | No | `lastPrice`,`markPrice`,`indexPrice` (仅 `stopLimit`, `profitLimit` 需要此参数)
triggerPrice | DECIMAL | NO | 仅 `stopLimit`, `profitLimit` 需要此参数
newOrderRespType | ENUM | NO | 指定响应类型 `ACK`, `RESULT`; 默认为`ACK`. 
recvWindow | LONG | NO |
timestamp | LONG | YES |



根据 order `type`的不同，某些参数强制要求，具体如下:

Type | 强制要求的参数
------------ | ------------
`LIMIT` | `quantity`, `price`
`stopLimit` | `quantity`, `price`,`triggerType`,`triggerPrice`
`profitLimit` |  `quantity`, `price`,`triggerType`,`triggerPrice`

其他:

* 用户预设止盈止损指令（`stopLimit`,`profitLimit`），提前设置 触发价格（`triggerPrice`）、委托价格(`price`)、委托数量(`quantity`)。当对应 “指标” 满足用户设置的 触发价（`triggerPrice`）格 条件时，止盈止损订单将被触发，系统将按照用户设置的 委托价格(`price`)，委托数量(`quantity`) 提交一笔限价委托订单。


条件单的触发价格必须:

* 止盈

  * 多仓： 对未来价格看涨，设置触发价格高于最新价格（或标记价格、指数），可以设置卖出止盈指令；

  * 空仓： 对未来价格看跌，设置触发价格低于最新价格（或标记价格、指数），可以设置买入止盈指令；

* 止损

  * 多仓： 对未来价格看跌，设置触发价格低于最新价格（或标记价格、指数），可以设置卖出止损指令；

   * 空仓： 对未来价格看涨，设置触发价格高于最新价格（或标记价格、指数），可以设置买入止损指令；

* 当触发价格 = 最新价格（或标记价格、指数），或者无持仓时候，不可发起止盈止损委托指令；





**Response ACK:**
返回速度快，不包含成交信息，信息量最少

>**响应:**  

```javascript
{
  "symbol": "BTCUSDT",
  "orderId": "4613019726031880200",
}
```

**Response RESULT:**

返回速度慢，返回吃单成交的少量信息

>**响应:**  

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



## 合约测试下单接口 (TRADE)

`POST /api/v1/contract/order/test (HMAC SHA256)`

用于测试订单请求，但不会提交到撮合引擎

**权重:**
1

**参数:**

参考 `POST /api/v1/contract/order`


>**响应:**

```javascript
{}
```

## 合约查询订单 (USER_DATA)

`GET /api/v1/contract/order (HMAC SHA256)`

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

## 合约撤销订单 (TRADE)


`DELETE /api/v1/contract/order  (HMAC SHA256)`

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

## 查看账户当前合约挂单 (USER_DATA)

`GET /api/v1/contract/openOrders  (HMAC SHA256)`

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

## 合约平仓 (TRADE)


`POST /api/v1/contract/liquidation  (HMAC SHA256)`

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
  "symbol": "EOSUSDT",
  "orderId": "4613030721148158029",
  "side": "sell",
  "origQty": "4.0000",
  "status": "entrusting",
  "type": "market"
}
```

## 查看账户合约平仓单 (MARKET_DATA)

`GET /api/v1/contract/liquidationOrder`

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


>**响应:**

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

## 查看账户合约仓位 (USER_DATA)


`GET /api/v1/contract/position  (HMAC SHA256)`

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

## 调整账户合约杠杆 (USER_DATA)


`POST /api/v1/contract/position/leverage  (HMAC SHA256)`

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
leverage | STRING | YES | 
recvWindow | LONG | NO |
timestamp | LONG | YES |


>**响应:**

```javascript
{}
```


## 账户合约历史委托 (USER_DATA)


`GET /api/v1/contract/historyOrders  (HMAC SHA256)`
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

## 账户合约账单 (USER_DATA)

`GET /api/v1/contract/bill  (HMAC SHA256)`
获取账户的合约账单

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
fromId | LONG | NO |返回该orderId之后的成交，Default -1(返回最近的成交)
limit | INT | NO | Default 100; max 100.
startTime | LONG | NO |
endTime | LONG | NO |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**响应:**

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

## 获取历史成交信息 (USER_DATA)

`GET /api/v1/contract/userHistoricalTrades  (HMAC SHA256)`
获取历史成交信息

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
endId | LONG | NO |返回该endId之前的成交
limit | INT | NO | Default 1000; max 1000.
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**响应:**

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


## 批量撤单  (TRADE)


`DELETE /api/v1/contract/batchOrder(HMAC SHA256)`

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
ordersJsonArray | String | YES | json 字符串 (1秒最多请求1次,1次最多10单)
recvWindow | LONG | NO |
timestamp | LONG | YES |

**ordersJsonArray json 字符串规则示例：**

`[{"symbol":"btcusdt","orderId":"4612172002566865072"},{"symbol":"btcusdt","orderId":"4612170903055237327"},{"symbol":"EOSUSDT","orderId":"4612170903055237327"},{"symbol":"ETHUSDT","orderId":"4612168704031981750"}]`

>**响应:**

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

**注意事项及说明:**

- 签名及请求注意事项:
  
  1. 先按照通用规则对参数进行签名
  1. 对ordersJsonArray的参数进行urlEncode
  1. 发送delete请求

- 限制事项：
  1. 1秒最多请求1次
  1. 1次最多10单

- 返回值说明:

  1. 如果签名通过，返回的是一个json数组。
  1. 数组对应入参的单
  1. 对应单撤单成功，数组元素为订单信息
  1. 对应单撤单失败，数组元素为一个错误信息




## 批量查询 (USER_DATA)

`GET /api/v1/contract/batchOrder(HMAC SHA256)`


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
ordersJsonArray | String | YES | json 字符串 
recvWindow | LONG | NO |
timestamp | LONG | YES |

**ordersJsonArray json 字符串规则示例：**

`[{"orderId":4611993881683165185},{"orderId":4611993881683165185,"symbol":"btcusdt"},{"orderId":4611993881683165185,"symbol":"eosusdt"},{"orderId":123,"symbol":"btcusdt"}]`


**响应:**

``` javascript

[{
  "symbol":"BTCUSDT",
  "orderId":"4611993881683165185",
  "updateTime":1576749391000,
  "side":"buy",
  "origQty":"1.0000",
  "executedQty":"1.0000",
  "price":"9990.0",
  "executedPrice":"9990.0",
  "status":"filled",
  "time":1576749391000,
  "type":"limit"
  },{
  "symbol":"BTCUSDT",
  "orderId":"4611993881683165185",
  "updateTime":1576749391000,
  "side":"buy",
  "origQty":"1.0000",
  "executedQty":"1.0000",
  "price":"9990.0",
  "executedPrice":"9990.0",
  "status":"filled",
  "time":1576749391000,
  "type":"limit"
  },{
  "msg":"Order does not exist.","code":-2013
  },{
  "msg":"Order does not exist.","code":-2013
  }]

```


**注意事项及说明:**

- 签名及请求注意事项:
  
  1. 先按照通用规则对参数进行签名
  1. 对ordersJsonArray的参数进行urlEncode
  1. 发送get请求
  1. symbol 可以不用传

- 限制事项：
  1. 1分钟1200,5分钟5000
  1. 1次最多100单

- 返回值说明:
  1. 如果签名通过，返回的是一个json数组。
  1. 数组对应入参的单
  1. 对应单查询成功，数组元素为订单信息
  1. 对应单查询失败，数组元素为一个错误信息

## 合约资金费率

`GET /api/v1/contract/historyRate `
获取某合约的资金费率

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
fromId | LONG | NO |返回该fromId之后的记录，Default -1(返回最近的记录)
limit | INT | NO | Default 100; max 100.
startTime | LONG | NO |
endTime | LONG | NO |

>**响应:**

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


## 合约保护基金 

`GET /api/v1/contract/protectionFund`
获取某合约的保护基金

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
start | LONG | NO |返回该start之后的记录，Default -1(返回最近的记录)
size | INT | NO | Default 100; max 100.
startTime | LONG | NO |
endTime | LONG | NO |

>**响应:**

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

## 转移保证金 (USER_DATA)

`POST /api/v1/contract/transferMargin  (HMAC SHA256)`
币币余额转移到某合约的保证金

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
amount | STRING | NO |
recvWindow | LONG | NO |
timestamp | LONG | YES |

>**响应:**

```javascript
{
  "asset": "USDT",
  "balance": 889486,
  "lockBalance": 1.6
}
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
