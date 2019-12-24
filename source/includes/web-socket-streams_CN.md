# Websocket 行情推送

## websock基本信息

* 本篇所列出的所有wss接口的baseurl为: wss://ws.jex.com 或者 wss://testnetws.jex.com
* 其中 wss://testnetws.jex.com 为模拟环境。
* 所有stream均可以直接访问，或者作为组合streams的一部分。
* 直接访问时URL格式为 /ws/\<streamName\>
* 组合streams的URL格式为 /stream?streams=\<streamName1\>/\<streamName2\>/\<streamName3\>
* 订阅组合streams时，事件payload会以这样的格式封装 {"stream":"\<streamName\>","data":\<rawPayload\>}
* stream名称中所有交易对均为大写
* 每个到stream.jex.com的链接有效期不超过24小时，请妥善处理断线重连。
* 每20秒，服务端会发送ping帧，客户端应当在10分钟内回复pong帧，否则服务端会主动断开链接。允许客户端发送不成对的pong帧(即客户端可以以高于10分钟每次的频率发送pong帧保持链接)。
* wss 的 contract 订单的sell数量统一为负数
* 推荐使用组合streams方式进行连接，每个组合streams可最多订阅10个stream


 **Stream 详细定义**

## Websocket 最近成交

* 每隔100 ms 推送一次，上一秒内所有的逐笔交易，一笔逐笔交易的定义是仅有一个吃单者与一个挂单者相互交易。

### Stream 名称: \<symbol\>@\<tradeType\>



>**响应:**

```javascript
[{
  "e": "spotTrade",     // 事件类型
  "E": 123456789,       // 事件时间
  "s": "JEXBTC",        // 交易对
  "t": 12345,           // 交易ID
  "p": "0.001",         // 成交价格
  "q": "100",           // 成交数量
  "T": 123456785,       // 成交时间
  "m": true,            // 买方是否是做市方。如true，则此次成交是一个主动卖出单，否则是一个主动买入单。
},{...},...
]
```

### tradeType类型说明

- 支持以下三个事件类型
  - spotTrade 现货类型
  - optionTrade 期权类型
  - contractTrade 合约类型

## Websocket K线

* K线stream逐秒推送所请求的K线种类(最新一根K线)的更新

- 订阅Kline需要提供间隔参数，最短为分钟线，最长为周线。支持以下间隔:

  - 1M 1分钟
  - 5M 5分钟
  - 15M 15分钟
  - 30M 30分钟
  - 1H 1小时
  - 4H 4小时
  - 1D 1天
  - 1W 1周

### Stream 名称: \<symbol\>@\<klineType\>_\<interval\>



>**响应:**

```javascript
{
  "e": "spotKline",     // 事件类型
  "E": 123456789,       // 事件时间
  "s": "JEXBTC",        // 交易对
  "k": {
    "t": 123400000,     // 这根K线的起始时间
    "T": 123460000,     // 这根K线的结束时间
    "s": "JEXBTC",      // 交易对
    "i": "1m",          // K线间隔
    "o": "0.0010",      // 这根K线期间第一笔成交价
    "c": "0.0020",      // 这根K线期间末一笔成交价
    "h": "0.0025",      // 这根K线期间最高成交价
    "l": "0.0015",      // 这根K线期间最低成交价
    "v": "1000",        // 这根K线期间成交量
    "q": "1.0000",      // 这根K线期间成交额
  }
}
```

### klineType类型说明

- klineType 支持的类型
  - spotKline
  - optionKline
  - contractKline

## 按Symbol的精简Ticker

* 按Symbol逐秒刷新的24小时精简ticker信息

### Stream 名称: \<symbol\>@\<miniTickerType\>




>**响应:**

```javascript
{
    "e": "24hrSpotMiniTicker",   // 事件类型
    "E": 123456789,              // 事件时间
    "s": "JEXBTC",               // 交易对
    "c": "0.0025",               // 最新成交价格
    "h": "0.0025",               // 24小时内最高成交价
    "l": "0.0010",               // 24小时内最低成交加
    "m": "10000",                // 标记价(只有contractMiniTicker 类型才存在字段)
    "i": "10000",                // 指数价(只有contractMiniTicker 类型才存在字段)
    "v": "10000",                // 成交量
    "q": "18"，                  // 成交额

}

```

### miniTickerType类型说明

- miniTickerType 支持的类型
  - spotMiniTicker
  - optionMiniTicker
  - contractMiniTicker

## 全市场所有Symbol的精简Ticker

* 按Symbol逐秒刷新的24小时精简全市场ticker信息

### Stream名称: !\<miniTickerType\>@arr




>**响应:**


```javascript

[
  {
    //数组每一个元素对应一个交易对，内容与 按Symbol的精简Ticker 相同
  }
]
```

### miniTickerType 类型说明

- miniTickerType 支持的类型
  - spotMiniTicker
  - optionMiniTicker
  - contractMiniTicker

## 按Symbol的完整Ticker

* 按Symbol逐秒刷新的24小时完整ticker信息

### Stream 名称: \<symbol\>@\<tickerType\>




>**响应:**


```javascript
{
  "e": "24hrSpotTicker",  // 事件类型
  "E": 123456789,         // 事件时间
  "s": "JEXBTC",          // 交易对
  "p": "0.0015",          // 24小时价格变化
  "P": "250.00",          // 24小时价格变化（百分比）
  "c": "0.0025",          // 最新成交价格
  "h": "0.0025",          // 24小时内最高成交价
  "l": "0.0010",          // 24小时内最低成交价
  "v": "10000",           // 24小时内成交量
  "q": "18",              // 24小时内成交额
  "m": "10000",           // 标记价(只有contractMiniTicker 类型才存在字段)
  "i": "10000",           // 指数价(只有contractMiniTicker 类型才存在字段)
  "O": 1234567890,        // TODO 0  统计开始时间
  "C": 86400000,          // TODO 3600*24*1000，24小时的毫秒数  统计关闭时间
}
```
## 全市场所有交易对的完整Ticker

* 同上，只是推送所有的交易对

### Stream名称: !\<tickerType\>@arr



>**响应:**


```javascript
[
  {
    //数组每一个元素对应一个交易对，内容与 按Symbol的完整Ticker 相同
  }
]
```
- tickerType 支持的类型
  - spotTicker
  - optionTicker
  - contractTicker


## 有限档深度信息

* 每秒推送有限档深度信息。levels表示几档买卖单信息, 可选 5/10/20档

### Stream 名称: \<symbol\>@\<depthType\>\<levels\>



>**响应:**

```javascript
{
  "bids": [             // 买单
    [
      "0.0024",         // 价
      "10",             // 量
    ]
  ],
  "asks": [             // 卖单
    [
      "0.0026",         // 价
      "100",            // 量
    ]
  ]
}
```
- depthType 支持的类型
  - spotDepth
  - optionDepth
  - contractDepth
- levels 支持的类型
  - 5
  - 10
  - 20

## 增量深度信息stream
* 每秒推送orderbook的变化部分（如果有）

### Stream 名称: \<symbol\>@\<depthType\>





>**响应:**


```javascript
{
  "e": "spotDepthUpdate", // 事件类型
  "s": "JEXBTC",          // 交易对
  "v": 157,               // 推送的版本号
  "b": [                  // 变动的买单深度
    [
      "0.0024",          // 价
      "10",              // 量
    ]
  ],
  "a": [                 // 变动的卖单深度
    [
      "0.0026",          // 价
      "100",             // 量
    ]
  ]
}
```
- depthType 支持的类型
  - spotDepth
  - optionDepth
  - contractDepth
- 增量深度推送的事件类型  
  - spotDepthUpdate
  - optionDepthUpdate
  - contractDepthUpdate


