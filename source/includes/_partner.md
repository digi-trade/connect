# 合作方API

合作方系统API是提供给合作方用来感知合作开户状态，系统基础设定，和非用户级别的设定。

## 获取合作方的配置


```shell
curl "https://partner.cabital.com/api/v1/config"
```

### HTTP 请求

`GET /api/v1/config`


> 获得以下JSON结构体:


```json
{
    "currencies": [
        {
            "symbol": "ETH",
            "type": 2,
            "deposit_methods": [
                "ERC20"
            ],
            "withdraw_methods": [
                "ERC20"
            ],
            "config": {
                "credit": {
                    "allow": true,
                    "min": "-1",
                    "max": "-1"
                },
                "debit": {
                    "allow": true,
                    "min": "0.02",
                    "max": "-1"
                },
                "conversion": {
                    "allow": true,
                    "min": "0.002",
                    "max": "100"
                }
            },
            "fees": {
                "debit_fee": {
                    "is_single": true,
                    "object": "customer",
                    "method": "fixed",
                    "value": "0.00025",
                    "original_value": "0.001"
                }
            }
        },
        {
            "symbol": "BTC",
            "type": 2,
            "deposit_methods": [
                "BTC"
            ],
            "withdraw_methods": [
                "BTC"
            ],
            "config": {
                "credit": {
                    "allow": true,
                    "min": "-1",
                    "max": "-1"
                },
                "debit": {
                    "allow": true,
                    "min": "0.001",
                    "max": "-1"
                },
                "conversion": {
                    "allow": true,
                    "min": "0.0002",
                    "max": "5"
                }
            },
            "fees": {
                "debit_fee": {
                    "is_single": true,
                    "object": "customer",
                    "method": "fixed",
                    "value": "0.00002",
                    "original_value": "0.0001"
                }
            }
        },
        {
            "symbol": "USDT",
            "type": 2,
            "deposit_methods": [
                "ERC20"
            ],
            "withdraw_methods": [
                "ERC20"
            ],
            "config": {
                "credit": {
                    "allow": true,
                    "min": "-1",
                    "max": "-1"
                },
                "debit": {
                    "allow": true,
                    "min": "40",
                    "max": "-1"
                },
                "conversion": {
                    "allow": true,
                    "min": "10",
                    "max": "200000"
                }
            },
            "fees": {
                "debit_fee": {
                    "is_single": true,
                    "object": "customer",
                    "method": "fixed",
                    "value": "0.99",
                    "original_value": "10"
                }
            }
        },
        {
            "symbol": "EUR",
            "type": 1,
            "deposit_methods": [
                "SEPA"
            ],
            "withdraw_methods": [
                "SEPA"
            ],
            "config": {
                "credit": {
                    "allow": true,
                    "min": "-1",
                    "max": "-1"
                },
                "debit": {
                    "allow": true,
                    "min": "25",
                    "max": "50000"
                },
                "conversion": {
                    "allow": true,
                    "min": "10",
                    "max": "200000"
                }
            }
        },
        {
            "symbol": "GBP",
            "type": 1,
            "deposit_methods": [
                "FPS"
            ],
            "withdraw_methods": [
                "FPS"
            ],
            "config": {
                "credit": {
                    "allow": true,
                    "min": "-1",
                    "max": "-1"
                },
                "debit": {
                    "allow": true,
                    "min": "20",
                    "max": "40000"
                },
                "conversion": {
                    "allow": true,
                    "min": "10",
                    "max": "200000"
                }
            }
        }
    ],
    "pairs": [
        {
            "pair": "BTC-EUR"
        },
        {
            "pair": "ETH-EUR"
        },
        {
            "pair": "EUR-USDT"
        },
        {
            "pair": "BTC-GBP"
        },
        {
            "pair": "ETH-GBP"
        },
        {
            "pair": "GBP-USDT"
        },
        {
            "pair": "BTC-ETH"
        },
        {
            "pair": "BTC-USDT"
        },
        {
            "pair": "ETH-USDT"
        }
    ]
}
```

### 组成部分

- currencies : 所有Connect使用到的货币配置
- paris: Convert使用的货币对，可用于报价的获取

### 货币配置定义

* 每个货币的基础配置

字段 | 类型 | 描述
--------- | ------- | ---------------
symbol | string(ENUM) | 货币标志
type | int(ENUM) | 货币类型，详见下面解释
deposit_methods | list | 当前入金方式
withdraw_methods | list | 当前出金方式
config | list(object) | 该币种的限制和配置
Fees | list(object) | 费用配置 

Config 对象定义

* debit 对象定义

字段 | 类型 | 描述
:-------- | ------- | ---------------
allow | bool | 允许（该允许使用）
min | string(number) | 交易最小每笔，`-1`表示不限制 
max | string(number) | 交易最大每笔，`-1`表示不限制 

* credit 对象定义

| 字段  | 类型           | 描述                         |
| :---- | -------------- | ---------------------------- |
| allow | bool           | 允许（该允许使用）           |
| min   | string(number) | 交易最小每笔，`-1`表示不限制 |
| max   | string(number) | 交易最大每笔，`-1`表示不限制 |

* conversion对象定义

| 字段  | 类型           | 描述               |
| :---- | -------------- | ------------------ |
| allow | bool           | 允许（该允许使用） |
| min   | string(number) | 转换货币最小每笔   |
| max   | string(number) | 转换货币最大每笔   |



Fees 内对象定义

* debit_fee 对象定义

| 字段               | 类型   | 描述                           |
| :----------------- | ------ | ------------------------------ |
| object             | sting  | 收费对象，Customer or Partner  |
| is_single          | bool | 收费类型Single or Tier         |
| method             | string | 收费方式：Fixed or Percent     |
| value              | string | 收费金额，百分比写具体比率数值 |
| original_value     | string | 展示金额，百分比写具体比率数值 |
| min_value          | string | 最小收费限额                   |
| original_min_value | string | 展示金额，百分比时使用         |
| max_value          | string | 最大收费限额                   |
| original_max_value | string | 展示金额，百分比时使用         |



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
    "quote": "3847.322377",
    "valid_until": 1638934081,
    "quote_id": "20211208032751:ETH-EUR:Customer",
    "valid_interval": 10,
    "reversed_quote": "0.00026253"
}
```
### 字段定义

字段 | 类型 | 描述
--------- | ------- | ---------------
valid_until | number(timestamp) | 以秒为单位从Unix Epoch到当前的数字，用来表示该报价的有效时间
quote_id | string | 报价的唯一ID，格式为 事件戳:报价货币对:报价目的
quote | string | 对Code-Pair为`CODE1-CODE2,`卖 CODE1 换取 CODE2 的报价 
valid_interval | number | 报价有效时间宽度，以秒记
reversed_quote | string(number) | 对Code-Pair为`CODE1-CODE2,`卖 CODE2 换取 CODE1 的报价 


<aside class="success">
法币的符号以 <a href="https://en.wikipedia.org/wiki/ISO_4217">ISO-4217</a> 为标准，数字货币依照 <a href="https://coinmarketcap.com/all/views/all/">CoinMarketCap</a> 列表
</aside>


## 获取最新的报价 (WS)

TBD

## 对账

为便于对账，合作方可通过第三方ID查询划转交易。

```shell
curl "/api/v1/recon/transfers/adb8f31d-7a71-4003-85d7-3ac58158461f"
```

> 返回JSON结构体:

```json
{
  "instruction_id": "3a344f80-f057-4841-b186-7a1daa0b8390",
  "account_id": "6d92e7b4-715c-4ce3-a028-19f1c8c9fa6c",
  "transfer_id": "4c416854-8970-4838-99ad-febc437ac81d",
  "instructed_amount": "1002.865",
  "customer_fee": "2.5",
  "actual_amount": "1000.365",
  "symbol": "USDT",
  "direction": "DEBIT",
  "conversion_id": "d81adf6d-0322-41d7-8c32-669203e35f11",
  "external_id": "adb8f31d-7a71-4003-85d7-3ac58158461f",
  "status": "SUCCESS",
  "created_at": 1633445162
}
```

### HTTP请求

`GET /api/v1/recon/transfers/<external_id>`

### URL参数

Parameter | 必须 |  Description
--------- | ------- |  -----------
external_id | true | 合作方的第三方ID

### 返回对账对象描述

字段 | 类型 | 描述
--------- | ------- | -----------
instruction_id | string(uuid) | 交易请求ID，对账用
account_id | string(uuid) | Cabital提供的账户ID
transfer_id | string(uuid) | 划转交易ID
instructed_amount | string(number) | 请求金额
customer_fee | string(number) | 收取客户的费用金额
actual_amount | string(number) | 客户实际收到的金额
symbol | string | 划转交易的货币
direction | string(enum) | 划转交易的方向，以Cabital为中心，`CREDIT`为充值，`DEBIT`为提款
conversion_id | string(uuid) | C+T关联交易中的转换订单ID
external_id | string(50) | 合作方的第三方ID
status | string(enum) | 划转交易的状态，`SUCCESS` / `FAILED` / `PROCESSING`
created_at | timestamp(number) | 划转交易创建时间

