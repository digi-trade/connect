# 合作方配置

合作方系统API是提供给合作方用来感知合作开户状态，系统基础设定，和非用户级别的设定。

## 获取合作方的配置


```shell
curl "http://partner.cabital.com/api/v1/config"
```

### HTTP 请求

`GET /api/v1/config`


> 获得以下JSON结构体:


```json
{
    "currencies":[
        {
            "symobl":"ETH",
            "type":2,
            "deposit_methods":[
                "ERC20",
                "SOL"
            ],
            "withdrew_methods":[
                "ERC20",
                "SOL"
            ],
            "config":{
                "debit":{
                    "min":"0.01",
                    "max":"5"
                },
                "credit":{
                    "allow":true,
                    "min":"0.01",
                    "max":"5"
                }
            }
        },
        {
            "symbol":"EUR",
            "type":1,
            "deposit_methods":[
                "SEPA"
            ],
            "withdrew_methods":[
                "SEPA"
            ],
            "config":{
                "debit":{
                    "allow":false
                },
                "credit":{
                    "allow":false
                },
                "conversion":{
                    "allow":true,
                    "min":"0.01",
                    "max":"5"
                }
            }
        }
    ],
    "conversions":[
        {
            "pair":"BTC-EUR",
            "sell_min":"0.0002",
            "sell_max":"5",
            "buy_min":"10",
            "buy_max":"200000"
        },
        {
            "pair":"ETH-EUR",
            "sell_min":"0.002",
            "sell_max":"100",
            "buy_min":"10",
            "buy_max":"200000"
        },
        {
            "pair":"EUR-USDT",
            "sell_min":"10",
            "sell_max":"200000",
            "buy_min":"10",
            "buy_max":"200000"
        },
        {
            "pair":"BTC-BGP",
            "sell_min":"0.0002",
            "sell_max":"5",
            "buy_min":"10",
            "buy_max":"200000"
        },
        {
            "pair":"ETH-BGP",
            "sell_min":"0.002",
            "sell_max":"100",
            "buy_min":"10",
            "buy_max":"200000"
        },
        {
            "pair":"GBP-USDT",
            "sell_min":"10",
            "sell_max":"200000",
            "buy_min":"10",
            "buy_max":"200000"
        }
    ]
}
```
### 字段定义

字段 | 类型 | 描述
--------- | ------- | ---------------
symbol | string(ENUM) | 货币标志
type | int(ENUM) | 货币类型，详见下面解释
deposit_methods | list | 当前入金方式
withdraw_methods | list | 当前出金方式
config | list(object) | 该币种的限制和配置
conversions | list(object) | 换币的限制和配置 

Config 对象定义

* debit 对象定义

字段 | 类型 | 描述
:-------- | ------- | ---------------
allow | bool | 允许（该允许使用）
min | string(number) | 交易最小每笔
max | string(number) | 交易最大每笔

* credit 对象定义

| 字段  | 类型           | 描述               |
| :---- | -------------- | ------------------ |
| allow | bool           | 允许（该允许使用） |
| min   | string(number) | 交易最小每笔       |
| max   | string(number) | 交易最大每笔       |


Conversions Array内对象定义

* Conversion 对象定义

| 字段     | 类型           | 描述                                       |
| :------- | -------------- | ------------------------------------------ |
| pair     | string         | 币种对，左卖右买如 'EUR-USDT'，卖EUR买USDT |
| sell_min | string(number) | 卖币交易最小每笔限额                       |
| sell_max | string(number) | 卖币交易最大每笔限额                       |
| buy_min  | string(number) | 买币交易最小每笔限额                       |
| buy_max  | string(number) | 买币交易最大每笔限额                       |



### Enum解释

- 货币类型 type

Bit位 | 描述
--------- | -----------
1 | 法币
2 | 数字货币

<aside class="success">
限额为整个合作方，单笔的限制
</aside>

## 获取最新的报价 (GET)

```shell
curl "http://partner.cabital.com/api/v1/quotes/USDT-EUR"
```
### HTTP 请求

`GET /api/v1/quotes/<code_pair>`

### URL参数

参数 | 是否必须 | 描述
--------- | ------- | -----------
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
quote | string | 报价的Pair，左侧为Base，右侧为Quote，报价以Cabital为中心
valid_interval | number | 报价有效时间宽度，以秒记
bid | string(number) | 卖 CODE2 换取 CODE1 的报价


<aside class="success">
法币的符号以 <a href="https://en.wikipedia.org/wiki/ISO_4217">ISO-4217</a> 为标准，数字货币依照 <a href="https://coinmarketcap.com/all/views/all/">CoinMarketCap</a> 列表
</aside>


## 获取最新的报价 (WS)

TBD