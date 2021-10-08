# Callback 事件

Callback 事件预计平均会在 1-5 分钟后到达，但理论上可能需要长达 24 小时。如果由于某种原因您错过了 Callback 事件，请不要担心：我们会记录尝试发送的所有内容，并且可以随时重新发送失败的 Callback 事件。如果 Callback 事件请求失败，我们会尝试重新发送四次：5 分钟、1 小时、5 小时和 18 小时后，直到请求成功获得 2XX 返回码的。

我们建议您等待 Callback 事件不超过一天，然后向我们的服务器发送请求以获取有关资源的状态信息。


## Name Match 事件

当用户从对手方发起连接后，系统会根据现在的关联状态发送 Name Match 事件，其包含以下几个状态：

类型 | 描述
--------- | -----------
LINKED | 用户成功连接后通知
CREATED | 用户成功开户后通知，本方账户开通
READYFORMATCHING | 我方KYC通过，等待合作方提交同名验证
MATCHED | 同名验证通过，完全开通同账户转账
REJECTED | 同名验证拒绝，多种因素
UNLINKED | 用户主动关闭与合作方的账户关联

账户关联的状态发生变化时，我们将向您发送带有 JSON 负载的 POST 请求到集成时提供给我们的 URL。
### 事件内容 JSON Payload

> JSON Payload
 
```json
{
    "account_uuid": "6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c",
    "user_email": "jone.doe@email.com",
    "user_ext_ref": "partner_id",
    "staus": "LINKED",
    "event_time": "2020-07-01T09:00:00.000Z",
    "data" : {}
}
```

字段 | 类型 | 必须 | 描述
--------- | ------- | ------------|-----------
account_uuid | string | true | 本方的账户Id
user_email | string | true | 账户的主用户email
user_ext_ref | string | true | 合作方填入的外部Id
staus | string(enum) | true | 账户关联的状态
staus | datestamp | true | 事件产生的时间，格式为[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)
data | object | false | 账户关联的一些额外数据，比如Shared Token

## 事件签名

Webhook 签名规则与API认证一致，请参考[API认证](#api)