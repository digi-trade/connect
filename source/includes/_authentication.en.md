# Authentication

API Key / Secret is the credential to access Connect API. All the HTTPS calls need to be signed and include following HTTP Headers:

- ACCESS-KEY: Customer Access Key, provided by Cabital
- ACCESS-SIGN: The signature build by the algorithm described below. 
- ACCESS-TIMESTAMP: API Access time stamp
- ACCESS-NONCE: We use nonces to ensure that old communications cannot be reused. Each API call must used different nounce, otherwise, system will ignore the 2nd request arrived within 60 mins time frame.

API Request must include `Content-Type` as `application/json` and the body must be in restricted JSON format.




ACCESS-SIGN Header is the sha256 HMAC hash by `time stamp + HTTP method + NONCE + access path + request body` with Secret Key.

- The timestamp should be the same as ACCESS-TIMESTAMP. 
- The request body is in the form of String, or regarded as empty, especially when accessing the API through GET
- HTTP method must be uppercase, i.e. `GET` or `POST`

ACCESS-TIMESTAMP is from the Unix Epoch to the current number in seconds. The timestamp of the customer request must be within 30 seconds of the API service time, otherwise it will be considered expired or rejected directly.

<aside class="warning">
Please do not share your Secret. Otherwise, it may cause information leakage and an unnecessary loss of transaction fees. If your Secret is leaked, please contact your service consultant immediately.
</aside>