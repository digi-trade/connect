# 账户关联相关

## 获取用户关联状况


```shell
curl "http://faas.cabital.com/faas/{partner_id}/account/detail" \
  -H "Authorization: Bearer <TOKEN>"
```

### HTTP 请求

`GET /faas/partners/<partner_id>/account/detail`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
partner_id | true | faas的partner id


> 获得以下JSON结构体:

```json

```
<aside class="success">
用户级别的限额
</aside>

## 关联同名用户

```shell
curl "/faas/partners/{partner_id}/balances" \
  -X "POST" \
  -H "Authorization: Bearer jwt_token" \
  -d '{ "name": "John Doe", "id": "J12345678D", "id_document": "PASSPORT", "DOB": "19700101" }' 
```

> 得以下JSON结构体:

```json
{
  "result": "MISMATCH",
  "MISMATCH": [
    "name",
    "DOB"
  ]
}
```

提交用户信息，以获得同名验证结果


### HTTP请求

`POST /faas/partners/<partner_id>/account/match`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
partner_id | true | faas的partner id

## 获取用户的OTP状况



```shell
curl "http://faas.cabital.com//faas/partners/<partner_id>/account/OTP" \
  -H "Authorization: Bearer <TOKEN>"
```


> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```



### HTTP Request

`GET /faas/partners/<partner_id>/account/OTP`

### URL参数

Parameter | Default | Description
--------- | ------- | -----------
partner_id | true | faas的partner id
