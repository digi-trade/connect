# 账户操作相关

<!-- ## 获取用户提现限额


```shell
curl "http://faas.cabital.com/api/v1/limit" \
  -H "Authorization: Bearer"
```

### HTTP 请求

`GET /api/v1/limit`


> 获得以下JSON结构体:

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



> 获得以下JSON结构体:

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

### HTTP请求

`GET /api/v1/accounts/<account_id>/balances`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital提供的账户id -->


## 账户可用余额列表

获取用户的所有账户余额

```shell
curl "/api/v1/accounts/{account_id}/balances"
```

> 获得以下JSON结构体:

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

### HTTP请求

`GET /api/v1/accounts/<account_id>/balances`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital提供的账户id

## 账户可用余额单币

获取用户的单一账户余额，货币符号请参考[报价API](/?shell#get)

```shell
curl "/api/v1/accounts/6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c/balances/BTC"
```

> 获得以下JSON结构体:

```json
{
  "code": "BTC",
  "balances": "0.12345678"
}
```

### HTTP请求

`GET /api/v1/accounts/<account_id>/balances/<symbol>`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital提供的账户id
symbol | true | 货币的Symbol


## 账户单币入账信息

获取用户的单一账户入账信息

```shell
curl "/api/v1/accounts/6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c/balances/EUR/deposit/SEPA"
```

> 获得以下JSON结构体:

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

### HTTP请求

`GET /api/v1/accounts/<account_id>/balances/<symbol>/deposit/<method>`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital提供的账户id
symbol | true | 货币的Symbol
method | false | 入币方式，非必须，如缺失将使用默认方法，具体方法请参考[配置API](/?shell#79fee25901)

## 账户转换货币

在Cabital的账户余额间通过转换的方法，兑换不同种的货币。通过转换货币，可以将Cabital方不支持划转的货币，通过先转换后划转的方式进行交易(C+T)

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

> 提交JSON结构体:

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

> 获得以下JSON结构体:

```json
{
  "transaction_id": "d81adf6d-0322-41d7-8c32-669203e35f11",
  "status": "SUCCESS"
}
```

### HTTP请求

`POST /api/v1/accounts/<account_id>/conversions`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital提供的账户id


### 提交参数

字段 | 类型 | 描述
--------- | ------- | -----------
quote_id | string | 报价的唯一标识符
quote | string(number) | 报价
pair | string | 报价的货币对，左侧为买，右侧为卖
buy_amount | string(number) | 买入数量
sell_amount | string(number) | 卖出数量
major_ccy | string | 主要货，用于转换限额判定

<aside class="warning">
<b>注意事项：</b>
<ul>
  <li>一定要注意pair的方向，左侧为买入货币，右侧为卖出货币。</li>
  <li>由于报价支持正反向（Ask & Bid），不论买入还是卖出，皆为同一个报价对。</li>
</ul>
</aside>

### 错误处理
TBD


## 账户划转列表

查询划转历史记录，支持参数查询和分页。

```shell
curl "/api/v1/accounts/6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c/transfers"
```

> 返回JSON结构体:

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

### HTTP请求

`GET /api/v1/accounts/<account_id>/transfers`

### URL参数

Parameter | 必须 | 默认值 | Description
--------- | ------- | ----------- | -----------
account_id | true | -- | Cabital提供的账户id
direction | false | 全部 | 方向过滤
symbol | false | 全部 | 币种过滤
cursor | false | 0 | 查询结果集的游标位置
page_size | false | 10 | 取值范围为（1-30）
has_coversion | false | 全部 | bool型，过滤是否有相关转换订单
created_from | false | 0 | 创建订单起始时间（Unix Time Epoch的秒数）
created_to | false | NOW | 创建订单结束时间（Unix Time Epoch的秒数）

### 返回transfers对象描述

字段 | 类型 | 描述
--------- | ------- | -----------
transfer_id | string(uuid) | 划转订单ID
amount | string(number) | 数量
symbol | string | 划转的货币
direction | string(enum) | 划转的方向，以Cabital为中心，`CREDIT`为充值，`DEBIT`为提款
conversion_id | string(uuid) | C+T关联交易中的转换订单ID，非必须
external_id | string(50) | 合作方的第三方ID，非必需
status | string(enum) | 划转的结果，SUCCESS / FAILED
## 账户划转详情

在 Cabital 与合作方的同名账户之间进行划转。

```shell
curl "/api/v1/accounts/6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c/transfers/4c416854-8970-4838-99ad-febc437ac81d"
```

> 返回JSON结构体:

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

### HTTP请求

`GET /api/v1/accounts/<account_id>/transfers/<trasfer_id>`

### URL参数

Parameter | 必须 |  Description
--------- | ------- |  -----------
account_id | true | Cabital提供的账户id
trasfer_id | true | 划转订单id

### 返回transfers对象描述

字段 | 类型 | 描述
--------- | ------- | -----------
transfer_id | string(uuid) | 划转订单ID
amount | string(number) | 数量
symbol | string | 划转的货币
direction | string(enum) | 划转的方向，以Cabital为中心，`CREDIT`为充值，`DEBIT`为提款
conversion_id | string(uuid) | C+T关联交易中的转换订单ID，非必须
external_id | string(50) | 合作方的第三方ID，非必需
status | string(enum) | 划转的结果，SUCCESS / FAILED
## 账户双向划转

在 Cabital 与合作方的同名账户之间进行划转。

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

> 提交JSON结构体:

```json
{
    "amount": "1000.365",
    "symbol": "USDT",
    "direction": "debit",
    "conversion_id": "d81adf6d-0322-41d7-8c32-669203e35f11",
    "external_id": "adb8f31d-7a71-4003-85d7-3ac58158461f"
}
```

> 获得以下JSON结构体:

```json
{
  "transfer_id": "4c416854-8970-4838-99ad-febc437ac81d",
  "status": "SUCCESS"
}
```

### HTTP请求

`POST /api/v1/accounts/<account_id>/transfers`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
account_id | true | Cabital提供的账户id


### 提交对象描述

字段 | 类型 | 描述
--------- | ------- | -----------
amount | string(number) | 数量
symbol | string | 划转的货币
otp | string | OTP的数值
direction | string(enum) | 划转的方向，以Cabital为中心，`CREDIT`为充值，`DEBIT`为提款
conversion_id | string(uuid) | C+T关联交易中的转换订单ID，非必须
external_id | string(50) | 合作方的唯一订单号，如重复订单将拒绝

### 返回对象描述

字段 | 类型 | 描述
--------- | ------- | -----------
transfer_id | string(uuid) | 划转订单ID
external_id | string(50) | 
status | string(enum) | 划转的结果

<!-- ### OTP的使用！！！ -->

### 失败原因

- 305 ： OTP 缺失