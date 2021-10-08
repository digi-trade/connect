# 账户关联相关

FaaS的服务，依照Cabital的要求，接入合作方必须提供与本方一致的身份信息（每一个字段），字段阐述请参考关联同名账户API。

## 获取用户关联状况


```shell
curl "http://faas.cabital.com/faas/faas_partner/accounts/cdaa9983-9b8f-4478-ba60-896ac239879d/detail"
```

### HTTP 请求

`GET /faas/partners/<partner_id>/accounts/<account_id>/detail`

### URL参数

参数 | 是否必须 | 描述
--------- | ------- | -----------
partner_id | true | faas的partner id
account_id | true | faas的Cabital账户id

> 获得以下JSON结构体:

```json
{
  "match_status" : "MATCHED",
  "otp_ready" : true,
  "email_address" : "john.doe@email.com",
  "kyc_token" : "8yzhvhv87dahfb",
  "limits": {}
}
```


字段 | 类型 | 描述
--------- | ------- | ---------------
match_status | string(ENUM) | 身份匹配状态 `MATCHED,READY_FOR_MATCHING,PENDING,REJECTED`
otp_ready | bool | 客户是否已经在 Cabital 绑定完成OTP，其在提现的时候需要附上。
email_address | string | 用户在本方的 Email， 供合作方匹配
kyc_token | string | 用户在本方的 KYC Token 供合作方使用
limits | object | 当前提款限额 （待定）


<aside class="success">
单个账户的限额
</aside>

## 关联同名用户

```shell
curl "/faas/partners/faas_partner/accounts/cdaa9983-9b8f-4478-ba60-896ac239879d/match" \
  -X "POST" \
  -d '{ "name": "John Doe", "id": "J12345678D", "id_document": "PASSPORT", "dob": "19700101" }' 
```



### HTTP请求

`PUT /faas/partners/<partner_id>/account/match`

### URL参数

参数 | 是否必须 | 描述
--------- | ------- | -----------
partner_id | true | faas的partner id
account_id | true | faas的Cabital账户id

### 请求体

字段 | 类型 | 描述
--------- | ------- | ---------------
name | string | 客户本人的姓名，按照身份证件上的常用顺序，如First Name + Last Name
id | string | 身份文件上的ID
id_document | string(ENUM) | 身份文件类型 `ID,PASSPORT,NO_SUGGESTION`
dob | string | 身份文件上的生日（Date of birth）

> 得以下JSON结构体:

```json
{
  "result": "MISMATCH",
  "mismatch_fields": [
    "name",
    "dob"
  ]
}
```

提交用户信息，以获得同名验证结果


