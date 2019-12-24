# User Data Streams

* The base API endpoint is: **https://www.jex.com** 
* A User Data Stream `listenKey` is valid for 60 minutes after creation.
* Doing a `PUT` on a `listenKey` will extend its validity for 60 minutes.
* Doing a `DELETE` on a `listenKey` will close the stream.
* The base websocket endpoint is: **wss://ws.jex.com** 
* User Data Streams are accessed at **/ws/\<listenKey\>**
* A single connection to **stream.jex.com** is only valid for 24 hours; expect to be disconnected at the 24 hour mark
* User data stream payloads are **not guaranteed** to be in order during heavy periods; **make sure to order your updates using E**

* Specifics on how user data streams work.
* For detailed subscription method, please refer to another websocket document.


## Start user data stream (USER_STREAM)

`POST /api/v1/userDataStream`

Start a new user data stream. The stream will close after 60 minutes unless a keepalive is sent.

**Weight:**
1

**Parameters:**
NONE

>**Response**  

```javascript
{
  "listenKey": "pqia91ma19a5s61cv6a81va65sdf19v8a65a1a5s61cv6a81va65sdf19v8a65a1" //用于订阅的数据流名
}
```

## Keepalive (USER_STREAM)

`PUT /api/v1/userDataStream`

Keepalive a user data stream to prevent a time out. User data streams will close after 60 minutes. It's recommended to send a ping about every 30 minutes.

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
listenKey | STRING | YES

>**Response**  

```javascript
{}
```

## Close user data stream (USER_STREAM)

`DELETE /api/v1/userDataStream`


**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
listenKey | STRING | YES

>**Response**  

```javascript
{}
```



## Account updates
* Event type of account updates is fixed to `accountSpotInfo` AND `accountOptionInfo` AND `accountContractInfo` They respectively refers to the asset related event of spot, options and futures.
When there is a change of asset, relevant event will be pushed


>**Response** 

```javascript
{
  "e": "accountSpotInfo",       // Event type
  "E": 1499405658849,           // Event time
  "m": 0,                       // Fee rate of entry order 
  "t": 0,                       // Fee rate of taking order
  "u": 1499405658848,           // Time stamp of
  "B": [                        // Balance
    {
      "a": "LTC",               // Asset name
      "o": "1.57",              // Order Margin（Only Furtures）
      "p":  "0"  ，             // Position Margin（Only Furtures ）
      "f": "17366.18538083",    // Free amount
      "l": "0.00000000",        // Locked amount  
      "T": true,                // Can trade
      "W": true,                // Can withdraw
      "D": true,                // Can deposit
    },
    {
      "a": "BTC",
      "f": "10537.85314051",
      "l": "2.19464093",
      "T": true,                // Can trade
      "W": true,                // Can withdraw
       "D": true,               // Can deposit
 },
    {
    "T": true,                  // Can trade
    "W": true,                  // Can withdraw
    "D": true,                  // Can deposit
    "a": "ETH",
    "f": "17902.35190619",
    "l": "0.00000000"
    }
  ]
}
```

### Update of orders

* Account state is updated with the outboundAccountInfo event.

event type is `execSpotReport` AND `execOptionReport` AND `execContractReport` They respectively refers to the asset related event of spot、options、futures Event type

Possible executive type of contract(X field)

> Response  of spot and options:

```javascript
{
  "E": 1499405658849,            // Event time
  "e": "execSpotReport",         // Event type
  "s": "JEXBTC",                 // Symbol
  "S": "BUY",                    // Order direction
  "q": "1.00000000",             //  Order quantity
  "p": "0.10264410",             //  Order price
  "X": "NEW",                    // Current order state
  "i": 4293153,                  // ID Order ID
  "z": "0.00000000",             //  Cumulative filled quantity
  "O": 1499405658657,            // Transaction time
  "Z": "0.00000000",             // Total transaction sum
}
```
#### Possible executive type of spot and options (X field):

* NEW 
* PARTIALLY_FILLED  
* FILLED  
* CANCELED  
* FAIL 
* CANCLEFILLED


> Response contract :

```javascript
{
  "E": 1499405658849,            // Event time
  "e": "execContractReport",     // Event type
  "s": "ETHBTC",                 // Symbol   (Not exist if refused)   
  "X": "NEW",                    // Current state of the order   
  "r": "NONE",                   // Refused reason of the order ( exist if refused)
  "i": 4293153,                  // Order ID
  "z": "0.00000000",             // Cumulative filled quantity (Not exist if refused)
  "Z": "0.00000000",             // Cumulative filled sum(Not exist if refused)

}
```



#### Possible executive type of contract(X field):

* ENTRUSTED 
* ENTRUSTING 
* FAIL 
* PARTFILLED 
* FILLED 
* CANCEL    


> Response example if refused:

``` javascript
{
  E: 1571225843289
  X: "FAIL"
  e: "execContractReport"
  i: "4612616205276108403"
  r: "insufficient Available"  
}
```
#### if refused(R field) 
  *  insufficient Available
  *  too many pending requests
  *  position is being liquidated
  *  position is being closed
  *  closing empty position
  *  closing order is enough
  *  contract is disabled
  *  reach liquidation price
  *  reach risk limit

### Contract positions

* Where there is a change in contract position
* event type is  contractPositions

> Response:

```javascript
{
	"E": 1553050064078,
	"e": "contractPositions",
	"p": [{
		"M": "100", // Largest leverage available 
		"R": "3903.99060000", // Used risk limits
		"S": "0", // sell Entrusted value
		"a": "20", // Auto-deleverage sequence
		"b": "0", // buy Entrusted value
		"c": "1301.33020000", //Open price
		"d": "longs", //Direction 
		"i": "0.05", //Initial margin
		"l": "-49.9759", //Close price
		"m": "0.005", //Maintenance Margin
		"n": "0", //Current entrusted value 
		"o": "4088.0779300000", //Margin
		"q": "3.0000",//Holding positions 
		"r": "900000",//Current risk value
		"s": "EOSUSDT",//Symbol
		"t": "3903.99060000", //Holding positions costs value 
		"v": "20.00"//Leverage
	}]
}

```