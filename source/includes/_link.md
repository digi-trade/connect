# Account Link



`Account Link` 是一种特殊的 Connect 业务模式，在此模式下，业务对手方支持多对一（包含一对一）的账户关联关系。

## Account Link 关系:

`Account-Link`

```mermaid
erDiagram
	Cabital-Account ||--o{ Account-Link : OneToMany
	Cabital-Account{
		uuid account-id
	}
	Account-Link {
		uuid account-uuid
		uuid account-id
		string user_ext_ref
		date valid_until
		enum status
	}
	Partner-Account ||--|| Account-Link : OneToOne
	Partner-Account {
		string partner-id
	}
	
```



## Link的具体方式：

```mermaid
graph TD
st[Partner Login]
op[Access Cabital Connect]
check{Is Connected?}
auth_wall{Has Account?}
account_check{Valid Account Id?}
login[Login]
onboard[Onboading]
connect[Kick Off Connect]
partner_api[Call API with V-Account ID]
connected[Confirm Connection]
callback[V-Account Callback]
transfer[Transfer/Convert API]
%%{`token[Fetch Widget Token]widget[Launch Widget]paid[Transfer Callback]debit[Infinit Debit API]recon[Confirm by Recon API]`}%%
st:::partner-->op:::partner-->check:::partner
check-->|Yes|partner_api
check-->|No|connect
connect:::partner-->auth_wall
subgraph Cabital Connect
auth_wall-->|Yes|login
auth_wall-->|No|onboard
login-->connected
onboard-->connected
end
connected-.->callback:::partner
partner_api:::partner-->account_check
%%{`account_check-->|Yes-H5|token-- Token & Ext_id -->widget`}%%
account_check-->|Yes|transfer:::partner
account_check-->|No|connect
%%{`transfer-.->paid:::partner transfer-->debit:::partner paid-->recon`}%%

classDef partner fill:#7FFFD4;
```



## 开始link

```shell
curl "https://api.cabital.com/api/v1/partner/link"
```

### HTTP 请求

`GET /api/v1/partner/link?partner_key=<partner_key>&user_ext_ref=<external_id>&feature=xxx`

### URL参数

| 参数         | 是否必须 | 描述                                                         |
| ------------ | -------- | ------------------------------------------------------------ |
| partner_key  | true     | Cabital Issue给Partner唯一的标识Partner Key                  |
| user_ext_ref | true     | Partner在开始Link的时候唯一的user_ext_ref，在Partner侧具有唯一性 |
| feature      | false    | deeplink跳转，非必需，可以直接跳到2FA，KYC，或者Link确认页面 |
| redirect_url | true     | Cabital在Link成功后浏览器redirect，需要encoding              |
| siganture    | true     | 根据API Authentication的计算                                 |

> 获得以下Response


```json
Resposne Code : 303
Url : 我方提供的页面
```



## 获取用户关联状况


```shell
curl "http://partner.cabital.com/api/v1/link/links/cdaa9983-9b8f-4478-ba60-896ac239879d"
```

### HTTP 请求

`GET /api/v1/links/user_ext_ref`

### URL参数

| 参数         | 是否必须 | 描述                |
| ------------ | -------- | ------------------- |
| user_ext_ref | true     | Connect的外部账户id |

> 获得以下JSON结构体:

```json
{
  "account_uuid": "6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c",
  "link_status": "LINKED",
  "otp_ready": true,
  "kyc_status": "KYC_PASS",
  "email_address": "john.doe@email.com",
  "valid_until": 1648836455
}
```


| 字段          | 类型              | 描述                                                         |
| ------------- | ----------------- | ------------------------------------------------------------ |
| link_status   | string(ENUM)      | 账户关联状态                                                 |
| account_uuid  | string            | Link后的虚拟Account ID                                       |
| otp_ready     | bool              | 客户是否已经在 Cabital 绑定完成OTP，其在提现的时候需要附上   |
| kyc_status    | enum              | 客户的KYC状态，其决定了是否已经开户成功                      |
| email_address | string            | 用户在本方的 Email， 供合作方匹配                            |
| valid_until   | number(timestamp) | 以秒为单位从Unix Epoch到当前的数字，用来表示该Link的有效时间，非必需 |



## Partner提供接口（Callback）

```shell
curl -X PUT "http://api.partner.com/link"
```

### HTTP 请求

`PUT 合作方提供URL`

### 请求体

| 字段        | 类型              | 描述                                                         |
| ----------- | ----------------- | ------------------------------------------------------------ |
| user_ext_ref | string            | Partner在开始Link的时候唯一的external_id，在Partner侧具有唯一性 |
| account_uuid  | string            | Link后的虚拟Account ID                                       |
| staus      | string            | 当前Link状态的最终状态 |
| event_time     | timestamp | 事件产生的时间，格式为[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) |
| data -> valid_until | number(timestamp) | 以秒为单位从Unix Epoch到当前的数字，用来表示该Link的有效时间，非必需 |

### JSON Payload

```json
{
    "account_uuid": "6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c",
    "user_ext_ref": "fce4fd45-7dd7-4d4c-b06c-e17ff12f3e30",
    "staus": "LINKED",
    "event_time": "2020-07-01T09:00:00.000Z",
    "data" : {
      "valid_until": 1648836455
    }
}
```

### 重试规则

由于无法保证Partner的API可用性，所以在特殊情况下，Callback会按照标准Webhook的的重试机制总共尝试5次，直到第一次成功（2xx）或者全部失败，其频率参考Webhook的规则。

