# Webhook

webhook 预计平均会在 3-5 分钟后到达，但理论上可能需要长达 24 小时。如果由于某种原因您错过了 webhook，请不要担心：我们会记录尝试发送的所有内容，并且可以随时重新发送失败的 webhook。如果 webhook 请求失败，我们会尝试重新发送四次：5 分钟、1 小时、5 小时和 18 小时后，直到请求成功获得 2XX 返回码的。

我们建议您等待 webhook 不超过一天，然后向我们的服务器发送请求以获取有关资源的状态信息。

## Webhooks Event types

类型 | 描述
--------- | -----------
Linked | 用户成功连接后通知
Created | 用户成功开户后通知，本方账户开通
ReadyForMatching | 我方KYC通过，等待合作方提交同名验证
Matched | 同名验证通过，完全开通同账户转账
Rejected | 同名验证拒绝，多种因素，

## Webhook Payload

### Normal Payload

> HTTP Payload
 
```json
{
    "caseSystemId": "6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c",
    "externalCaseId": "243d19cf-562f-4060-89fa-1d35a7723c3e"
}
```

`CaseCreated` `CasePending` webhook payload

字段 | 类型 | 必须 | 描述
--------- | ------- | ------------|-----------
externalCaseId | string | true | 客户填写的外部Id
caseSystemId | string | true | Case的system uuid

### Reviewed Payload

> HTTP Payload
 
```json
{
    "caseSystemId": "6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c",
    "externalCaseId": "243d19cf-562f-4060-89fa-1d35a7723c3e",
    "suggestion": "SUGGEST_TO_REJECT",
    "comment": "User is okay to me."
}
```

字段 | 类型 | 必须 | 描述
--------- | ------- | ------------|-----------
externalCaseId | string | true | 客户填写的外部Id
caseSystemId | string | true | Case的system uuid
suggestion | string(ENUM) | true | 系统建议 `SUGGEST_TO_ACCEPT,SUGGEST_TO_REJECT,NO_SUGGESTION`
comment | string | false| 系统建议的人工备注

### Completed Payload

> HTTP Payload
 
```json
{
    "caseSystemId": "6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c",
    "externalCaseId": "243d19cf-562f-4060-89fa-1d35a7723c3e",
    "decision": "ACCEPT",
    "comment": "User is okay to me."
}
```

字段 | 类型 | 必须 | 描述
--------- | ------- | ------------|-----------
externalCaseId | string | true | 客户填写的外部Id
caseSystemId | string | true | Case的system uuid
decision | string(ENUM) | true | 决策 `ACCEPT,REJECT`
comment | string | false| 决策备注

case的状态发生变化时，我们将向您发送带有 JSON 负载的 POST 请求到集成时提供给我们的 URL。

## Webhook Signature

Webhook 签名规则与API认证一致，请参考[API认证](#api)
