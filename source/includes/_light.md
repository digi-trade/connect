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
      "deposit": {
        "methods": [
          "ERC20"
        ]
      },
      "withdraw": {
        "methods": [
          "ERC20"
        ],
        "limit": {
          "min": "0.02",
          "max": "-1"
        },
        "fee": {
          "is_single": true,
          "object": "customer",
          "method": "fixed",
          "value": "0.004"
        }
      },
      "conversion": {
        "limit": {
          "min": "0.02",
          "max": "100"
        }
      }
    },
    {
      "symbol": "BTC",
      "type": 2,
      "deposit": {
        "methods": [
          "BTC"
        ]
      },
      "withdraw": {
        "methods": [
          "BTC"
        ],
        "limit": {
          "min": "0.001",
          "max": "-1"
        },
        "fee": {
          "is_single": true,
          "object": "Customer",
          "method": "Fixed",
          "value": "0.0006"
        }
      },
      "conversion": {
        "limit": {
          "min": "0.0002",
          "max": "5"
        }
      }
    },
    {
      "symbol": "USDT",
      "type": 2,
      "deposit": {
        "methods": [
          "ERC20"
        ]
      },
      "withdraw": {
        "methods": [
          "ERC20"
        ],
        "limit": {
          "min": "40",
          "max": "-1"
        },
        "fee": {
          "is_single": true,
          "object": "Customer",
          "method": "Fixed",
          "value": "12"
        }
      },
      "conversion": {
        "limit": {
          "min": "10",
          "max": "200000"
        }
      }
    },
    {
      "symbol": "EUR",
      "type": 1,
      "deposit": {
        "methods": [
          "SEPA"
        ]
      },
      "withdraw": {
        "methods": [
          "SEPA"
        ],
        "limit": {
          "min": "25",
          "max": "50000"
        },
        "fee": {
          "is_single": true,
          "object": "Customer",
          "method": "Fixed",
          "value": "2.5"
        }
      },
      "conversion": {
        "limit": {
          "min": "10",
          "max": "200000"
        }
      }
    },
    {
      "symbol": "GBP",
      "type": 1,
      "deposit": {
        "methods": [
          "FPS"
        ]
      },
      "withdraw": {
        "methods": [
          "FPS"
        ],
        "limit": {
          "min": "20",
          "max": "40000"
        },
        "fee": {
          "is_single": true,
          "object": "Customer",
          "method": "Fixed",
          "value": "2.5"
        }
      },
      "conversion": {
        "limit": {
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
| deposit         | object         | 当前入金配置           |
| withdraw        | object         | 当前出金配置            |
| conversion       | object        | 当前换汇配置           |

Config 对象定义

* withdraw 对象定义

| 字段 | 类型           | 描述                         |
| :--- | -------------- | ---------------------------- |
| methods  | list(string) | 当前出金方式 |
| limit  | object(limit) | 交易金额限制 |
| fee  | object(fee) | 费用金额限制 |

* deposit 对象定义

| 字段 | 类型           | 描述                         |
| :--- | -------------- | ---------------------------- |
| methods  | list(string) | 当前入金方式 |
| limit  | object(limit) | 交易金额限制, 如果没有则不限制 |
| fee  | object(fee) | 费用金额限制 |

* conversion 对象定义

| 字段 | 类型           | 描述                         |
| :--- | -------------- | ---------------------------- |
| limit  | object(limit) | 换汇交易限制 |

* limit对象定义

| 字段 | 类型           | 描述             |
| :--- | -------------- | ---------------- |
| min  | string(number) | 最小每笔，`-1`表示不限制 |
| max  | string(number) | 最大每笔，`-1`表示不限制 |

fee 内对象定义

* fee 对象定义

| 字段      | 类型   | 描述                           |
| :-------- | ------ | ------------------------------ |
| object    | sting  | 收费对象，Customer or Partner  |
| is_single | bool   | 收费类型Single or Tier         |
| method    | string | 收费方式：Fixed or Percent     |
| value     | string | 收费金额，百分比写具体比率数值 |



### Enum解释

- 货币类型 type

| Bit位 | 描述     |
| ----- | -------- |
| 1     | 法币     |
| 2     | 数字货币 |



## Cabital Connect 标准跳转入口

```shell
curl "https://api.cabital.com/api/v1/light/partner/redirect"
```

### HTTP 请求

`GET /api/v1/light/partner/redirect?partner_id=<partner_name>&user_ext_ref=<user_ext_ref>&feature=xxx`


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
| pair         | true     | 使用的报价对 |
| major_ccy    | true     | 主要币种     |
| major_amount | true     | 该币种数额   |
| type         | true     | 交易方向，buy or sell  |


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
