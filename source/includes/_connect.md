# 账户关联相关

## 获取用户关联状况


```shell
curl "http://faas.cabital.com/faas/{partner_id}/limit" \
  -H "Authorization: Bearer"
```

### HTTP 请求

`GET /faas/partners/<partner_id>/limit`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
partner_id | true | faas的partner id

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

获取用户的Balance

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP请求

`GET /faas/partners/<partner_id>/balances`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
partner_id | true | faas的partner id


## 关联同名用户

```shell
curl "/faas/partners/{partner_id}/balances" \
  -H "Authorization: Bearer jwt_token"
```

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

获取用户的Balance

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP请求

`GET /faas/partners/<partner_id>/balances`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
partner_id | true | faas的partner id

## 获取用户的OTP状况

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -X DELETE \
  -H "Authorization: meowmeowmeow"
```



> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

