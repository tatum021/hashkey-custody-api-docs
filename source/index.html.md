--- 

title: Jadepool SAAS API

language_tabs: 
   - shell
   - javascript
   - go
   - java

toc_footers: 
   - <a href='https://github.com/nbltrust/jadepool-saas-api-docs'>Documentation</a>

includes:
  - errors

search: true 

---

# Introduction 

Jadepool SAAS API

Feel free to check out our [NodeJs SDK](https://github.com/nbltrust/jadepool-saas-sdk-nodejs), [Go SDK](https://github.com/nbltrust/jadepool-saas-sdk-golang), [Java SDK](https://github.com/nbltrust/jadepool-saas-sdk-java).

# API Server URL:

### sandbox: https://openapi.jadepool.io


# General Structure

This is the general request structure for verifing the request by server.

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| timestamp | query/body | unix timestamp, seconds | Yes | string |
| nonce | query/body | nonce, random string, not repeated in 10 minutes | Yes | string |
| sign | query/body | hex string, sign parameters with HMACSHA256 | Yes | string |

The parameters `timestamp`, `nonce` and `sign` are located in query for GET requests, but located in the body for POST requests.

if POST request, the body looks like this:

`
{ 
  "timestamp": 1583376284,
  "nonce": "15833762841261615239762485",
  "mode": "auto",
  "sign":"7042a9fd6deea017be7ad76dfb48e4c36feca279819630c870a628f5352c9044"
}
`

if GET request, the url looks like this:

`
/api/v1/app/balance/ETH?timestamp=1583374390&nonce=1583374390314165815853245&sign=7c482cf15d9d25772eee2c43bd146c8136472a055b4587b9fb22d02720037875
`

<font size=4>The [signature method](#signature) is similar to OAuth.</font>

# Wallet API

```shell
$ git clone https://github.com/nbltrust/jadepool-saas-sdk-golang.git && cd jadepool-saas-sdk-golang
```

```javascript
const key = 'TyTLvCnHINbWZQag88hhmMz1'
const secret = 'uf0rPlTluGnIllGqx0X1os4hQ6rOdXDxStiN4qGd79lS6yeHZaOK4ldvRv1TBqr6'
const apiAddr = 'http://127.0.0.1:8092'

const api = new API(key, secret, apiAddr)
```

```go
app := sdk.NewAppWithAddr(apiAddr, key, secret)
```

```java
String endpoint = "http://127.0.0.1:8092";
String appKey = "TyTLvCnHINbWZQag88hhmMz1";
String appSecret = "uf0rPlTluGnIllGqx0X1os4hQ6rOdXDxStiN4qGd79lS6yeHZaOK4ldvRv1TBqr6";

APIContext appContext = new APIContext(endpoint, appKey, appSecret);
App appTest = new App(appContext);
```

The wallet key and secret can be generated in the wallet settings.

## Address

### create a new address 

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "CreateAddress" "ETH"
code: 0
message: success
data:
{
  "address": "0x9bf65CDF5729b9588F6bAEBb2Aa2926472D4a035",
  "mode": "deposit"
}
```

```javascript
    try {
        const result = await api.createAddress("ETH")
        console.log(result)
    } catch(e) {
        // do something
        console.log(e.response)
    }
```

```go
	result, _ := app.CreateAddress("ETH")
```

```java
  APIResult result = appTest.createAddress("ETH");
```

**Summary:** create a new address

#### HTTP Request 
`POST /api/v1/address/{coinName}/new` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| coinName | path | coinName | Yes | string |
| mode | body | mode | No | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
address | string | the new address
mode | string | address mode

### verify a address 

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "VerifyAddress" "ETH" "0x9bf65CDF5729b9588F6bAEBb2Aa2926472D4a035"
code: 0
message: success
data:
{
  "address": "0x9bf65CDF5729b9588F6bAEBb2Aa2926472D4a035",
  "valid": true
}
```

```javascript
    try {
        result = await api.verifyAddress("ETH", address) 
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.VerifyAddress("ETH", "0x9bf65CDF5729b9588F6bAEBb2Aa2926472D4a035")
```

```java
  APIResult result = appTest.verifyAddress("ETH", "0x9bf65CDF5729b9588F6bAEBb2Aa2926472D4a035");
```

**Summary:** verify a address

#### HTTP Request 
`POST /api/v1/address/{coinName}/verify` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| coinName | path | coinName | Yes | string |
| address | body | address | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
address | string | the address to validate
valid | boolean | the address is valid or not

## Wallet
### get wallet balance 

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "GetBalance" "ETH"
code: 0
message: success
data:
{
  "balance": "0.450000000000000000",
  "inLocked": "0.000000000000000000",
  "inLockedFee": "0.000000000000000000",
  "outLocked": "0.000000000000000000",
  "outLockedFee": "0.000000000000000000",
  "stakingBalance": "0.000000000000000000",
  "stakingInLocked": "0.000000000000000000",
  "stakingOutLocked": "0.000000000000000000",
  "delegateAmount": "0.000000000000000000",
  "delegateInLocked": "0.000000000000000000",
  "delegateOutLocked": "0.000000000000000000",
  "undelegateAmount": "0.000000000000000000",
  "undelegateInLocked": "0.000000000000000000",
  "undelegateOutLocked": "0.000000000000000000"
}
```

```javascript
    try {
        result = await api.getBalance("ETH")
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.GetBalance("ETH")
```

```java
  APIResult result = appTest.getBalance("ETH")
```

**Summary:** get wallet balance

#### HTTP Request 
`GET /api/v1/app/balance/{coinName}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| coinName | path | coin type | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
balance | string | the coin balance
inLocked | string | the coin transfer in amount under locked
inLockedFee | string | the coin transfer in fee under locked
outLocked | string | the coin transfer out amount under locked
outLockedFee | string | the coin transfer out fee under locked
stakingBalance | string | the staking coin balance
stakingInLocked | string | the staking coin transfer in amount under locked
stakingOutLocked | string | the staking coin transfer out amount under locked
delegateAmount | string | the coin delegate amount
delegateInLocked | string | the coin delegate transfer in amount under locked
delegateOutLocked | string | the coin delegate transfer out amount under locked
undelegateAmount | string | the undelegate coin amount
undelegateInLocked | string | the undelegate coin transfer in amount under locked
undelegateOutLocked | string | the undelegate coin transfer out amount under locked

### get wallet assets 

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "GetAssets"
code: 0
message: success
data:
{
  "assets": [
    {"id": 1, name: "ETH"}
  ]
}
```

```javascript
    try {
        result = await api.getAssets()
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.GetAssets()
```

```java
  APIResult result = appTest.getAssets();
```

**Summary:** get wallet assets

#### HTTP Request 
`GET /api/v2/app/assets` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
assets | array | the wallet coin list

### get wallet order

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "GetOrder" "rNXBQGJlw09apVyg4nDo"
code: 0
message: success
data:
{
  "bizType": "WITHDRAW",
  "block": 12970670,
  "coinName": "ETH",
  "confirmations": 21,
  "fee": "0.005000000000000000",
  "from": "0xdedc1eca923cc1227c20571030146d8a01b70774",
  "id": "rNXBQGJlw09apVyg4nDo",
  "memo": "",
  "n": 0,
  "state": "DONE",
  "to": "0xF0706B7Cab38EA42538f4D8C279B6F57ad1d4072",
  "txid": "0xcfc4cb925c77c2ea10be3c233029fc1f05d0ddffe7396920ddd493544e52933d",
  "type": "ETH",
  "value": "0.045000000000000000"
}
```

```javascript
    try {
        result = await api.getOrder("rNXBQGJlw09apVyg4nDo")
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.GetOrder("rNXBQGJlw09apVyg4nDo")
```

```java
  APIResult result = appTest.getOrder("rNXBQGJlw09apVyg4nDo");
```

**Summary:** get wallet order

#### HTTP Request 
`GET /api/v1/app/order/{id}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| id | path | order id | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
bizType | string | order type
block | number | the block transaction mined in
coinName | string | unique token name
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
from | string | transaction input
id | string | order id
memo | string | order note
n | number | order index
state | string | order state
to | string | transaction output
txid | string | transaction hash
type | string | token type
value | string | transaction value

### withdraw 

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "Withdraw" "$(date +%s)" "ETH" "0xF0706B7Cab38EA42538f4D8C279B6F57ad1d4072" "0.05"
code: 0
message: success
data:
{
  "bizType": "WITHDRAW",
  "block": -1,
  "coinName": "ETH",
  "confirmations": 0,
  "fee": "0.0050000000",
  "from": "",
  "id": "Jd7qbDRa1qM1lxK5Qe2M",
  "memo": "",
  "n": 0,
  "state": "INIT",
  "to": "0xF0706B7Cab38EA42538f4D8C279B6F57ad1d4072",
  "txid": "",
  "type": "ETH",
  "value": "0.0450000000"
}
```

```javascript
    try {
        let id = ''+new Date().valueOf()
        let coinType = 'ETH'
        let to = '0x56204b988844b20160035273fD98Dbb2A54C85F5'
        let value = '0.01'
        let memo = ''
        result = await api.withdraw(id, coinType, to, value, memo)
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.Withdraw("1569225735", "ETH", "0xF0706B7Cab38EA42538f4D8C279B6F57ad1d4072", "0.05")
```

```java
  APIResult result = appTest.withdraw(id, "ETH", "0xF0706B7Cab38EA42538f4D8C279B6F57ad1d4072", "0.05");
```

**Summary:** withdraw

#### HTTP Request 
`POST /api/v1/app/{coinName}/withdraw` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| coinName | path | coin type | Yes | string |
| id | body | withdraw id | Yes | string |
| to | body | receive address | Yes | string |
| value | body | withdraw amount | Yes | string |
| memo | body | address memo | No | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
bizType | string | order type
block | number | the block transaction mined in
coinName | string | unique token name
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
from | string | transaction input
id | string | order id
memo | string | order note
n | number | order index
state | string | order state
to | string | transaction output
txid | string | transaction hash
type | string | token type
value | string | transaction value

## Staking
### get staking validators 

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "GetValidators" "IRIS"
code: 0
message: success
data:
{
  "validators": [
    {
      "address": "fva125aw6ew57jugyultpcqrlk7sewamxtjer48cy0",
      "commission": {
        "maxChangeRate": 1,
        "maxRate": 1,
        "rate": 0.1,
        "updateAt": "0001-01-01T00:00:00.000Z"
      },
      "delegationAmount": "2770068.5544012594476618430000000000",
      "jailed": false,
      "minSelfDelegation": "0",
      "name": "validator0",
      "pubkey": "fcp1zcjduepqt9c5p8e25s5zxxsg5758cx4aqylarkxrc35qg0d0pge84p3rtaasmz58sl",
      "shares": "2770068.5544012594476618430000000000",
      "status": 2
    },
    {
      "address": "fva1at9ye95xmsp9wzpc6asmju5w55nece5c4fv28m",
      "commission": {
        "maxChangeRate": 1,
        "maxRate": 1,
        "rate": 0.1,
        "updateAt": "0001-01-01T00:00:00.000Z"
      },
      "delegationAmount": "370.1929307965746291140000000000",
      "jailed": true,
      "minSelfDelegation": "0",
      "name": "validator1",
      "pubkey": "fcp1zcjduepqj0gp48fu32h2ttsn4anz368cyjyklq0ut7c0slk0ncxqv9l3mqmsehkpr6",
      "shares": "370.1929307965746291140000000000",
      "status": 0
    }
  ]
}
```

```javascript
    try {
        result = await api.getValidators("IRIS")
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.GetValidators("IRIS")
```

```java
  APIResult result = appTest.getValidators("IRIS");
```

**Summary:** get staking validators

#### HTTP Request 
`GET /api/v1/staking/{coinName}/validators` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| coinName | path | coin type | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
validators | array | the validator list

### delegate

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "Delegate" $(date +%s) "IRIS2" "1"
code: 0
message: success
data:
{
  "id": "J9rPWXA13Kl9bam4Vo6G",
  "type": "DELEGATE",
  "value": "1"
}
```

```javascript
    try {
        let id = ''+new Date().valueOf()
        let coinType = 'ATOM'
        let value = '0.06'
        result = await api.delegate(id, coinType, value)
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.Delegate("1569231558", "IRIS2", "1")
```

```java
  APIResult result = appTest.delegate("1569231558", "IRIS2", "1");
```

**Summary:** delegate

#### HTTP Request 
`POST /api/v1/staking/{coinName}/delegate` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| coinName | path | coin type | Yes | string |
| id | body | delegate custom order id | Yes | string |
| value | body | delegate amount | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
id | string | order id
type | string | order type
value | string | delegate amount

### undelegate

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "UnDelegate" $(date +%s) "IRIS2" "1"
code: 0
message: success
data:
{
  "id": "P6wAQNrWp5XanXk5L7VR",
  "type": "UNDELEGATE",
  "value": "1"
}
```

```javascript
    try {
        let id = ''+new Date().valueOf()
        let coinType = 'ATOM'
        let value = '0.01'
        result = await api.undelegate(id, coinType, value)
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.UnDelegate("1569231809", "IRIS2", "1")
```

```java
  APIResult result = appTest.unDelegate("1569231809", "IRIS2", "1");
```

**Summary:** undelegate

#### HTTP Request 
`POST /api/v1/staking/{coinName}/undelegate` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| coinName | path | coin type | Yes | string |
| id | body | delegate custom order id | Yes | string |
| value | body | delegate amount | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
id | string | order id
type | string | order type
value | string | delegate amount

### add funding requirement

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "AddUrgentStakingFunding" $(date +%s) "IRIS2" "1" "1569302076"
code: 0
message: success
data:
{}
```

```javascript
    try {
        let id = ''+new Date().valueOf()
        let coinType = 'ATOM'
        let value = '0.06'
        let expiredAt = parseInt(new Date().valueOf()/1000) + 86400 * 21
        result = await api.addUrgentStakingFunding(id, coinType, value, expiredAt)
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.AddUrgentStakingFunding("1569292076", "IRIS2", "1", 1569302076)
```

```java
  APIResult result = appTest.addUrgentStakingFunding("1569292076", "IRIS2", "1", 1569302076);
```

**Summary:** add funding requirement

#### HTTP Request 
`POST /api/v1/staking/{coinName}/funding` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| coinName | path | coin type | Yes | string |
| id | body | funding id | Yes | string |
| value | body | funding amount | Yes | string |
| expiredAt | body | expired time | Yes | number |

**Response Result**

Value | Type | Description
--------- | ------- | ---------

### get staking interest

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "GetStakingInterest" "IRIS" "2019-09-26"
code: 0
message: success
data:
{
  "coinName": "IRIS",
  "interest": "0.12345"
}
```

```javascript
    try {
        result = await api.getStakingInterest("IRIS", "2019-09-26")
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.GetStakingInterest("IRIS", "2019-09-26")
```

```java
  APIResult result = appTest.getStakingInterest("IRIS", "2019-09-26");
```

**Summary:** get staking interest

#### HTTP Request 
`GET /api/v1/staking/{coinName}/interest` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| coinName | path | coin type | Yes | string |
| date | query | UTC date | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
coinName | string | the coin name
value | string | the interest value

## OTC

Provide better pricing and fast settlement for large trades. The customer can trade on the OTC tab in custody dashboard. The following are normal processs.

1. [Set support quote symbols](#set-support-quote-symbols) for the customer. The customer will see the quote symbols on the OTC tab.
2. An open order would be created after the customer get quote on the OTC tab. Get that by [the API](#get-open-orders).
3. [Feed price](#feed-price) for the open order.
4. The price will show on the OTC tab and wait for confirmation by the customer. [Check if the price is chosen](#get-price) consistently during this period.
5. [Close the price](#close-price) after the price is chosen. But if the price no longer suitable, just [terminate it](#terminate-price). The transaction would be done when close the price.

### set support quote symbols

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "OTCSetSymbols" '[
  {
    "baseCoinID": 1,
    "quoteCoinID": 2,
    "baseCoinAmountRange": ["10", "100"],
    "quoteCoinAmountRange": ["10000", "100000000"]
  }]'
code: 0
message: success
data:
{
  "symbols": [
    {
      "baseCoin": {
        "id": 1,
        "name": "BTC"
      },
      "quoteCoin": {
        "id": 2,
        "name": "USD"
      },
      "baseCoinAmountRange": [10, 100],
      "quoteCoinAmountRange": [10000, 100000000]
    }
  ]
}
```

**Summary:** set all support quote symbols

#### HTTP Request 
`POST /api/v1/otc/symbols`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| symbols | body | quote symbol list | Yes | array |

symbol:

Value | Description | Required
--------- | --------- | --------
baseCoinID | buy/sell asset id | Yes
quoteCoinID | spend/receive asset id | Yes
baseCoinAmountRange | the min/max limit of baseCoin | No
quoteCoinAmountRange | the min/max limit of quoteCoin | No

**Response Result**

Value | Type | Description
--------- | ------- | ---------
symbols | array | quote symbol list

### get open orders

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "OTCGetOrders" '[
{
  "baseCoinID": 1,
  "quoteCoinID": 2
}]'
code: 0
message: success
data:
{
  "orders": [
    {
      "id": "8yxkewovldjmmqj2m0n9pz71",
      "status": "OPEN",
      "accountID": "1009",
      "type": "BUY",
      "baseCoin": {
        "id": 1,
        "name": "BTC"
      },
      "quoteCoin": {
        "id": 2,
        "name": "USD"
      },
      "amountCoin": {
        "id": 1,
        "name": "BTC"
      },
      "amount": 10
    }
  ]
}
```

**Summary:** get opening quote orders

#### HTTP Request 
`POST /api/v1/otc/orders` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| quoteSymbols | body | quote symbol list | Yes | array |

quote symbol:

| Name | Description | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| baseCoinID | buy/sell asset id | number |
| quoteCoinID | spend/receive asset id | number |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
orders | array | order list


### get order

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "OTCGetOrder" "8yxkewovldjmmqj2m0n9pz71"
code: 0
message: success
data:
{
  "id": "8yxkewovldjmmqj2m0n9pz71",
  "status": "OPEN",
  "accountID": "1009",
  "type": "BUY",
  "baseCoin": {
    "id": 1,
    "name": "BTC"
  },
  "quoteCoin": {
    "id": 2,
    "name": "USD"
  },
  "amountCoin": {
    "id": 1,
    "name": "BTC"
  },
  "amount": 10
}
```

**Summary:** get order by id

#### HTTP Request 
`GET /api/v1/otc/order/{orderID}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| orderID | path | order id | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
id | string | order id
status | string | order status
accountID | string | account id
type | string | quote mode, BUY/SELL
baseCoin | string | base coin
quoteCoin | string | quote coin
amountCoin | string | amount coin
amount | number | amount

order status:

Value | Description
--------- | ---------
OPEN | the order is waiting for feeding price
DONE | the order has been carried out
TERMINATE | the order has been cancelled

### feed price

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "OTCFeedPrice" "8yxkewovldjmmqj2m0n9pz71" "7739.90" "0320042810120495" 1588151193
code: 0
message: success
data:
{
  "id": "25ovqdkrm86201g73zx4ynep",
  "status": "OPEN",
  "invaildAt": 1588151193
}
```

**Summary:** feed price

#### HTTP Request 
`POST /api/v1/otc/orders/{orderID}/price` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| orderID | path | order id | Yes | string |
| price | body | price | Yes | string |
| customID | body | the custom id defined by requester | Yes | string |
| invalidAt | body | timestamp | Yes | number |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
id | string | price id
status | string | price status
invalidAt | number | invalid timestamp

price status:

Value | Description
--------- | ---------
OPEN | the price is waiting for confirmation
CLOSE | the order has been carried out with the price
TERMINATE | the price was rejected by OTC provider

### get price

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "OTCGetPrice" "25ovqdkrm86201g73zx4ynep"
$ go run cmd/ctl/main.go "appkey" "appsecret" "OTCGetPriceByCustomID" "0320042810120495"
code: 0
message: success
data:
{
  "id": "25ovqdkrm86201g73zx4ynep",
  "status": "OPEN",
  "choose": true,
  "customID": "0320042810120495",
  "price": 7739.90,
  "invaildAt": 1588151193,
  "orderID": "8yxkewovldjmmqj2m0n9pz71"
}
```

**Summary:** get the latest status of price

#### HTTP Request 
`GET /api/v1/otc/price/{priceID}`

`GET /api/v1/otc/price/custom/{customID}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| priceID | path | price id | Yes | string |
| customID | path | custom id | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
id | string | price id
status | string | price status
choose | boolean | mark if customer choose
customID | string | the custom id
price | number | price
invaildAt | number | invalid timestamp
orderID | string | the associated order id

### close price

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "OTCClosePrice" "25ovqdkrm86201g73zx4ynep"
code: 0
message: success
data:
{
  "id": "25ovqdkrm86201g73zx4ynep",
  "status": "CLOSE",
  "choose": true,
  "customID": "0320042810120495",
  "price": 7739.90,
  "invaildAt": 1588151193,
  "orderID": "8yxkewovldjmmqj2m0n9pz71"
}
```

**Summary:** make a deal with the price

#### HTTP Request 
`GET /api/v1/otc/price/{priceID}/close`

`GET /api/v1/otc/price/custom/{customID}/close`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| priceID | path | price id | Yes | string |
| customID | path | custom id | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
id | string | price id
status | string | price status, OPEN/CLOSE/TERMINATE
choose | boolean | mark if customer choose
customID | string | the custom id
price | number | price
invaildAt | number | invalid timestamp
orderID | string | the associated order id


### terminate price

```shell
$ go run cmd/ctl/main.go "appkey" "appsecret" "OTCTerminatePrice" "25ovqdkrm86201g73zx4ynep"
code: 0
message: success
data:
{
  "id": "25ovqdkrm86201g73zx4ynep",
  "status": "TERMINATE",
  "choose": true,
  "customID": "0320042810120495",
  "price": 7739.90,
  "invaildAt": 1588151193,
  "orderID": "8yxkewovldjmmqj2m0n9pz71"
}
```

**Summary:** reject the price

#### HTTP Request 
`GET /api/v1/otc/price/{priceID}/terminate`

`GET /api/v1/otc/price/custom/{customID}/terminate`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| priceID | path | price id | Yes | string |
| customID | path | custom id | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
id | string | price id
status | string | price status, OPEN/CLOSE/TERMINATE
choose | boolean | mark if customer choose
customID | string | the custom id
price | number | price
invaildAt | number | invalid timestamp
orderID | string | the associated order id


# Company API

```shell
$ git clone https://github.com/nbltrust/jadepool-saas-sdk-golang.git && cd jadepool-saas-sdk-golang
```

```javascript
const companyKey = 'TyTLvCnHINbWZQag88hhmMz1'
const companySecret = 'uf0rPlTluGnIllGqx0X1os4hQ6rOdXDxStiN4qGd79lS6yeHZaOK4ldvRv1TBqr6'
const apiAddr = 'http://127.0.0.1:8092'

const api = new API(companyKey, companySecret, apiAddr)
```

```go
company := sdk.NewCompanyWithAddr(apiAddr, key, secret)
```

```java
String endpoint = "http://127.0.0.1:8092";
String appKey = "TyTLvCnHINbWZQag88hhmMz1";
String appSecret = "uf0rPlTluGnIllGqx0X1os4hQ6rOdXDxStiN4qGd79lS6yeHZaOK4ldvRv1TBqr6";
APIContext companyContext = new APIContext(endpoint, appKey, appSecret);
Company companyTest = new Company(companyContext);
```

The company key and secret can be generated in the company settings.

## Funding
### get funding wallets list 

```shell
$ go run cmd/ctl/main.go "companykey" "companysecret" "GetFundingWallets"
code: 0
message: success
data:
{
  "wallets": [
    {
      "balances": [
        {
          "balance": "0.000400000000000000",
          "money": 4.652535315760001,
          "name": "BTC"
        },
        {
          "balance": "0.450000000000000000",
          "money": 102.18689952390001,
          "name": "ETH"
        }
      ],
      "id": "QrLxg3XgKPMR1O8",
      "name": "test2"
    }
  ]
}
```

```javascript
    try {
        result = await api.getFundingWallets()
        console.dir(result, {depth: null})
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = company.GetFundingWallets()
```

```java
  APIResult result = companyTest.getFundingWallets();
```

**Summary:** get funding wallets list

#### HTTP Request 
`GET /api/v1/funding/balances` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-Company-Key | header | company key | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
wallets | array | wallet list

### funding transfer 

```shell
$ go run cmd/ctl/main.go "companykey" "companysecret" "FundingTransfer" "QrLxg3XgKPMR1O8" "ZKz8XpwXGPBAQVE" "BTC" "0.0001"
code: 0
message: success
data:
{
  "balances": [
    {
      "assetName": "BTC",
      "balance": "0.000300000000000000",
      "id": "QrLxg3XgKPMR1O8"
    },
    {
      "assetName": "BTC",
      "balance": "0.000200000000000000",
      "id": "ZKz8XpwXGPBAQVE"
    }
  ],
  "record": {
    "amount": "0.0001",
    "assetName": "BTC",
    "created_at": 1569306519,
    "from": "QrLxg3XgKPMR1O8",
    "id": "27x05MGQnwKnKRVeD9Eo",
    "memo": "",
    "to": "ZKz8XpwXGPBAQVE",
    "type": "SUPPLY"
  }
}
```

```javascript
    try {
        const from = 'L6RayqPn4jXExW0'
        const to = 'e5dJyVp8R3B1m4o'
        const coinType = 'ETH'
        const value = '0.01'
        result = await api.fundingTransfer(from, to, coinType, value)
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = company.FundingTransfer("QrLxg3XgKPMR1O8", "ZKz8XpwXGPBAQVE", "BTC", "0.0001")
```

```java
  APIResult result = companyTest.fundingTransfer("QrLxg3XgKPMR1O8", "ZKz8XpwXGPBAQVE", "BTC", "0.0001");
```

**Summary:** funding transfer

#### HTTP Request 
`POST /api/v1/funding/transfer` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-Company-Key | header | company key | Yes | string |
| from | body | from wallet id | Yes | string |
| to | body | to wallet id | Yes | string |
| value | body | transfer amount | Yes | string |
| assetName | body | coin name | Yes | string |
| memo | body | address memo | No | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
balances | array | balances after transfer
record | object | transfer record

### get funding records list 

```shell
$ go run cmd/ctl/main.go "companykey" "companysecret" "GetFundingRecords" 1 10
code: 0
message: success
data:
{
  "records": [
    {
      "amount": "0.000100000000000000",
      "assetName": "BTC",
      "created_at": 1569306519,
      "from": "QrLxg3XgKPMR1O8",
      "id": "27x05MGQnwKnKRVeD9Eo",
      "memo": "",
      "opsAccount": "yang.zhang@nbltrust.com",
      "status": "OPEN",
      "to": "ZKz8XpwXGPBAQVE",
      "type": "SUPPLY"
    },
    {
      "amount": "0.000100000000000000",
      "assetName": "BTC",
      "created_at": 1564124679,
      "from": "QrLxg3XgKPMR1O8",
      "id": "qDP67gB3XPon14v92jW5",
      "memo": "",
      "opsAccount": "yang.zhang@nbltrust.com",
      "status": "OPEN",
      "to": "ZKz8XpwXGPBAQVE",
      "type": "SUPPLY"
    }
  ],
  "totalAmount": 2
}
```

```javascript
    try {
        result = await api.getFundingRecords()
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = company.GetFundingRecords(1, 10)
```

```java
  APIResult result = companyTest.getFundingRecords(1, 10);
```

**Summary:** get funding records list

#### HTTP Request 
`GET /api/v1/funding/records` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-Company-Key | header | company key | Yes | string |
| page | query | page, e.g. 1 | Yes | string |
| amount | query | item count on this page, e.g. 10 | Yes | string |
| coins | query | coin list, can be empty string | Yes | string |
| froms | query | from list, can be empty string | Yes | string |
| toes | query | to list, can be empty string | Yes | string |
| type | query | record type, can be empty string | Yes | string |
| orderBy | query | order by | Yes | string |
| sort | query | sort | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
records | array | record list

# Callback

## Order
> Order notification sends JSON structured like this:

```json
{
  "id": "2XB0eKAvj7KvjZDk59zl",
  "bizType": "DEPOSIT",
  "coinName": "ETH",
  "type": "ETH",
  "state": "DONE",
  "memo": "",
  "value": "1.000000000000000000",
  "fee": "0.000000000000000000",
  "from": "0xF0706B7Cab38EA42538f4D8C279B6F57ad1d4072",
  "to": "0x29152c850456899A78178622B6543BBFfC224495",
  "txid": "0x8487e23bbf71f1763e015598283ae891cc5ea8d444f87a0a60a0b5eb7e1a4d59",
  "n": 0,
  "block": 13721091,
  "affirmativeConfirmation": 20,
  "confirmations": 27,
  "sign": "796dde931a15c98edc3dfdb65a2c2addfde422f217a1f6934be9226542839aa0"
}
```

**Callbck Result**

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
affirmativeConfirmation | number | affirmative confirmation of the blockchain
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
block | number | the block transaction mined in
memo | string | order note, editable on admin
n | number | the order index
sign | string | hex string, sign parameters with HMACSHA256

# Signature
1. Get the current timestamp (seconds) and the nonce (random string). Please make sure the timestamp error not exceed 5 minutes and the nonce not repeated in 10 minutes.

2. Form a String message (sorted) that contains data prarams, the timestamp and nonce above. 

    if body params is: 
</br>
`
{
  "mode": "auto"
}
`

    The message string looks like this:
</br>
`
mode=auto&nonce=15833762841261615239762485&timestamp=1583376284
`

3. Sign the message using HMAC-SHA256. If the AppSecret is `yeTJ3EnOkyQQEjhTMVqn165Dqjp43bhTwXLIv25Ycdu8qwDOyqpa0WV54C6sO4HW`, the sign param would be like this:
</br>
`
7042a9fd6deea017be7ad76dfb48e4c36feca279819630c870a628f5352c9044
`

4. Send request as the same format as described in [General Structure](#general-structure).