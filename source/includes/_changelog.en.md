# Version changes

## 0.2.5

- Remove IBAN from `GBP` deposit information

## 0.2.4

- Update error code and error information

## 0.2.3

- Modified partner configuration to pass the transaction fee.
- Append currency table and decimal precision standard.
- Add country code in `matching`
- Add `GBP` deposit information
- Modified `transfers`
- Modified `quote`
- Webhook event: Removed `MATCHING` status

## 0.2.2

- Modified the conversions transaction configuration information for the configuration interface of the get partner
- Added the transaction cost configuration information of the interface that obtains the configuration of the partner

## 0.2.1

- Modified the conversions transaction configuration information of the Partner-Configuration interface
- Fixed URL and API inconsistencies problem

## 0.2.0

- Standardized the product name, changed from `partner` to `cabital connect`
- Added partner preparation documents to prepare for the partner's access to its own system
- Minor adjustments in the naming of the Connect status, which refines the two different properties of `Rejected`
- Added error handling of the account, adjusted the status, and converted it to config

## 0.1.1

- API-Key has been restored to speed up the integration of Connect for partners who have integrated CaaS
- Due to the use of API-Key, remove the Partner Id in the access Path
- Add more API details and error handling

## 0.1.0

- Provide the original Partner API documentation