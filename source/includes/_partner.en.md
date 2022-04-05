# Partner Level API

The partner system API is provided to the partner to perceive the status of cooperative account opening, system basic settings, and non-user level settings.

## Partner Configuration


```shell
curl "https://partner.cabital.com/api/v1/config"
```

### HTTP Request

`GET /api/v1/config`


> Response Sample:


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

### Basic Elements

- currencies : all currencies available for the connect partners with configuration
- pairs: exchange pairs for quotes.

### Currency Object Definition

* Each Currency object elements

Field | Type | Description
--------- | ------- | ---------------
symobl | string(ENUM) | currency symbol
type | int(ENUM) | currency type, see the explanation below for details
deposit_methods | list | Current deposit methods
withdraw_methods | list | Current withdrawal method
config | list(object) | restrictions and configuration of the currency
conversions | list(object) | restrictions and configuration of currency pair
Fees | list(object) | Fee configuration 


* credit/debit object definition

| Field | Type | Description     |
| :---- | -------------- | ---------------------------- |
| allow | bool           | the pair does allow           |
| min   | string(number) | minimum amount per transaction，`-1` means unlimited |
| max   | string(number) | maximum amount per transaction，`-1` means unlimited |

* conversion object definition

Field | Type | Description
--------- | ------- | ---------------
allow | bool | availability of the currency (killer switch)
min | string(number) | minimum amount per transaction
max | string(number) | maximum amount per transaction


* debit_fee object definition

|Field | Type | Description|
| :----------------- | ------ | ------------------------------ |
| object             | sting  | Charge Subject: `Customer` / `Partner`  |
| is_single          | bool | Charge Modal `Single` or `Tier`         |
| method             | string | Calculation method：Fixed or Percent     |
| value              | string | Fixed : fee amount / Percent : percentage |
| original_value     | string | (For Display) Fixed : fee amount / Percent : percentage |
| min_value          | string | min fee amount                   |
| original_min_value | string | min fee amount for display in case the method is `Percent`         |
| max_value          | string | max fee amount                   |
| original_max_value | string |  max fee amount for display in case the method is `Percent`     |

### Enum

- Currency type

Value | Description
--------- | -----------
1 | FIAT
2 | Crypto Currency

<aside class="success">
The limit is for the entire partner, limit of a single transaction
</aside>

## Get the Latest Quotation (GET)

```shell
curl "http://partner.cabital.com/api/v1/quotes/USDT-EUR"
```
### HTTP Request

`GET /api/v1/quotes/<code_pair>`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
code_pair | true | The code_pair parameter uses `-` as the separator. The Cabital system will ignore the incoming order and sort according to its own rules. Therefore, a currency pair will always return the quotes in the same code pair order regardless of the incoming order and the buying and selling relationship, i.e. fetch `USDT-EUR` or `EUR-USDT` will return quotes in `USDT-EUR` pair. 

> Response Sample


```json
{
    "quote": "37112.64",
    "valid_until": 1645113784,
    "quote_id": "20220217160254:BTC-EUR:Customer",
    "valid_interval": 10,
    "reversed_quote": "0.00002724"
}
```
### Response Object

Field | Type | Description
--------- | ------- | ---------------
quote | string(number) | Sell *Left Code* in exchange for *Right Code* price
valid_until | number(timestamp) | The number in seconds from the Unix Epoch to the current, used to indicate the effective time of the quote
quote_id | string | The unique ID of the quotation, the format is [timestamp:quotation currency pair: quotation purpose]
valid_interval | number | The effective time duration of the quotation, in seconds
reversed_quote | string(number) | Sell *Right Code* in exchange for *Left Code* quotation

<aside class="success">
The symbol of FIAT is based on <a href="https://en.wikipedia.org/wiki/ISO_4217">ISO-4217</a>, and the crypto currency is based on <a href="https://coinmarketcap.com/ all/views/all/">CoinMarketCap</a> list
</aside>





## Reconciliation 

Connect provides reconciliation interface to help partner to query transfer by external id.

```shell
curl "/api/v1/recon/transfers/adb8f31d-7a71-4003-85d7-3ac58158461f"
```

> Response Sample:

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

### HTTP Request

`GET /api/v1/recon/transfers/<external_id>`

### URL Parameters

| Parameter   | Required | Description         |
| ----------- | -------- | ------------------- |
| external_id | true     | Partner External ID |

### Response Object

| Field             | Type              | Description                                                  |
| ----------------- | ----------------- | ------------------------------------------------------------ |
| transfer_id       | string(uuid)      | Transfer order id                                            |
| instructed_amount | string(number)    | Instructed Amount                                            |
| customer_fee      | string(number)    | Fee charged to customer                                      |
| actual_amount     | string(number)    | Customer actually received                                   |
| symbol            | string            | Currency symbol                                              |
| direction         | string(enum)      | The direction of the transfer to Cabital. `CREDIT` for top up into the Cabital Account. `DEBIT` for withdraw out of the Cabital Account. |
| conversion_id     | string(uuid)      | The conversion order ID in C+T related transactions, optional |
| external_id       | string(50)        | The third-party ID of the partner, optional, but have to be unique to individual transfer. |
| status            | string(enum)      | The result of the transfer (`SUCCESS` / `FAILED` / `PROCESSING`) |
| created_at        | timestamp(number) | Created timestamp                                            |