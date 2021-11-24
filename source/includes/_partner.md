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
            "currencies": [
                {
                    "symbol": "ETH",
                    "type": 2,
                    "is_enable": true,
                    "deposit_methods": [
                        "ERC20"
                    ],
                    "withdraw_methods": [
                        "ERC20"
                    ],
                    "limits": {
                        "deposit": {
                            
                            "min": "0",
                            "max": "0",
                            "is_enable": true
                        },
                        "withdraw": {
                            "min": "0.02",
                            "max": "0",
                            "is_enable": true
                        },
                        "transfer_deposit": {
                            
                            "min": "0",
                            "max": "0",
                            "is_enable": true
                        },
                        "transfer_withdraw": {
                            "min": "0.02",
                            "max": "0",
                            "is_enable": true
                        },
                        "conversion": {
                            
                            "min": "0.002",
                            "max": "100",
                            "is_enable": true
                        }
                    },
                    "fees": {
                        "deposit_fee_charges": [
                            {
                                "condition": "*",
                                "fee_charge": {
                                    "object": "customer",
                                    "type": "single",
                                    "method": "fixed",
                                    "value": "0.00025",
                                    "original_value": "0.001"
                                }
                            }
                        ],
                        "transfer_deposit_fee_charges": [
                            {
                                "condition": "*",
                                "fee_charge": {
                                    "object": "customer",
                                    "type": "single",
                                    "method": "fixed",
                                    "value": "0.00025",
                                    "original_value": "0.001"
                                }
                            }
                        ]
                    }
                },
                {
                    "symbol": "BTC",
                    "type": 2,
                    "is_enable": true,
                    "deposit_methods": [
                        "ERC20"
                    ],
                    "withdraw_methods": [
                        "ERC20"
                    ],
                    "limits": {
                        "deposit": {
                            
                            "min": "0",
                            "max": "0",
                            "is_enable": true
                        },
                        "withdraw": {
                            "min": "0.001",
                            "max": "0",
                            "is_enable": true
                        },
                        "transfer_deposit": {
                            
                            "min": "0",
                            "max": "0",
                            "is_enable": true
                        },
                        "transfer_withdraw": {
                            "min": "0.001",
                            "max": "0",
                            "is_enable": true
                        },
                        "conversion": {
                            
                            "min": "0.0002",
                            "max": "5",
                            "is_enable": true
                        }
                    },
                    "fees": {
                        "deposit_fee_charges": [
                            {
                                "condition": "*",
                                "fee_charge": {
                                    "object": "customer",
                                    "type": "single",
                                    "method": "fixed",
                                    "value": "0.00002",
                                    "original_value": "0.0001"
                                }
                            }
                        ],
                        "transfer_deposit_fee_charges": [
                            {
                                "condition": "*",
                                "fee_charge": {
                                    "object": "customer",
                                    "type": "single",
                                    "method": "fixed",
                                    "value": "0.00002",
                                    "original_value": "0.0001"
                                }
                            }
                        ]
                    }
                },
                {
                    "symbol": "USDT",
                    "type": 1,
                    "is_enable": true,
                    "deposit_methods": [
                        "ERC20"
                    ],
                    "withdraw_methods": [
                        "ERC20"
                    ],
                    "limits": {
                        "deposit": {
                            
                            "min": "0",
                            "max": "0",
                            "is_enable": true
                        },
                        "withdraw": {
                            "min": "40",
                            "max": "0",
                            "is_enable": true
                        },
                        "transfer_deposit": {
                            
                            "min": "0",
                            "max": "0",
                            "is_enable": true
                        },
                        "transfer_withdraw": {
                            "min": "40",
                            "max": "0",
                            "is_enable": true
                        },
                        "conversion": {
                            
                            "min": "10",
                            "max": "200000",
                            "is_enable": true
                        }
                    },
                    "fees": {
                        "deposit_fee_charges": [
                            {
                                "condition": "*",
                                "fee_charge": {
                                    "object": "customer",
                                    "type": "single",
                                    "method": "fixed",
                                    "value": "0.99",
                                    "original_value": "10"
                                }
                            }
                        ],
                        "transfer_deposit_fee_charges": [
                            {
                                "condition": "*",
                                "fee_charge": {
                                    "object": "customer",
                                    "type": "single",
                                    "method": "fixed",
                                    "value": "0.99",
                                    "original_value": "10"
                                }
                            }
                        ]
                    }
                },
                {
                    "symbol": "EUR",
                    "type": 1,
                    "is_enable": true,
                    "deposit_methods": [
                        "SEPA"
                    ],
                    "withdraw_methods": [
                        "SEPA"
                    ],
                    "limits": {
                        "deposit": {
                            
                            "min": "0",
                            "max": "0",
                            "is_enable": true
                        },
                        "withdraw": {
                            "min": "25",
                            "max": "50000",
                            "is_enable": true
                        },
                        "transfer_deposit": {
                            
                            "min": "0",
                            "max": "0",
                            "is_enable": true
                        },
                        "transfer_withdraw": {
                            "min": "25",
                            "max": "50000",
                            "is_enable": true
                        },
                        "conversion": {
                            
                            "min": "10",
                            "max": "200000",
                            "is_enable": true
                        }
                    },
                    "fees": {}
                },
                {
                    "symbol": "BGP",
                    "type": 1,
                    "is_enable": true,
                    "deposit_methods": [
                        "FPS"
                    ],
                    "withdraw_methods": [
                        "FPS"
                    ],
                    "limits": {
                        "deposit": {
                            
                            "min": "0",
                            "max": "0",
                            "is_enable": true
                        },
                        "withdraw": {
                            "min": "20",
                            "max": "40000",
                            "is_enable": true
                        },
                        "transfer_deposit": {
                            
                            "min": "0",
                            "max": "0",
                            "is_enable": true
                        },
                        "transfer_withdraw": {
                            "min": "20",
                            "max": "40000",
                            "is_enable": true
                        },
                        "conversion": {
                            "min": "10",
                            "max": "200000",
                            "is_enable": true
                        }
                    },
                    "fees": {}
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
Fees | list(object) | 费用配置 

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

* conversion对象定义

| 字段  | 类型           | 描述               |
| :---- | -------------- | ------------------ |
| allow | bool           | 允许（该允许使用） |
| min   | string(number) | 交易最小每笔       |
| max   | string(number) |                    |



Fees Array内对象定义

* Deposit_fee_charges Array内对象定义

| 字段       | 类型   | 描述                   |
| :--------- | ------ | ---------------------- |
| condition  | string | 匹配条件，* 表示全匹配 |
| fee_charge | object | 具体收费配置           |

*  Transfer_deposit_fee_charges Array内对象定义

| 字段       | 类型   | 描述                   |
| :--------- | ------ | ---------------------- |
| condition  | string | 匹配条件，* 表示全匹配 |
| fee_charge | object | 具体收费配置           |

* fee_charge 对象定义

| 字段           | 类型   | 描述                           |
| :------------- | ------ | ------------------------------ |
| is_percent     | bool   | 是否百分比                     |
| object         | sting  | 收费对象，Customer or Partner  |
| type           | string | 收费类型Single or Tier         |
| method         | string | 收费方式：Fixed or Percent     |
| value          | string | 收费金额，百分比写具体比率数值 |
| original_value | string | 展示金额，百分比写具体比率数值 |
| min            | string | 最小收费限额                   |
| max            | string | 最大收费限额                   |



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