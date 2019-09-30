--- 

title: API Reference

language_tabs: 
   - shell
   - javascript
   - go

toc_footers: 
   - <a href='https://github.com/nbltrust/jadepool-saas-api-docs'>Documentation</a> 

search: true 

---

# Introduction 

Jadepool SAAS API

Feel free to check out our [NodeJs SDK](https://github.com/nbltrust/jadepool-saas-sdk-nodejs), [Go SDK](https://github.com/nbltrust/jadepool-saas-sdk-golang).

# General Structure

```shell
$ git clone https://github.com/nbltrust/jadepool-saas-sdk-golang.git && cd jadepool-saas-sdk-golang
```

```javascript
const appKey = 'TyTLvCnHINbWZQag88hhmMz1'
const appSecret = 'uf0rPlTluGnIllGqx0X1os4hQ6rOdXDxStiN4qGd79lS6yeHZaOK4ldvRv1TBqr6'
const apiAddr = 'http://127.0.0.1:8092'

const api = new API(appKey, appSecret, apiAddr)
```

```go
// App Api
app := sdk.NewAppWithAddr(apiAddr, key, secret)
// Company Api
company := sdk.NewCompanyWithAddr(apiAddr, key, secret)
```

This is the general request structure for verifing the request by server.

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| timestamp | query/body | unix timestamp | Yes | string |
| nonce | query/body | nonce | Yes | string |
| sign | query/body | hex string, sign parameters with HMACSHA256 | Yes | string |

The paramters `timestamp`, `nonce` and `sign` are located in query for GET requests, but located in the body for POST requests.

The [signature method](#signature) is similar to OAuth.

# V1 API

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
  "delegateAmount": "0.000000000000000000",
  "delegateInLocked": "0.000000000000000000",
  "delegateOutLocked": "0.000000000000000000",
  "inLocked": "0.000000000000000000",
  "inLockedFee": "0.000000000000000000",
  "outLocked": "0.000000000000000000",
  "outLockedFee": "0.000000000000000000",
  "stakingBalance": "0.000000000000000000",
  "stakingInLocked": "0.000000000000000000",
  "stakingOutLocked": "0.000000000000000000",
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
delegateAmount | string | the coin delegate amount
delegateInLocked | string | the coin delegate transfer in amount under locked
delegateOutLocked | string | the coin delegate transfer out amount under locked
inLocked | string | the coin transfer in amount under locked
inLockedFee | string | the coin transfer in fee under locked
outLocked | string | the coin transfer out amount under locked
outLockedFee | string | the coin transfer out fee under locked
stakingBalance | string | the staking coin balance
stakingInLocked | string | the staking coin transfer in amount under locked
stakingOutLocked | string | the staking coin transfer out amount under locked
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
    "ETH"
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

**Summary:** get wallet assets

#### HTTP Request 
`GET /api/v1/app/assets` 

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
$ go run cmd/ctl/main.go "appkey" "appsecret" "GetStakingInterest" "IRIS" "2019-09-28"
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
        result = await api.getStakingInterest("IRIS", "2019-09-28")
        console.log(result)
    } catch(e) {
        // do something
        console.log(e)
    }
```

```go
	result, _ = app.GetStakingInterest("IRIS", "2019-09-28")
```

**Summary:** get staking interest

#### HTTP Request 
`GET /api/v1/staking/{coinName}/interest` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| X-App-Key | header | app key | Yes | string |
| coinName | path | coin type | Yes | string |
| date | query | date | Yes | string |

**Response Result**

Value | Type | Description
--------- | ------- | ---------
coinName | string | the coin name
value | string | the interest value

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
  "state": "PENDING",
  "memo": "",
  "value": "1.000000000000000000",
  "fee": "0.000000000000000000",
  "from": "0xF0706B7Cab38EA42538f4D8C279B6F57ad1d4072",
  "to": "0x29152c850456899A78178622B6543BBFfC224495",
  "txid": "0x8487e23bbf71f1763e015598283ae891cc5ea8d444f87a0a60a0b5eb7e1a4d59",
  "n": 0,
  "block": 13721091,
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
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
block | number | the block transaction mined in
memo | string | order note, editable on admin
n | number | the order index
sign | string | hex string, sign parameters with HMACSHA256

# Signature
1. Get the current timestamp (seconds) and the nonce (random string). Please make sure the timestamp error not exceed 5 minutes and the nonce not repeated in 10 minutes.

2. Form a String message (sorted) that contains the timestamp above. The message looks like this:
</br>
```
nonce=1559811763025698274450320&timestamp=1559811763
```

3. Sign the message using HMAC-SHA256. If the AppSecret is `yeTJ3EnOkyQQEjhTMVqn165Dqjp43bhTwXLIv25Ycdu8qwDOyqpa0WV54C6sO4HW`, the sign param would be like this:
</br>
```
a0e18ef88ff9d2fb8ad417dace09834632732de86ae97bbba84186375dd0c0a8
```

4. Send request as the same format as described in [General Structure](#general-structure).