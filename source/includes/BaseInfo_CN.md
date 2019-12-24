
# 基本信息
## api 基本信息
* 本篇列出REST接口的baseurl  https://www.jex.com 
* 所有接口的响应都是JSON格式
* 响应中如有数组，数组元素以时间升序排列，越早的数据越提前。
* 所有时间、时间戳均为UNIX时间，单位为毫秒
* HTTP `4XX` 错误码用于指示错误的请求内容、行为、格式。
* HTTP `429` 错误码表示警告访问频次超限，即将被封IP
* HTTP `418` 表示收到429后继续访问，于是被封了。
* HTTP `5XX` 错误码用于指示服务器侧的问题。 
* HTTP `504` 表示API服务端已经向业务核心提交了请求但未能获取响应，特别需要注意的是`504`代码不代表请求失败，而是未知。很可能已经得到了执行，也有可能执行失败，需要做进一步确认。
* 每个接口都有可能抛出异常，异常响应格式如下：

```shell
{
  "code": -1121,
  "msg": "Invalid symbol."
}
```

* 具体的错误码及其解释在  `错误代码汇总`
* `GET`方法的接口, 参数必须在`query string`中发送.
* `POST`, `PUT`, 和 `DELETE` 方法的接口, 参数可以在 `query string`中发送，也可以在 `request body`中发送(content type `application/x-www-form-urlencoded`。允许混合这两种方式发送参数。但如果同一个参数名在query string和request body中都有，query string中的会被优先采用。


## 访问限制
* 在headers 以下字符代表下列意思:
  - SECOND => S
  - MINUTE => M
  - HOUR => H
  - DAY => D
* 在 `/api/v1/exchangeInfo`接口中`rateLimits`数组里包含有REST接口(不限于本篇的REST接口)的访问限制。包括带权重的访问频次限制、下单速率限制、交易对精度。本篇`枚举定义`章节有限制类型的进一步说明。
* 违反上述任何一个访问限制都会收到HTTP 429，这是一个警告.
* 每一个接口均有一个相应的权重(weight)，有的接口根据参数不同可能拥有不同的权重。越消耗资源的接口权重就会越大。
* 当收到429告警时，调用者应当降低访问频率或者停止访问。
* **收到429后仍然继续违反访问限制，会被封禁IP，并收到418错误码**
* 频繁违反限制，封禁时间会逐渐延长，**从最短2分钟到最长3天**.
* Headers `X-MBX-USED-WEIGHT-(intervalNum)(intervalLetter)` 会给你当前的请求权重 (intervalNum)(intervalLetter)频率的限制. 举个例子有分钟请求权重频率的设置, 在请求返回中含有  `X-MBX-USED-WEIGHT-1M `. 请求头header 中`X-MBX-USED-WEIGHT ` 仍然会返回并且已经使用的次数。
* Header 中 `X-MBX-ORDER-COUNT-(intervalNum)(intervalLetter)`在任何有效的订单上更新，并跟踪间隔的当前订单计数; 拒绝/不成功的订单不确保有`X-MBX-ORDER-COUNT-**` headers 在返回值中。
  - 例子. `X-MBX-ORDER-COUNT-1S` 是 "这秒有多少单" and` X-MBX-ORDER-COUNT-1D` 是 ”这一天有多少单“
* 消息头header上`Retry-After`会给出具体需要等待的时间,以秒为单位，直到解除封禁。
* 错误的下单精度不允许下单



**委托数量限制**

* 为了保持有序的市场，jex 就每个账户的待交易委托数量设置上限。这些上限是： 这些限制是：
* 每个账户每个合约最多 200 笔未执行普通交易委托数量;
* 每个账户每个合约最多 10 笔止损或者止盈交易委托数量;
* 当发出超过这些上限的新交易委托时，该交易将被拒绝，并显示“has reach max order number 200或者10”。

**其他说明**

- api的contract订单的sell数量统一为负数

## 接口鉴权类型
* 每个接口都有自己的鉴权类型，鉴权类型决定了访问时应当进行何种鉴权
* 如果需要 API-key，应当在HTTP头中以`X-JEX-APIKEY`字段传递
* API-key 与 API-secret 是大小写敏感的
* 可以在网页用户中心修改API-key 所具有的权限，例如读取账户信息、发送交易指令、发送提现指令

鉴权类型 | 描述
------------ | ------------
NONE | 不需要鉴权的接口
TRADE | 需要有效的API-KEY和签名
USER_DATA | 需要有效的API-KEY和签名
USER_STREAM | 需要有效的API-KEY
MARKET_DATA | 需要有效的API-KEY


## 需要签名的接口 (TRADE 与 USER_DATA)
* 调用这些接口时，除了接口本身所需的参数外，还需要传递`signature`即签名参数。
* 签名使用`HMAC SHA256`算法. API-KEY所对应的API-Secret作为 `HMAC SHA256` 的密钥，其他所有参数作为`HMAC SHA256`的操作对象，得到的输出即为签名。
* 签名大小写不敏感。
* 当同时使用query string和request body时，`HMAC SHA256`的输入query string在前，request body在后
* 签名的参数一定按照接口文档顺序传参

**时间同步安全**

* 签名接口均需要传递`timestamp`参数, 其值应当是请求发送时刻的unix时间戳（毫秒）
* 服务器收到请求时会判断请求中的时间戳，如果是5000毫秒之前发出的，则请求会被认为无效。这个时间窗口值可以通过发送可选参数`recvWindow`来自定义。
* 另外，如果服务器计算得出客户端时间戳在服务器时间的‘未来’一秒以上，也会拒绝请求。
* 逻辑伪代码：

  ```shell
  if (timestamp < (serverTime + 1000) && (serverTime - timestamp) <= recvWindow) {
    // process request
  } else {
    // reject request
  }
  ```

**关于交易时效性** 

互联网状况并不100%可靠，不可完全依赖,因此你的程序本地到服务器的时延会有抖动.
这是我们设置`recvWindow`的目的所在，如果你从事高频交易，对交易时效性有较高的要求，可以灵活设置recvWindow以达到你的要求。
<aside class="notice">
不推荐使用5秒以上的recvWindow
</aside>


**POST /api/v1/spot/order**

以下是在linux bash环境下使用 echo openssl 和curl工具实现的一个调用接口下单的示例
apikey、secret仅供示范

Key | Value
------------ | ------------
apiKey | vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A
secretKey | NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j


参数 | 取值
------------ | ------------
symbol | LTCBTC
side | BUY
type | LIMIT
timeInForce | GTC
quantity | 1
price | 0.1
recvWindow | 5000
timestamp | 1499827319559


**示例 1: 所有参数通过 query string 发送**

* **queryString:** symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559
* **HMAC SHA256 签名:**

```shell
[linux]$ echo -n "symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559" | openssl dgst -sha256 -hmac "NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j"
(stdin)= c8db56825ae71d6d79447849e617115f4a920fa2acdcab2b053c4b2838bd6b71
```


* **curl 调用:**

```shell
(HMAC SHA256)
[linux]$ curl -H "X-JEX-APIKEY: vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A" -X POST 'https://www.jex.com/api/v1/order?symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559&signature=c8db56825ae71d6d79447849e617115f4a920fa2acdcab2b053c4b2838bd6b71'
```

**示例 2: 所有参数通过 request body 发送**

* **requestBody:** symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559
* **HMAC SHA256 签名:**

```shell
[linux]$ echo -n "symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559" | openssl dgst -sha256 -hmac "NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j"
(stdin)= c8db56825ae71d6d79447849e617115f4a920fa2acdcab2b053c4b2838bd6b71
```


>  * **curl 调用:**

```shell
  (HMAC SHA256)
  [linux]$ curl -H "X-JEX-APIKEY: vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A" -X POST 'https://www.jex.com/api/v1/order' -d 'symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559&signature=c8db56825ae71d6d79447849e617115f4a920fa2acdcab2b053c4b2838bd6b71'
 ```

**示例 3: 混合使用 query string 与 request body**

* **queryString:** symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC
* **requestBody:** quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559
* **HMAC SHA256 签名:**

```shell
  [linux]$ echo -n "symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTCquantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559" | openssl dgst -sha256 -hmac "NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j"
  (stdin)= 0fd168b8ddb4876a0358a8d14d0c9f3da0e9b20c5d52b2a00fcf7d1c602f9a77
 ```


* **curl 调用:**

```shell
  (HMAC SHA256)
  [linux]$ curl -H "X-JEX-APIKEY: vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A" -X POST 'https://www.jex.com/api/v1/order?symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC' -d 'quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559&signature=0fd168b8ddb4876a0358a8d14d0c9f3da0e9b20c5d52b2a00fcf7d1c602f9a77'
```

Note that the signature is different in example 3.
There is no & between "GTC" and "quantity=1".




## 公开API参数

**术语解释**

* `base asset` 指一个交易对的交易对象，即写在靠前部分的资产名
* `quote asset` 指一个交易对的定价资产，即写在靠后部分资产名


**枚举定义**

**交易对状态:**

* PRE_TRADING 盘前交易
* TRADING 正常交易中
* POST_TRADING 盘后交易
* END_OF_DAY 收盘
* HALT 交易终止(该交易对已下线)
* AUCTION_MATCH 集合竞价
* BREAK 交易暂停

**交易对类型:**

* SPOT 币币
* OPTION 期权
* CONTRACT 合约

**币币和期权订单状态:**

* NEW 新建订单
* PARTIALLY_FILLED  部分成交
* FILLED  全部成交
* CANCELED  已撤销
* PENDING_CANCEL 正在撤销中(目前不会遇到这个状态)
* FAIL 下单失败
* CANCLEFILLED 撤单完成
* REJECTED 订单被拒绝 (目前不会遇到这个状态)
* EXPIRED 订单过期(目前不会遇到这个状态)

**合约的订单状态:**
 
* ENTRUSTED 下单完成
* ENTRUSTING 下单中
* FAIL 下单失败
* PARTFILLED 部分成交
* FILLED 订单完成
* CANCEL 订单取消


**订单种类:**

* LIMIT 限价单
* MARKET  市价单
* STOPLIMIT 限价止损
* STOPMARKET 市价止损
* PROFITLIMIT 限价止盈
* PROFITMARKET 市价止盈单

**订单方向:**

* BUY 买入
* SELL 卖出

**Time in force:**

* GTC - Good Till Cancel 成交为止
* IOC - Immediate or Cancel 无法立即成交(吃单)的部分就撤销
* FOK - Fill or Kill 无法全部立即成交就撤销

**K线间隔:**

m -> 分钟; h -> 小时; d -> 天; w -> 周; M -> 月

* 1m
* 3m
* 5m
* 15m
* 30m
* 1h
* 2h
* 4h
* 6h
* 12h
* 1d
* 1w

**限制种类 (rateLimitType)**

* REQUESTS_WEIGHT  单位时间请求权重之和上限
* ORDERS    单位时间下单(撤单)次数上限
* RAW_REQUESTS  单位时间请求次数上限

**限制间隔**

* SECOND
* MINUTE
* DAY

**合约账单接口(type)**

* transfer       资金划转
* fee            手续费
* funding        资金费用
* closPosition   平仓
* liquidation    爆仓平仓
* adl            强减平仓
* autoMargin     自动减仓
* positionSize   持仓变化





