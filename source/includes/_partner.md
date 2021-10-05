# 合作方配置

合作方系统API是提供给合作方用来感知合作开户状态，系统基础设定，和非用户级别的设定。

## 获取合作方的配置


```shell
curl "http://faas.cabital.com/faas/faas_partner/config"
```

### HTTP 请求

`GET /faas/partners/<partner_id>/config`

### URL参数

参数 | 是否必须 | 描述
--------- | ------- | -----------
partner_id | true | faas的partner id

> 获得以下JSON结构体:


```json
{
    "currencies" : [
        {
            "symobl": "ETH",
            "type": 2,
            "status": 15, // 1 + 2 + 4 + 8
            "deposit_methods": [
                "ERC20", "SOL"
            ],
            "withdrew_methods": [
                "ERC20", "SOL"
            ],
            "cap_limit": "500"
        },
        {
            "symobl": "EUR",
            "type": 1, 
            "status": 9, // 1 + 8
            "deposit_methods": [
                "SEPA"
            ],
            "withdrew_methods": [
                "SEPA"
            ],
            "cap_limit": "1000000"
        }
    ]
}
```
### 字段定义

字段 | 类型 | 描述
--------- | ------- | ---------------
match_status | string(ENUM) | 身份匹配状态 `MATCHED,READY_FOR_MATCHING,PENDING,REJECTED`
otp_ready | bool | 客户是否已经在 Cabital 绑定完成OTP，其在提现的时候需要附上。
email_address | string | 用户在本方的 Email， 供合作方匹配
limits | object | 当前提款限额 （待定）
### Enum解释

- 状态 Status

Staus 是一个以 Bit 位的复合字段，基于合作协议，以及 Cabital 在渠道上的状态。

Bit位 |  值 |  描述
--------- |  ---- |  -----------
1 | 1 |  开关
2 | 2 |  允许转出 (Cabile -> Partner)
3 | 4 |  允许转入 (Partner -> Cabile)
4 | 8 |  支持 Convert

- 货币类型 type


Bit位 | 描述
--------- | -----------
1 | 法币
2 | 数字货币

<aside class="success">
整个合作方的限额
</aside>

## 获取最新的报价 (GET)

```shell
curl "http://faas.cabital.com/faas/faas_partner/quotes/USDT-EUR"
```
### HTTP 请求

`GET /faas/partners/<partner_id>/quotes/<code_pair>`

### URL参数

参数 | 是否必须 | 描述
--------- | ------- | -----------
partner_id | true | faas的partner id
code_pair | true | 报价的CODE Pair，以`-`作为分割符，Cabital 系统会忽视传入的次序，并以自身规则进行排序，所以一个货币对，不论传入的次序和买卖关系，始终返回一种报价，比如USDT-EUR。

> 获得以下JSON结构体:


```json
{
    "ask": "0.864962",
    "valid_until": 1633426832,
    "quote_id": "20211005094027:USDT-EUR:Customer",
    "valid_interval": 5,
    "bid": "1.16424"
}
```
### 字段定义

字段 | 类型 | 描述
--------- | ------- | ---------------
ask | string(number) | 卖 CODE1 换取 CODE2 的报价
valid_until | number(timestamp) | 以秒为单位从Unix Epoch到当前的数字，用来表示该报价的有效时间
quote_id | string | 报价的唯一ID，格式为 事件戳:报价货币对:报价目的
valid_interval | number | 报价有效时间宽度，以秒记
bid | string(number) | 卖 CODE2 换取 CODE1 的报价


<aside class="success">
法币的符号以 <a href="https://en.wikipedia.org/wiki/ISO_4217">ISO-4217</a> 为标准，数字货币依照 <a href="https://coinmarketcap.com/all/views/all/">CoinMarketCap</a> 列表
</aside>


## 获取最新的报价 (WS)

TBD