# Web Socket Streams 

## General WSS information
* The base endpoint is: **wss://ws.jex.com** 
* Streams can be accessed either in a single raw stream or in a combined stream
* Raw streams are accessed at **/ws/\<streamName\>**
* Combined streams are accessed at **/stream?streams=\<streamName1\>/\<streamName2\>/\<streamName3\>**
* Combined stream events are wrapped as follows: **{"stream":"\<streamName\>","data":\<rawPayload\>}**
* All symbols for streams are **lowercase**
* A single connection to **stream.jex.com** is only valid for 24 hours; expect to be disconnected at the 24 hour mark
* The websocket server will send a `ping frame` every 3 minutes. If the websocket server does not receive a `pong frame` back from the connection within a 10 minute period, the connection will be disconnected. Unsolicited `pong frames` are allowed.
* WSS contract order sells are uniformly negative
* It is recommended to use the combined streams method for connection. Each combined stream can subscribe to a maximum of 10 streams

## Stream Detailed Stream information

## Websocket Trade Streams

* Every 100 ms push, every single transaction in the last second is defined as a transaction between only one order taker and one bill holder.

### Stream Name: \<symbol\>@\<tradeType\>

>**Response**  

```javascript

[{
  "e": "spotTrade",    //  Event type
  "E": 123456789,      // Event time
  "s": "JEXBTC",       // Symbol
  "t": 12345,          // Trade ID
  "p": "0.001",        // Price
  "q": "100",          // Quantity
  "T": 123456785,      // Trade time
  "m": true,           // Is the buyer the market maker?If true，this is a seller order，otherwise it is a buyer order.
},{...},...
]
```

### tradeType

- Support 3 types as below:
  - spotTrade 
  - optionTrade 
  - contractTrade 

## Websocket Kline/Candlestick Streams

* The Kline/Candlestick Stream push updates to the current klines/candlestick every second

- Booking Kline requires interval parameter, shortest one is minute line, longest one is week line. Support intervals as below:

  - 1M 
  - 5M 
  - 15M 
  - 30M
  - 1H 
  - 4H
  - 1D 
  - 1W

### Stream Name: \<symbol\>@\<klineType\>_\<interval\>

>**Response**  

```javascript
{
  "e": "spotKline",     // Event type
  "E": 123456789,       // Event time
  "s": "JEXBTC",        // Symbol
  "k": {
    "t": 123400000,     // Kline start time
    "T": 123460000,     // Kline close time
    "s": "JEXBTC",      // Symbol
    "i": "1m",          // Interval
    "o": "0.0010",      // Open price
    "c": "0.0020",      // Close price
    "h": "0.0025",      // High price
    "l": "0.0015",      // Low price
    "v": "1000",        // Base asset volume
    "q": "1.0000",      // Quote asset volume
  }
}
```

### klineType

-Support types
  - spotKline
  - optionKline
  - contractKline

## Individual Symbol Mini Ticker Stream

* 24hr rolling window mini-ticker statistics for a single symbol pushed every second.

### Stream Name: \<symbol\>@\<miniTickerType\>

>**Response**  

```javascript
{
    "e": "24hrSpotMiniTicker",   // Event type
    "E": 123456789,              // Event time
    "s": "JEXBTC",               // Symbol
    "c": "0.0025",               // Close price
    "h": "0.0025",               //  High  price
    "o": "0.0025",               // Open price
    "l": "0.0010",               // Low price
    "m": "10000",                // mark price(Only  contractMiniTicker have)
    "i": "10000",                // index price(Only contractMiniTicker have)
    "v": "10000",                // Base asset volume
    "q": "18"                    // Total traded quote asset volume
}

```

### miniTickerType

- Support types:
  - spotMiniTicker
  - optionMiniTicker
  - contractMiniTicker

## All Market Mini Tickers Stream

* 24hr rolling window mini-ticker statistics for all symbols that changed in an array pushed every second

### StreamName: !\<miniTickerType\>@arr

>**Response**  

```javascript

[
  {
     // Same as <symbol>@miniTicker payload
  }
]
```

### miniTickerType 

- Support types: 
  - spotMiniTicker
  - optionMiniTicker
  - contractMiniTicker

## Individual Symbol Ticker Streams

* 24hr rollwing window ticker statistics for a single symbol pushed every second. 

### Stream Name: \<symbol\>@\<tickerType\>

>**Response**  

```javascript
{
  "e": "24hrSpotTicker",  // Event type
  "E": 123456789,         // Event time
  "s": "JEXBTC",          // Symbol
  "p": "0.0015",          // Price change in 24h
  "P": "250.00",          // Price change in 24h percent
  "c": "0.0025",          // Last price
  "h": "0.0025",          // Highest price in 24h
  "l": "0.0010",          // Lowest price in 24h
  "v": "10000",           // Trade volume in 24h
  "q": "18",              // Trade asset in 24h
  "m": "10000",           // mark price(Only  contractMiniTicker have)
  "i": "10000",           // index price(Only contractMiniTicker have)
  "O": 1234567890,        // TODO 0  Starting time
  "C": 86400000,          // TODO 3600*24*1000，milliseconds in 24h  ending time
}
```
## All Market Tickers Stream

> 24hr rolling window ticker statistics for all symbols that changed in an array pushed every second.

### StreamName: !\<tickerType\>@arr

### Payload:

```javascript
[
  {
     //Same as <symbol>@ticker payload
  }
]
```
- tickerType 
  - spotTicker
  - optionTicker
  - contractTicker


## Partial Book Depth Streams

* Top <levels> bids and asks, pushed every second. Valid <levels> are 5, 10, or 20.

### Stream Name: \<symbol\>@\<depthType\>\<levels\>

>**Response**  :

```javascript
{
  "bids": [             // Buy
    [
      "0.0024",         // Price
      "10",             // Quantity
    ]
  ],
  "asks": [             // Sell
    [
      "0.0026",         // Price
      "100",            // Quantity
    ]
  ]
}
```


- depthType 
  - spotDepth
  - optionDepth
  - contractDepth
- levels 
  - 5
  - 10
  - 20

## Diff. Depth Stream
* Order book price and quantity depth updates used to locally manage an order book pushed every second.

### Stream Name: \<symbol\>@\<depthType\>


>**Response** :

```javascript
{
  "e": "spotDepthUpdate", // Event type
  "s": "JEXBTC",          // Symbol
  "v": 157,               // Version to be updated
  "b": [                  // Changing depth for buyer
    [
      "0.0024",          // Price
      "10",              // Quantity
    ]
  ],
  "a": [                 // Changing depth for seller
    [
      "0.0026",          // Price
      "100",             // Quantity
    ]
  ]
}
```
- depthType Support types
  - spotDepth
  - optionDepth
  - contractDepth
- Types for Depth Streams 
  - spotDepthUpdate
  - optionDepthUpdate
  - contractDepthUpdate
