# 轻量化集成

部分合作方，希望以最快的方法集成Cabital Connect，在一开始可以采用轻量化的方式进行集成。



## 获取合作方的配置


```shell
curl "https://api.cabital.com/api/v1/light/config"
```

### HTTP 请求

`GET /api/v1/light/config`


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
                "deposit": {
                    "min": "-1",
                    "max": "-1"
                },
                "withdrew": {
                    "min": "0.02",
                    "max": "-1"
                },
                "conversion": {
                    "min": "0.002",
                    "max": "100"
                }
            },
            "fees": {
                "withdrew_fee": {
                    "is_single": true,
                    "object": "customer",
                    "method": "fixed",
                    "value": "0.00025"
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
                "deposit": {
                    "min": "-1",
                    "max": "-1"
                },
                "withdrew": {
                    "min": "0.001",
                    "max": "-1"
                },
                "conversion": {
                    "min": "0.0002",
                    "max": "5"
                }
            },
            "fees": {
                "withdrew_fee": {
                    "is_single": true,
                    "object": "customer",
                    "method": "fixed",
                    "value": "0.00002"
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
                "deposit": {
                    "min": "-1",
                    "max": "-1"
                },
                "withdrew": {
                    "min": "40",
                    "max": "-1"
                },
                "conversion": {
                    "min": "10",
                    "max": "200000"
                }
            },
            "fees": {
                "withdrew_fee": {
                    "is_single": true,
                    "object": "customer",
                    "method": "fixed",
                    "value": "0.99"
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
                "deposit": {
                    "min": "-1",
                    "max": "-1"
                },
                "withdrew": {
                    "min": "25",
                    "max": "50000"
                },
                "conversion": {
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
                "deposit": {
                    "min": "-1",
                    "max": "-1"
                },
                "debit": {
                    "min": "20",
                    "max": "40000"
                },
                "conversion": {
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
- pairs: Convert使用的货币对，可用于报价的获取

### 货币配置定义

* 每个货币的基础配置

| 字段             | 类型         | 描述                   |
| ---------------- | ------------ | ---------------------- |
| symbol           | string(ENUM) | 货币标志               |
| type             | int(ENUM)    | 货币类型，详见下面解释 |
| deposit_methods  | list         | 当前入金方式           |
| withdraw_methods | list         | 当前出金方式           |
| config           | list(object) | 该币种的限制和配置     |
| Fees             | list(object) | 费用配置               |

Config 对象定义

* Withdrew 对象定义

| 字段 | 类型           | 描述                         |
| :--- | -------------- | ---------------------------- |
| min  | string(number) | 交易最小每笔，`-1`表示不限制 |
| max  | string(number) | 交易最大每笔，`-1`表示不限制 |

* Deposit 对象定义

| 字段 | 类型           | 描述                         |
| :--- | -------------- | ---------------------------- |
| min  | string(number) | 交易最小每笔，`-1`表示不限制 |
| max  | string(number) | 交易最大每笔，`-1`表示不限制 |

* conversion对象定义

| 字段 | 类型           | 描述             |
| :--- | -------------- | ---------------- |
| min  | string(number) | 转换货币最小每笔 |
| max  | string(number) | 转换货币最大每笔 |



Fees 内对象定义

* debit_fee 对象定义

| 字段      | 类型   | 描述                           |
| :-------- | ------ | ------------------------------ |
| object    | sting  | 收费对象，Customer or Partner  |
| is_single | bool   | 收费类型Single or Tier         |
| method    | string | 收费方式：Fixed or Percent     |
| value     | string | 收费金额，百分比写具体比率数值 |
| min_value | string | 最小收费限额                   |
| max_value | string | 最大收费限额                   |



### Enum解释

- 货币类型 type

| Bit位 | 描述     |
| ----- | -------- |
| 1     | 法币     |
| 2     | 数字货币 |



## Cabital Connect 标准跳转入口

```shell
curl "https://api.cabital.com/api/v1/partner/redirect"
```

### HTTP 请求

`GET /api/v1/partner/redirect?partner_id=<partner_name>&user_ext_ref=<user_ext_ref>&feature=xxx`



### URL参数

| 参数         | 是否必须 | 描述                                                         |
| ------------ | -------- | ------------------------------------------------------------ |
| partner_id   | false    | Cabital赋予Partner唯一的标识名称，通常为Partner的品牌        |
| user_ext_ref | false    | Partner在开始集成的的时候唯一的user_ext_ref，在Partner侧具有唯一性，有助于埋点 |
| feature      | false    | deeplink跳转，非必需，可以直接跳到2FA，KYC，或者转账等页面   |
| redirect_url | false    | Cabital在注册成功后浏览器redirect返回路径，需要encoding      |


#### 转换：feature=cfx

| 参数         | 是否必须 | 描述         |
| ------------ | -------- | ------------ |
| quote        | true     | 使用的报价对 |
| major_ccy    | true     | 主要币种     |
| major_amount | true     | 该币种数额   |


#### 转出：feature=payout

| 参数         | 是否必须 | 描述                                                         |
| ------------ | -------- | ------------------------------------------------------------ |
| major_ccy    | true     | 主要转出币种                                                 |
| major_amount | true     | 币种数额                                                     |
| method       | false    | 支付方式                                                     |
| wallet       | false    | 当数字货币时，尽量提供，格式为    chain:address:memo 其中memo为可选 |

#### 转入：feature=payin

| 参数         | 是否必须 | 描述         |
| ------------ | -------- | ------------------------------------------------------------ |
| major_ccy    | true     | 主要转出币种 |
| major_amount | true     | 币种数额     |
| method       | false    | 支付方式，比如SEPA，FPS，ERC20 |

#### KYC: feature=kyc

N/A

#### 2FA: feature=2FA

N/A

> 获得以下Response


```json
Resposne Code : 303
Url : 我方提供的页面
```
