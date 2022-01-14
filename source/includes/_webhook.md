# Callback 事件

Callback 事件预计平均会在 1-5 分钟后到达，但理论上可能需要长达 24 小时。如果由于某种原因您错过了 Callback 事件，请不要担心：我们会记录尝试发送的所有内容，并且可以随时重新发送失败的 Callback 事件。如果 Callback 事件请求失败，我们会尝试重新发送四次：5 分钟、1 小时、5 小时和 18 小时后，直到请求成功获得 2XX 返回码的。

我们建议您等待 Callback 事件不超过一天，然后向我们的服务器发送请求以获取有关资源的状态信息。


## 账户关联状态事件

当用户从对手方发起连接后，系统会根据现在的关联状态发送账户关联状态事件，其包含以下几个状态：

类型 | 描述
--------- | -----------
INITIALIZED | 用户成功连接，还未在 Cabital 提交 KYC
PENDING | Cabital处理用户材料中
TEMPORARY_REJECTED | 用户被 Cabital 要求提供正确材料
FINAL_REJECTED | 用户被 Cabital 最终拒绝开户
CREATED | 用户成功 KYC，Cabital 账户开通，等待合作方提交同名验证。
MATCHED | 同名验证通过，完全开通同账户转账
MISMATCHED | 同名验证拒绝，多种因素
UNLINKED | 用户/ Cabital 主动关闭与合作方的某账户关联

<!-- READYFORMATCHING | 我方KYC通过，等待合作方提交同名验证 (以后）-->

账户关联的状态发生变化时，我们将向您发送带有 JSON 负载的 POST 请求到集成时提供给我们的 URL。

### 事件内容 JSON Payload

> JSON Payload

```json
{
    "account_uuid": "6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c",
    "user_ext_ref": "fce4fd45-7dd7-4d4c-b06c-e17ff12f3e30",
    "staus": "INITIALIZED",
    "event_time": "2020-07-01T09:00:00.000Z",
    "data" : {}
}
```

字段 | 类型 | 必须 | 描述
--------- | ------- | ------------|-----------
account_uuid | string | true | 本方的账户Id
user_ext_ref | string | true | 合作方填入的外部Id
staus | string(enum) | true | 账户关联的状态
event_time | datestamp | true | 事件产生的时间，格式为[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)
data | object | false | 账户关联的一些额外数据，比如Mis-matched fields. 

**用户unlink后再reconnect状态变化表**

| 用户状态           | reconnect过程事件流                          | 备注 |
| ------------------ | ---------------------------------------------- | ---- |
| INITIALIZED        | UNLINKED ==》INITIALIZED                       |      |
| PENDING            | UNLINKED ==》INITIALIZED==》PENDING            |      |
| TEMPORARY_REJECTED | UNLINKED ==》INITIALIZED==》TEMPORARY_REJECTED |      |
| FINAL_REJECTED     | UNLINKED ==》INITIALIZED==》FINAL_REJECTED     |      |
| CREATED            | UNLINKED ==》INITIALIZED==》CREATED            |      |
| MATCHED            | UNLINKED ==》INITIALIZED==》CREATED            |  需要重新match    |
| MISMATCHED         | UNLINKED ==》INITIALIZED==》CREATED            |  需要重新match    |


<!-- ## Transfer事件

当对手方发起划转后，系统会根据现在的划转状态发送 Transfer事件 事件，其包含以下几个状态：

- SUCCESS
- FAILED

<aside class="notice">
通常不需要监听 Transfer 事件，因为Transfer结果是同期返回给对手方的。
</aside>
### 事件内容 JSON Payload

> JSON Payload

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

字段 | 类型 | 描述
--------- | ------- | -----------
transfer_id | string(uuid) | 划转订单ID
amount | string(number) | 数量
symbol | string | 划转的货币
direction | string(enum) | 划转的方向，以Cabital为中心，`CREDIT`为充值，`DEBIT`为提款
conversion_id | string(uuid) | C+T关联交易中的转换订单ID，非必须
external_id | string(50) | 合作方的第三方ID，非必需
status | string(enum) | 划转的结果，SUCCESS / FAILED -->

## 事件签名

Webhook 签名规则与API认证一致，请参考[API认证](#api)