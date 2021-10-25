# Error Handling Code

System error handling

Error code | Meaning
---------- | -------
400 | Bad Request - There is an error in parameters
401 | Unauthorized - API Secret error, or Request signature error.
403 | Forbidden - Usually the customer has canceled the association with the partner's account, and the partner needs to initiate a re-association. (Unlink status)
404 | Not Found - The specific resource does not exist
405 | Method Not Allowed - The method of access is incorrect
406 | Not Acceptable - The mandatory format is JSON, not allowed
409 | Conflict - The message violates idempotence (NONCE)
429 | Too Many Requests - Too many requests
500 | Internal Server Error - A server error occurred on this party, please try again.
503 | Service Unavailable - Our service is unavailable, please try again later.