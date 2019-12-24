# 用户数据流订阅接口
* 此处仅列出如何得到数据流名称及如何维持有效期的接口
* 本篇所列出REST接口的baseurl **https://testnet.jex.com**
* 用于订阅账户数据的 `listenKey` 从创建时刻起有效期为60分钟
* 可以通过`PUT`一个`listenKey`延长60分钟有效期
* `DELETE`一个 `listenKey` 立即关闭当前数据流
* 本篇所列出的websocket接口baseurl: **wss://testnetws.jex.com**
* 其中 **wss://testnetws.jex.com** 为模拟环境。
* 订阅账户数据流的stream名称为 **/ws/\<listenKey\>**
* 每个到stream.jex.com的链接有效期不超过24小时，请妥善处理断线重连。
* 账户数据流的消息**不保证**严格时间序; **请使用 E 字段进行排序**


## 新建用户数据流 (USER_STREAM)

`POST /api/v1/userDataStream`
从创建起60分钟有效

**权重:**
1

**Parameters:**
NONE

>**响应:**

```javascript
{
  "listenKey": "pqia91ma19a5s61cv6a81va65sdf19v8a65a1a5s61cv6a81va65sdf19v8a65a1" //用于订阅的数据流名
}
```

## Keepalive (USER_STREAM)

`PUT /api/v1/userDataStream`
延长用户数据流有效期到60分钟之后。 建议每30分钟调用一次

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
listenKey | STRING | YES

>**响应:**

```javascript
{}
```

## 关闭用户数据流 (USER_STREAM)

`DELETE /api/v1/userDataStream`

**权重:**
1

**参数:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
listenKey | STRING | YES

>**响应:**

```javascript
{}
```




**用户私有数据 User Data Streams**




## 账户更新
* 账户更新事件的 event type 固定为 `accountSpotInfo` AND `accountOptionInfo` AND `accountContractInfo` 分别对应现货、期权、期货的资产事件
当对应资产信息有变动时，会推送相应的事件

>**响应:**

```javascript
{
  "e": "accountSpotInfo",       // 事件类型
  "E": 1499405658849,           // 事件时间
  "m": 0,                       // 挂单费率
  "t": 0,                       // 吃单费率
  "u": 1499405658848,           // 账户末次更新时间戳
  "B": [                        // 余额
    {
      "a": "LTC",               // 资产名称
      "o": "1.57",              // 委托保证金（仅合约资产存在）
      "p":  "0"  ，             // 持仓保证金（仅合约资产存在）
      "f": "17366.18538083",    // 可用余额
      "l": "0.00000000",        // 冻结余额  
      "T": true,                // 是否允许交易
      "W": true,                // 是否允许提现
      "D": true,                // 是否允许充值
    },
    {
      "a": "BTC",
      "f": "10537.85314051",
      "l": "2.19464093",
      "T": true,                // 是否允许交易
      "W": true,                // 是否允许提现
       "D": true,               // 是否允许充值
 },
    {
    "T": true,                  // 是否允许交易
    "W": true,                  // 是否允许提现
    "D": true,                  // 是否允许充值
    "a": "ETH",
    "f": "17902.35190619",
    "l": "0.00000000"
    }
  ]
}
```

## 订单交易更新

* 当有新订单创建、订单有新成交或者新的状态变化时会推送此类事件

event type统一为 `execSpotReport` AND `execOptionReport` AND `execContractReport` 分别对应现货、期权、合约的事件类型

具体内容需要读取 `x`字段 判断执行类型

### 现货和期权的Playload:

>**响应:**

```javascript
{
  "E": 1499405658849,            // 事件时间
  "e": "execSpotReport",         // 事件类型
  "s": "JEXBTC",                 // 交易对
  "S": "BUY",                    // 订单方向
  "q": "1.00000000",             // 订单原始数量
  "p": "0.10264410",             // 订单原始价格
  "X": "NEW",                    // 订单的当前状态
  "i": 4293153,                  // 订单ID
  "z": "0.00000000",             // 订单累计已成交数量
  "O": 1499405658657,            // 订单创建时间
  "Z": "0.00000000",             // 订单累计已成交金额
}
```
#### 现货和期权可能的执行类型(X字段):


* NEW 新建订单
* PARTIALLY_FILLED  部分成交
* FILLED  全部成交
* CANCELED  已撤销
* FAIL 下单失败
* CANCLEFILLED 撤单完成





>**响应:**

```javascript
{
  "E": 1499405658849,            // 事件时间
  "e": "execContractReport",     // 事件类型
  "s": "ETHBTC",                 // 交易对           (被拒绝时不存在)
  "X": "NEW",                    // 订单的当前状态   
  "r": "NONE",                   // 订单被拒绝的原因  (被拒绝才会存在)
  "i": 4293153,                  // 订单ID
  "z": "0.00000000",             // 订单累计已成交数量 (被拒绝时不存在)
  "Z": "0.00000000",             // 订单累计已成交金额 (被拒绝时不存在)
}
```

#### 合约可能的执行类型(X字段):

* ENTRUSTED 下单完成
* ENTRUSTING 下单中
* FAIL 下单失败
* PARTFILLED 部分成交
* FILLED 订单完成
* CANCEL 订单取消

#### 被拒绝的例子:

>**响应:**

``` javascript
{
  E: 1571225843289
  X: "FAIL"
  e: "execContractReport"
  i: "4612616205276108403"
  r: "insufficient Available"  
}
```
####  失败的原因(R字段)
  * insufficient Available
  * too many pending requests
  * position is being liquidated
  * position is being closed
  * closing empty position
  * closing order is enough
  * contract is disabled
  * reach liquidation price
  * reach risk limit



## 合约持仓

* 当合约持仓有变化时，信息推送
* event type 为 contractPositions



>**响应:**

```javascript
{
	"E": 1553050064078,
	"e": "contractPositions",
	"p": [{
		"M": "100", // 最大可用杠杆
		"R": "3903.99060000", // 已用风险限额
		"S": "0", // sell委托价值
		"a": "20", // 强减顺序
		"b": "0", // buy委托价值
		"c": "1301.33020000", //开仓价格
		"d": "longs", //方向
		"i": "0.05", //初始保证金
		"l": "-49.9759", //平仓价格
		"m": "0.005", //维持保证金
		"n": "0", //当前委托价值
		"o": "4088.0779300000", //保证金
		"q": "3.0000",//持仓数量
		"r": "900000",//当前风险值
		"s": "EOSUSDT",//交易对
		"t": "3903.99060000", //持仓成本价值
		"v": "20.00"//杠杆
	}]
}

```
