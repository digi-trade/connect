# Account Operations

List of Account Operation Business Errors

Error | Description
---------- | -------
PA001 | Cabital account id does not exist
PA002 | Insufficient account available balance
PA003 | The transaction has not reached the minimum limit
PA004 | The transaction exceeds the maximum limit
PA005 | The requested transaction type is not available
PA006 | The currency of the requested transaction request is not available
PA007 | Quote has been expired
PA008 | Incorrect 2FA (OTP)
PA009 | The customer’s KYC status has not yet been matched
PA010 | 2FA (OTP) is not set properly
PA011 | Convert's Quote usage is incorrect (Invalid Calculation)

<!-- ## 获取用户提现限额


```shell
curl "http://partner.cabital.com/api/v1/limit" \
  -H "Authorization: Bearer"
```

### HTTP 请求

`GET /api/v1/limit`


> Response Sample:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```
<aside class="success">
用户级别的限额
</aside>

获取该账户的提款限额（根据账户的AML规则）



> Response Sample:

```json
[
  {
    "code": "BTC",
    "balances": "1",
    "allow_debit": true,
    "allow_credit": true
  },
  {
    "code": "ETH",
    "balances": "1",
    "allow_debit": true,
    "allow_credit": true
  },
  {
    "code": "USDT",
    "balances": "1",
    "allow_debit": true,
    "allow_credit": true
  },
  {
    "code": "EUR",
    "balances": "1",
    "allow_debit": false,
    "allow_credit": false
  },
  {
    "code": "GBP",
    "balances": "1.0",
    "allow_debit": false,
    "allow_credit": false
  }
]
```

获取用户的所有账户余额

### HTTP Request

`GET /api/v1/accounts/<account_id>/balances`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital Connect Account Id -->


## List of Available Account Balances

Get all account balances of the user

```shell
curl "/api/v1/accounts/{account_id}/balances"
```

> Response Sample:

```json
[
  {
    "code": "BTC",
    "balances": "0.12345678"
  },
  {
    "code": "ETH",
    "balances": "1.000000000"
  },
  {
    "code": "USDT",
    "balances": "1200.000000"
  },
  {
    "code": "EUR",
    "balances": "0.00"
  },
  {
    "code": "GBP",
    "balances": "0.00"
  }
]
```

### HTTP Request

`GET /api/v1/accounts/<account_id>/balances`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital Connect Account Id

## Available Account Balance in a Single Currency

Get the user's single account balance, please refer to the currency symbol [Quotation API](/connect/en/#get-the-latest-quotation-get)

```shell
curl "/api/v1/accounts/6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c/balances/BTC"
```

> Response Sample:

```json
{
  "code": "BTC",
  "balances": "0.12345678"
}
```

### HTTP Request

`GET /api/v1/accounts/<account_id>/balances/<symbol>`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital Connect Account Id
symbol | true | Currency Symbol


## Account Currency Deposite Information

Get account currency deposite information

```shell
curl "/api/v1/accounts/6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c/balances/EUR/deposit/SEPA"
```

> Response Sample:

```json
{
  "method": "SEPA",
  "meta": {
    "account_name": "Cabital Fintech (LT) UAB",
    "iban": "LT493780000074448705",
    "ref_code": "N6FH7B",
    "bic": "TRYULT21XXX",
    "bank_name": "Transactive Systems UAB",
    "bank_address": "Verkių g. 25C-1, Vilnius",
    "bank_country": "Lithuania"
  }
}
```

### HTTP Request

`GET /api/v1/accounts/<account_id>/balances/<symbol>/deposit/<method>`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital Connect Account Id
symbol | true | Currency symbol
method | false | The deposit method currency is not required. If missing, the default method will be used. Please use the method defined in[Configuration API](/?shell#79fee25901)

## Currency Conversion 

Through the method of conversion between Cabital's account balances, exchange different currencies. By converting currencies, you can trade currencies that Cabital does not support transfer by converting first and then transferring (C+T)

You can exchange different currencies. By converting currencies, you can also trade currencies that Cabital does not support by converting them first to a supporting currency and then transferring (convert and then transfer).

```shell
curl -X POST "/api/v1/accounts/6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c/conversions"
-d '{
    "quote_id": "20210416225049:BTC-USDT:Customer",
    "quote": "61426.365",
    "pair": "BTC-USDT",
    "buy_amount": "0.001",
    "sell_amount": "61.426365",
    "major_ccy": "BTC"
}'
```

> Request Sample:

```json
{
    "quote_id": "20210416225049:BTC-USDT:Customer",
    "quote": "61426.365",
    "pair": "BTC-USDT",
    "buy_amount": "0.001",
    "sell_amount": "61.426365",
    "major_ccy": "BTC"
}
```

> Response Sample:

```json
{
  "transaction_id": "d81adf6d-0322-41d7-8c32-669203e35f11",
  "status": "SUCCESS"
}
```

### HTTP Request

`POST /api/v1/accounts/<account_id>/conversions`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital Connect Account Id


### Request Object

Field | Type | Description
--------- | ------- | -----------
quote_id | string | The unique identifier of the quote
quote | string(number) | Qsuote
pair | string | The currency pair that is to be quoted. The left side repesents what is being bought. The right side represents what is being sold. 
buy_amount | string(number) | Buy amount
sell_amount | string(number) | Sell amount
major_ccy | string | Major currency, used to determine the conversion quota

<aside class="warning">
<b>Attention：</b>
<ul>
  <li>Be sure to pay attention to the direction of the pair. The left side repesents what is being bought. The right side represents what is being sold. </li>
  <li>Because the quotation supports Ask & Bid, no matter if you are buying or selling, it will be the same quotation pair. </li>
</ul>
</aside>

## Account Transfer History

Query transfer history records. Filter query and pagination are supported.

```shell
curl "/api/v1/accounts/6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c/transfers"
```

> Response Sample:

```json
{
  "pagination_response": {
    "cursor": 0
  },
  "transfers":[
    {
      "transfer_id": "4c416854-8970-4838-99ad-febc437ac81d",
      "amount": "1000.365",
      "symbol": "USDT",
      "direction": "DEBIT",
      "conversion_id": "d81adf6d-0322-41d7-8c32-669203e35f11",
      "external_id": "adb8f31d-7a71-4003-85d7-3ac58158461f",
      "created_at": 1633445162
    },
    {
      "transfer_id": "4c416854-8971-4838-99ad-febc437ac81d",
      "amount": "0.365",
      "symbol": "BTC",
      "direction": "CREDIT",
      "conversion_id": "d81adf6d-0322-41d7-8c32-669203e35f11",
      "external_id": "adb8f31d-7a71-4003-85d7-3ac58158461f",
      "created_at": 1633445160
    },
  ]
}
```

### HTTP Request

`GET /api/v1/accounts/<account_id>/transfers`

### URL Parameters

Parameter | Required | Default | Description
--------- | ------- | ----------- | -----------
account_id | true | -- | Cabital Connect Account Id
direction | false | 2 Ways | Filter by direction `CREDIT` / `DEBIT`
symbol | false | all currencies | Filter for currencies
cursor | false | 0 | Result set cursor
page_size | false | 10 | Page size（1-30）
has_conversion | false | both | Bool type (filter whether there are related conversion orders)
created_from | false | 0 | Create order start time (Unix Time Epoch in seconds)
created_to | false | NOW | Create order end time (Unix Time Epoch in seconds)

### Response Object

Field | Type | Description
--------- | ------- | -----------
transfer_id | string(uuid) | Transfer order id
amount | string(number) | Amount
symbol | string | Currency symbol
direction | string(enum) | The direction of the transfer to Cabital. `CREDIT` for top up into the Cabital Account. `DEBIT` for withdraw out of the Cabital Account.
conversion_id | string(uuid) | The conversion order ID in C+T related transactions, optional
external_id | string(50) | The third-party ID of the partner
status | string(enum) | The result of the transfer (SUCCESS / FAILED)


## Account Transfer Details

Transfer Details

```shell
curl "/api/v1/accounts/6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c/transfers/4c416854-8970-4838-99ad-febc437ac81d"
```

> Response Sample:

```json
{
  "transfer_id": "4c416854-8970-4838-99ad-febc437ac81d",
  "amount": "1000.365",
  "symbol": "USDT",
  "direction": "DEBIT",
  "conversion_id": "d81adf6d-0322-41d7-8c32-669203e35f11",
  "external_id": "adb8f31d-7a71-4003-85d7-3ac58158461f",
  "created_at": 1633445162,
  "status": "SUCCESS"
}
```

### HTTP Request

`GET /api/v1/accounts/<account_id>/transfers/<trasfer_id>`

### URL Parameters

Parameter | Required |  Description
--------- | ------- |  -----------
account_id | true | Cabital Connect Account Id
trasfer_id | true | Transfer order id

### Response Object

Field | Type | Description
--------- | ------- | -----------
transfer_id | string(uuid) | Transfer order id
amount | string(number) | Amount
symbol | string | Currency symbol
direction | string(enum) | The direction of the transfer to Cabital. CREDIT for top up into the Cabital Account. DEBIT for withdraw out of the Cabital Account.
conversion_id | string(uuid) | The conversion order ID in Convert+Transfer related transactions (optional)
external_id | string(50) | The third-party ID of the partner, not required
status | string(enum) | The result of the transfer, SUCCESS / FAILED

## Two-way Account Transfer

Transfer between Cabital and the partner’s account under the same name.

```shell
curl -X POST "/api/v1/accounts/6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c/transfers"
-d '{
    "amount": "1000.365",
    "symbol": "USDT",
    "direction": "debit",
    "conversion_id": "d81adf6d-0322-41d7-8c32-669203e35f11",
    "external_id": "adb8f31d-7a71-4003-85d7-3ac58158461f"
}'
```

> Request Sample:

```json
{
    "amount": "1000.365",
    "symbol": "USDT",
    "direction": "debit",
    "conversion_id": "d81adf6d-0322-41d7-8c32-669203e35f11",
    "external_id": "adb8f31d-7a71-4003-85d7-3ac58158461f"
}
```

> Response Sample:

```json
{
  "transfer_id": "4c416854-8970-4838-99ad-febc437ac81d",
  "status": "SUCCESS"
}
```

### HTTP Request

`POST /api/v1/accounts/<account_id>/transfers`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital Connect Account Id


### Request Object

Field | Type | Description
--------- | ------- | -----------
amount | string(number) | Amount
symbol | string | Currency symbol
otp | string | OTP Number (6 digits from Google Authenticator)
direction | string(enum) | The direction of the transfer to Cabital. `CREDIT` for top up into the Cabital Account. `DEBIT` for withdraw out of the Cabital Account.
conversion_id | string(uuid) | The conversion order ID in C+T related transactions, optional
external_id | string(50) | The third-party ID of the partner, must be unique for one partner



### Response Object

Field | Type | Description
--------- | ------- | -----------
transfer_id | string(uuid) | Transfer id
external_id | string(50) |  The third-party ID of the partner, must be unique for one partner
status | string(enum) | The result of the transfer (SUCCESS / FAILED)