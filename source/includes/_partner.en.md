# Partner Level Setting

The partner system API is provided to the partner to perceive the status of cooperative account opening, system basic settings, and non-user level settings.

## Partner configuration


```shell
curl "http://partner.cabital.com/partners/partner_id/config"
```

### HTTP Request

`GET /api/v1/config`


> Response Sample:


```json
{
    "currencies" : [
        {
            "symobl": "ETH",
            "type": 2,
            "deposit_methods": [
                "ERC20", "SOL"
            ],
            "withdrew_methods": [
                "ERC20", "SOL"
            ],
            "config": {
                "debit": {
                    "min": "0.01",
                    "max": "5"
                },
                "credit": {
                    "allow": true,
                    "min": "0.01",
                    "max": "5"
                },
                "conversion": {
                    "allow": true,
                    "min": "0.01",
                    "max": "5"
                }
            }
        },
        {
            "symobl": "EUR",
            "type": 1, 
            "deposit_methods": [
                "SEPA"
            ],
            "withdrew_methods": [
                "SEPA"
            ],
            ,
            "config": {
                "debit": {
                    "allow": false,
                },
                "credit": {
                    "allow": false,
                },
                "conversion": {
                    "allow": true,
                    "min": "0.01",
                    "max": "5"
                }
            }
        }
    ]
}
```
### Request Object

symobl | string(ENUM) | currency symbol
type | int(ENUM) | currency type, see the explanation below for details
deposit_methods | list | Current deposit methods
withdraw_methods | list | Current withdrawal method
config | list(object) | restrictions and configuration of the currency


Config object definition

Field | Type | Description
--------- | ------- | ---------------
allow | bool | availability of the currency (killer switch)
min | string(number) | minimum per transaction
max | string(number) | Maximum per transaction

### Enum

- Currency type

Value | Description
--------- | -----------
1 | FIAT
2 | Crypto Currency

<aside class="success">
The limit is for the entire partner, limit of a single transaction
</aside>

## Get the latest quotation (GET)

```shell
curl "http://partner.cabital.com/partners/partner_id/quotes/USDT-EUR"
```
### HTTP Request

`GET /api/v1/quotes/<code_pair>`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
code_pair | true | The CODE Pair of the quote uses `-` as the separator. The Cabital system will ignore the incoming order and sort according to its own rules. Therefore, a currency pair will always return one regardless of the incoming order and the buying and selling relationship. A quotation, such as USDT-EUR.

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
ask | string(number) | Sell CODE1 in exchange for CODE2 offer
valid_until | number(timestamp) | The number in seconds from the Unix Epoch to the current, used to indicate the effective time of the quote
quote_id | string | The unique ID of the quotation, the format is event stamp: quotation currency pair: quotation purpose
quote | string | The quoted Pair, the left is Base, the right is Quote, and the quote is centered on Cabital
valid_interval | number | The effective time width of the quotation, in seconds
bid | string(number) | Sell CODE2 in exchange for CODE1's quotation

<aside class="success">
The symbol of FIAT is based on <a href="https://en.wikipedia.org/wiki/ISO_4217">ISO-4217</a>, and the crypto currency is based on <a href="https://coinmarketcap.com/ all/views/all/">CoinMarketCap</a> list
</aside>