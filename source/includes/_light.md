# 轻量化集成

部分合作方希望以最快的方法集成Cabital Connect，在一开始可以采用轻量化的方式进行集成。

所有的API都归属在`api.cabital.com`下，并无特殊验证，可自行进行集成。



## 合作方准备

- 请向Cabital [BD](mailto:bd@cabital.com) 申请 NDA，在电子签署及确定后，向 Cabital 索取 CI Media的资源包
- 向 Cabital BD 提供合作方的Logo，要求包含亮、暗两类，SVG和PNG各一个，PNG要求为512 X 512 Pixel
- Cabital 提供沙箱环境，请提供测试账号的email地址，Cabital会在系统配置完备之后，给对应email创建账号，并通过模拟KYC以供测试。该账户将获得1000元USDT测试金，该账户禁止使用提款功能
- 如需联系技术支持，请发给[技术服务](mailto:developer@cabital.com) / [客户服务](support@cabital.com)



## 获取Light配置

由于轻量级集成，所有的交互由Cabital提供，所有的配置和设定均和Cabital系统设置一致。


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
| limit  | object(limit) | 交易金额限制 |
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



## Connect Light 跳转入口

合作方可以通过浏览器或者嵌入式浏览器访问跳转接口，并提供特殊参数以获得专属的落地页支持，并通过传入部分可选参数，以得到登陆后的跳转支持

```shell
curl "https://api.cabital.com/api/v1/light/partner/redirect"
```

### HTTP 请求

`GET /api/v1/light/partner/redirect?partner_id=<partner_name>&user_ext_ref=<user_ext_ref>&feature=xxx`



### URL参数

| 参数         | 是否必须 | 描述                                                         |
| ------------ | -------- | ------------------------------------------------------------ |
| partner_id   | true     | Cabital赋予Partner唯一的标识名称，通常为Partner的品牌        |
| user_ext_ref | false    | Partner在开始集成的的时候唯一的user_ext_ref，在Partner侧具有唯一性，有助于埋点 |
| feature      | false    | deeplink跳转，非必需，可以直接跳到换汇页面，默认为账户列表页 |


#### 转换：feature=cfx

| 参数         | 是否必须 | 描述         |
| ------------ | -------- | ------------ |
| quote        | false | 使用的报价对 |
| major_ccy    | false | 主要币种     |
| major_amount | false | 该币种数额   |
| type         | true     | 交易方向，buy or sell  |

<!--
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

-->

> 获得以下Response


```json
Resposne Code : 303
Url : 我方提供的页面
```





## 获取Light的报价 (GET)

通过上面提到的Light配置中所定义的报价币对列表，按某一币对获取报价信息。

```shell
curl "http://api.cabital.com/api/v1/light/quotes/USDT-EUR"
```

### HTTP 请求

`GET /api/v1/light/quotes/<code_pair>`

### URL参数

| 参数      | 是否必须 | 描述                                                         |
| --------- | -------- | ------------------------------------------------------------ |
| code_pair | true     | 报价的CODE Pair，以`-`作为分割符，Cabital 系统会忽视传入的次序，并以自身规则进行排序，所以一个货币对，不论传入的次序和买卖关系，始终返回一种报价，比如USDT-EUR。 |

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

| 字段           | 类型              | 描述                                                         |
| -------------- | ----------------- | ------------------------------------------------------------ |
| valid_until    | number(timestamp) | 以秒为单位从Unix Epoch到当前的数字，用来表示该报价的有效时间 |
| quote_id       | string            | 报价的唯一ID，格式为 事件戳:报价货币对:报价目的              |
| quote          | string            | 对Code-Pair为`CODE1-CODE2,`卖 CODE1 换取 CODE2 的报价        |
| valid_interval | number            | 报价有效时间宽度，以秒记                                     |
| reversed_quote | string(number)    | 对Code-Pair为`CODE1-CODE2,`卖 CODE2 换取 CODE1 的报价        |


<aside class="success">
法币的符号以 <a href="https://en.wikipedia.org/wiki/ISO_4217">ISO-4217</a> 为标准，数字货币依照 <a href="https://coinmarketcap.com/all/views/all/">CoinMarketCap</a> 列表
</aside>


