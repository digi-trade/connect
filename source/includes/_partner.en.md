# Partner Level Setting

The partner system API is provided to the partner to perceive the status of cooperative account opening, system basic settings, and non-user level settings.

## Partner Configuration


```shell
curl "http://partner.cabital.com/api/v1/config"
```

### HTTP Request

`GET /api/v1/config`


> Response Sample:


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
### Request Object

Field | Type | Description
--------- | ------- | ---------------
symobl | string(ENUM) | currency symbol
type | int(ENUM) | currency type, see the explanation below for details
deposit_methods | list | Current deposit methods
withdraw_methods | list | Current withdrawal method
config | list(object) | restrictions and configuration of the currency
conversions | list(object) | restrictions and configuration of currency pair



Config object definition

Field | Type | Description
--------- | ------- | ---------------
allow | bool | availability of the currency (killer switch)
min | string(number) | minimum amount per transaction
max | string(number) | maximum amount per transaction

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
code_pair | true | The code_pair parameter uses `-` as the separator. The Cabital system will ignore the incoming order and sort according to its own rules. Therefore, a currency pair will always return the quotes in the same code pair order regardless of the incoming order and the buying and selling relationship, i.e. fetch USDT-EUR or EUR-USDT will return quotes in USDT-EUR pair.

> Response Sample


```json
{
    "ask": "0.864962",
    "valid_until": 1633426832,
    "quote_id": "20211005094027:USDT-EUR:Customer",
    "valid_interval": 5,
    "bid": "1.16424"
}
```
### Response Object

Field | Type | Description
--------- | ------- | ---------------
ask | string(number) | Sell *Left Code* in exchange for *Right Code* price
valid_until | number(timestamp) | The number in seconds from the Unix Epoch to the current, used to indicate the effective time of the quote
quote_id | string | The unique ID of the quotation, the format is [timestamp:quotation currency pair: quotation purpose]
valid_interval | number | The effective time duration of the quotation, in seconds
bid | string(number) | Sell *Right Code* in exchange for *Left Code* quotation

<aside class="success">
The symbol of FIAT is based on <a href="https://en.wikipedia.org/wiki/ISO_4217">ISO-4217</a>, and the crypto currency is based on <a href="https://coinmarketcap.com/ all/views/all/">CoinMarketCap</a> list
</aside>