# Callback event

Callback events are expected to arrive in 1-5 minutes on average, but in theory it may take up to 24 hours. If you miss the Callback event for some reason, don’t worry, we will resend the failed Callback event at any time. If the Callback event request fails, we will try to resend it four times: after 5 minutes, 1 hour, 5 hours, and 18 hours, until the request succeeds and obtains a 2XX return code.

We recommend that you wait no more than a day for the Callback event, and then send a request to our server to obtain status information about the resource.


## Account Connect status event

When the user initiates a connection from the counterparty, the system will send an account association status event based on the current connection status, which includes the following states:

Type | Description
--------- | -----------
INITIALIZED | The user is successfully registered and has not yet submitted KYC in Cabital
PENDING | Cabital processing user identification documents
TEMPORARY_REJECTED | The user is requested by Cabital to provide more materials
FINAL_REJECTED | The user was finally rejected by Cabital
CREATED | The user passed Cabital's KYC, and the Cabital account is opened, waiting for the partner to submit the same name verification.
MATCHING | The partner has been submitted, the same name verification is under manual review
MATCHED | Same name verification is passed, the same account transfer is fully availalbe
MISMATCHED | Same name verification rejection, multiple reasons
UNLINKED | The user / Cabital actively closes the connection with an account of the partner

<!-- READYFORMATCHING | 我方KYC通过，等待合作方提交同名验证 (以后）-->

When the status of the account connection changes, we will send you a POST request with a JSON payload to the URL provided to us during integration.

### JSON Payload

> JSON Payload
 
```json
{
    "account_uuid": "6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c",
    "user_email": "jone.doe@email.com",
    "user_ext_ref": "fce4fd45-7dd7-4d4c-b06c-e17ff12f3e30",
    "staus": "INITIALIZED",
    "event_time": "2020-07-01T09:00:00.000Z",
    "data" : {}
}
```

Field | Type | Required | Description
--------- | ------- | ------------|-----------
account_uuid | string | true | This party's account Id
user_email | string | true | The user email of the account
user_ext_ref | string | true | External Id entered by the partner
staus | string(enum) | true | status of account association
staus | datestamp | true | The time when the event occurred, in the format [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)
data | object | false | Some additional data associated with the account, such as Shared Token

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

Field | Type | Description
--------- | ------- | -----------
transfer_id | string(uuid) | 划转订单ID
amount | string(number) | 数量
symbol | string | 划转的货币
direction | string(enum) | 划转的方向，以Cabital为中心，`CREDIT`为充值，`DEBIT`为提款
conversion_id | string(uuid) | C+T关联交易中的转换订单ID，非必须
external_id | string(50) | 合作方的第三方ID，非必需
status | string(enum) | 划转的结果，SUCCESS / FAILED -->

## Event signature

Webhook signature rules are consistent with API authentication, please refer to [API authentication](#api)