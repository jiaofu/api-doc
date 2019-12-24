
# General API 

## General API Information

* The base endpoint is:  https://testnet.jex.com
* which the https://testnet.jex.com simulation environment。
* All endpoints return either a JSON object or array.
* Data is returned in ascending order. Oldest first, newest last。
* All time and timestamp related fields are in milliseconds
* HTTP `4XX` return codes are used for malformed requests; the issue is on the sender's side
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `418` return code is used when an IP has been auto-banned for continuing to send requests after receiving `429` codes.
* HTTP `5XX` return codes are used for internal errors; the issue is on
  jex's side.
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


## LIMITS

* New intervalLetter values for headers:
  - SECOND => S
  - MINUTE => M
  - HOUR => H
  - DAY => D
* The `/api/v1/exchangeInfo` `rateLimits` array contains objects related to the exchange's `RAW_REQUEST`, symbol accuracy,`REQUEST_WEIGHT`, and `ORDER` rate limits. These are further defined in the `ENUM definitions` section under `Rate limiters (rateLimitType)`.
* When a 429 is recieved, it's your obligation as an API to back off and not spam the API
* Repeatedly violating rate limits and/or failing to back off after receiving 429s will result in an automated IP ban (http status 418).
* IP bans are tracked and scale in duration for repeat offenders, from 2 minutes to 3 days
* New Headers  `X-MBX-USED-WEIGHT-(intervalNum)(intervalLetter) ` will give your current used request weight for the (intervalNum)(intervalLetter) rate limiter. For example, if there is a one minute request rate weight limiter set, you will get a  `X-MBX-USED-WEIGHT-1M ` header in the response. The legacy header  `X-MBX-USED-WEIGHT ` will still be returned and will represent the current used weight for the one minute request rate weight limit
* New Header `X-MBX-ORDER-COUNT-(intervalNum)(intervalLetter)`that is updated on any valid order placement and tracks your current order count for the interval; rejected/unsuccessful orders are not guaranteed to have `X-MBX-ORDER-COUNT-**` headers in the response。
  - Eg. `X-MBX-ORDER-COUNT-1S` for "orders per 1 second" and `X-MBX-ORDER-COUNT-1D` for orders per "one day"
* A `Retry-After` header is sent with a 418 or 429 responses and will give the number of seconds required to wait, in the case of a 418, to prevent a ban, or, in the case of a 429, until the ban is over.。
* Wrong order accuracy does not allow ordering

**Order Count Limits**
* To keep an orderly market, jex imposes limits on the number of open orders per account. These limits are:
* Maximum 200 open orders per contract per account;
* Maximum 10 stop profit or stop  orders per contract per account;
* When placing a new order that causes these caps to be exceeded, it will be rejected with the message “has reach max order number [200|10]”.



**Other instructions**
The number of contract orders sells for API is uniformly negative

## Endpoint security type
* Each endpoint has a security type that determines the how you will interact with it
* API-keys are passed into the Rest API via the `X-JEX-APIKEY` header
* API-keys and secret-keys are case sensitive
* API-keys can be configured to only access certain types of secure endpoints. For example, one API-key could be used for TRADE only, while another API-key can access everything except for TRADE routes.

Security Type | Description
------------ | ------------
NONE | Endpoint can be accessed freely.
TRADE | Endpoint requires sending a valid API-Key and signature
USER_DATA | Endpoint requires sending a valid API-Key and signature.
USER_STREAM | Endpoint requires sending a valid API-Key.
MARKET_DATA | Endpoint requires sending a valid API-Key.


## SIGNED (TRADE and USER_DATA) Endpoint security

* `SIGNED` endpoints require an additional parameter, `signature`, to be
  sent in the  `query string` or `request body`.
* Endpoints use `HMAC SHA256` signatures. The `HMAC SHA256 signature` is a keyed `HMAC SHA256` operation.
  Use your `secretKey` as the key and `totalParams` as the value for the HMAC operation.
* The `signature` is **not case sensitive**.
* `totalParams` is defined as the `query string` concatenated with the
  `request body`.
* The parameters of signature must be transmitted in the order of interface documents.

**Timing security**
* A `SIGNED` endpoint also requires a parameter, `timestamp`, to be sent which
  should be the millisecond timestamp of when the request was created and sent.
* An additional parameter, `recvWindow`, may be sent to specify the number of
  milliseconds after `timestamp` the request is valid for. If `recvWindow`
  is not sent, **it defaults to 5000**.
* The logic is as follows:
  ```javascript
  if (timestamp < (serverTime + 1000) && (serverTime - timestamp) <= recvWindow) {
    // process request
  } else {
    // reject request
  }
  ```
**Serious trading is about timing.** Networks can be unstable and unreliable,
which can lead to requests taking varying amounts of time to reach the
servers. With `recvWindow`, you can specify that the request must be
processed within a certain number of milliseconds or be rejected by the
server.

<aside class="notice">
**It recommended to use a small recvWindow of 5000 or less! The max cannot go beyond 60,000!**
</aside>



* POST /api/v1/spot/order Examples for POST
Here is a step-by-step example of how to send a vaild signed payload from the
Linux command line using `echo`, `openssl`, and `curl`.

Key | Value
------------ | ------------
apiKey | vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A
secretKey | NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j


Parameters | Value
------------ | ------------
symbol | LTCBTC
side | BUY
type | LIMIT
timeInForce | GTC
quantity | 1
price | 0.1
recvWindow | 5000
timestamp | 1499827319559


### Example 1: As a query string
* **queryString:** symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559
* **HMAC SHA256 signature:**

 ```shell
[linux]$ echo -n "symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559" | openssl dgst -sha256 -hmac "NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j"
(stdin)= c8db56825ae71d6d79447849e617115f4a920fa2acdcab2b053c4b2838bd6b71
```


* **curl command:**

```shell
  (HMAC SHA256)
  [linux]$ curl -H "X-JEX-APIKEY: vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A" -X POST 'https://www.jex.com/api/v1/order?symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559&signature=c8db56825ae71d6d79447849e617115f4a920fa2acdcab2b053c4b2838bd6b71'
```

### Example 2: As a request body
* **requestBody:** symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559
* **HMAC SHA256 signature:**

```shell
[linux]$ echo -n "symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559" | openssl dgst -sha256 -hmac "NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j"
(stdin)= c8db56825ae71d6d79447849e617115f4a920fa2acdcab2b053c4b2838bd6b71
```


* **curl command:**

```shell
(HMAC SHA256)
[linux]$ curl -H "X-JEX-APIKEY: vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A" -X POST 'https://www.jex.com/api/v1/order' -d 'symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559&signature=c8db56825ae71d6d79447849e617115f4a920fa2acdcab2b053c4b2838bd6b71'
```

### Example 3: Mixed query string and request body
* **queryString:** symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC
* **requestBody:** quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559
* **HMAC SHA256 signature:**

```shell
[linux]$ echo -n "symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTCquantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559" | openssl dgst -sha256 -hmac "NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j"
(stdin)= 0fd168b8ddb4876a0358a8d14d0c9f3da0e9b20c5d52b2a00fcf7d1c602f9a77
```


* **curl command:**

```shell
(HMAC SHA256)
[linux]$ curl -H "X-JEX-APIKEY: vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A" -X POST 'https://www.jex.com/api/v1/order?symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC' -d 'quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559&signature=0fd168b8ddb4876a0358a8d14d0c9f3da0e9b20c5d52b2a00fcf7d1c602f9a77'
```

Note that the signature is different in example 3.
There is no & between "GTC" and "quantity=1".





## Terminology

* `base asset` refers to the asset that is the `quantity` of a symbol.
* `quote asset` refers to the asset that is the `price` of a symbol.



## ENUM definitions
**Symbol status (status):**

* PRE_TRADING 
* TRADING 
* POST_TRADING 
* END_OF_DAY 
* HALT 
* AUCTION_MATCH 
* BREAK 

**Symbol type:**

* SPOT 
* OPTION 
* CONTRACT 

**spot and option Order status (status):**

* NEW 
* PARTIALLY_FILLED  
* FILLED  
* CANCELED  
* PENDING_CANCEL (currently unused))
* FAIL 
* CANCLEFILLED
* REJECTED  (currently unused)
* EXPIRED  (currently unused)


**contract status (status):**
 
* ENTRUSTED 
* ENTRUSTING 
* FAIL 
* PARTFILLED 
* FILLED 
* CANCEL 

**Order types (orderTypes, type):**

* LIMIT 
* MARKET  
* STOPLIMIT 
* STOPMARKET 
* PROFITLIMIT 
* PROFITMARKET 

**Order side (side):**

* BUY 
* SELL 

**Time in force:**

* GTC - Good Till Cancel 
* IOC - Immediate or Cancel 
* FOK - Fill or Kill 

**Kline/Candlestick chart intervals:**

m -> minutes; h -> hours; d -> days; w -> weeks; M -> months

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

**Rate limiters (rateLimitType)**

* REQUESTS_WEIGHT  
* ORDERS    
* RAW_REQUESTS 

**Rate limit intervals (interval)**

* SECOND
* MINUTE
* DAY

**Endpoints for futures account**

* transfer       
* fee            
* funding        
* closPosition   
* liquidation    
* adl            
* autoMargin     
* positionSize   
