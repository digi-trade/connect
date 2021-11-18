# Account Connect related

Cabital Connect is subject to regulatory requirements. The access partner must provide the identity information (each field) consistent with this party. For field descriptions, please refer to the API for linking accounts with the same name.

## Get User Connection Status


```shell
curl "http://partner.cabital.com/api/v1/accounts/cdaa9983-9b8f-4478-ba60-896ac239879d/detail"
```

### HTTP Request

`GET /api/v1/accounts/<account_id>/detail`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
account_id | true | Cabital Connect Account Id

> Response Sample:


```json
{
  "match_status" : "MATCHED",
  "otp_ready" : true,
  "email_address" : "john.doe@email.com",
  "kyc_token" : "8yzhvhv87dahfb",
  "limits": {}
}
```


Field | Type | Description
--------- | ------- | ---------------
account_status | string(ENUM) | Status, see the definition in Events
otp_ready | bool | Whether the customer has completed the OTP binding in Cabital, it needs to be attached when withdrawing.
email_address | string | The user’s Email for the partner to match
kyc_token | string | The user's KYC Token for the partner to apply
limits | object | Current payment limit

<aside class="success">
Single account limit
</aside>

## Name Match

```shell
curl "/api/v1/accounts/cdaa9983-9b8f-4478-ba60-896ac239879d/match" \
  -X "PUT" \
  -d '{ "name": "John Doe", "id": "J12345678D", "id_document": "PASSPORT", "dob": "19700101" }' 
```



### HTTP Request

`PUT /api/v1/accounts/<account_id>/match`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
account_id | true | Cabital Connect Account Id

### Request Body

Field | Type | Description
--------- | ------- | ---------------
name | string | The full name of the customer, in accordance with the usual order on the document, such as First Name + (Middle Name) + Last Name
id | string |ID on the identity document
id_document | string(ENUM) | Identity document type (`ID,PASSPORT,DRIVER_LICENSE`)
issued_by | string | The country where the identity document was issued
dob | string | Date of birth format in `YYYYMMDD`


> Sample Response:

```json
{
  "result": "MISMATCH",
  "mismatch_fields": [
    "name",
    "dob"
  ]
}
```

Submit user information to get the verification result of the same name control.

### Error Message


- HTTP 400 : Connection Status must be in CREATED or MISMATCHED