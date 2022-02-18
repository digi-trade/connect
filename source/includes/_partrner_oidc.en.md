

# Prepare for Integration

The following section will introduce how partner could integrate seamlessly with cabital through OpenID connect (OIDC).



### Why does connect use OIDC?

The Cabital Connect product will connect users between two platforms, the user should have exactly one to one binding relationship. OIDC could ensure the user could have seamless experiences to finish KYC match and setup account in both platform vice versa.

Also, Connect will build both API and Widget (In the roadmap), OIDC could provide very flexible solutions to different partner to use different type of integration.

## 1. Partner APIs

**Partner should provide：**

| Name                  | Value Example | Comments |
| ------------------------ | ----------------------------- | ---- |
| Authorized origins       | https://partner.com                |      |
| Authorization URL | https://partner.com/auth | Partner Callback URL |
| Token URL | https://partner.com/token | Auth Request Interface |
| User Info URL | https://partner.com/account/info | Optional, in case the token didn't include email |
| Client ID | 077795d9-e7ec-4b07-8011-9909418c3ad8 | The client or client identifier registered within the identity provider. |
| Client Secret | secret_terces | The client or client secret registered within the identity provider. |
| Client Authentication | Basic Authentication | By default it should be basic authentication, we do support other methods, reach out to your consultant. |



**Cabital will provide：**

| Name             | Value                          | Comments           |
| -------------------- | ------------------------------------ | --------------------- |
| Client ID            | `Client_ID`                          | OAuth 2 Client ID     |
| Client Secret        | `Client_SECRET`                      | OAuth 2 Client Secret |
| Partner Base URL | [https://domain/{partner-id}](https://domain/{partner-id}) | Domain URL            |
| OIDC Configuration | [https://domain/{partner-id}/oidc-configuration](https://domain/{partner-id}/oidc-configuration) | openid-configuration configuration |




## 2. OIDC API Resources

### 2.1 Auth request Interface

Login or Connect through partner's OIDC.

**Request URL**

```shell
https://{Partner Authorization URL}
```

**Primary Parameters：**

| Parameter     | Description                                                  |
| :------------ | :----------------------------------------------------------- |
| response_type | REQUIRED.  Value is  ‘code’                                  |
| client_id     | REQUIRED. OAuth2 Client identifier created by Cabital        |
| redirect_url  | REQUIRED. Redirection URI to which the response will be sent. This URI MUST exactly match one of the Redirection URI values for the Client pre-registered at Cabital |
| scope         | REQUIRED. See the scope options in the table below           |
| state         | OPTIONAL. Opaque value used to maintain state between the request and the callback. Returned as a URL parameter on the authentication response. |

**Supported scopes：**

| **Scope** | Description                                                  |
| :-------- | :----------------------------------------------------------- |
| basic     | REQUIRED. Makes the Openid Connect authentication flow possible and includes the `id_token` in the response |
| email     | OPTIONAL. To include the users email address in the `id_token` |

**HTTP Method**

*  GET:  Parameters are URL encoded in the query string.

**Error Code:**

| HTTP Satus Code | Check                                                        | Reponse        Code | Reponse Message           |
| --------------- | :----------------------------------------------------------- | ------------------- | :------------------------ |
| 400             | The `redirect_uri` must be known to Cabital for the Service. | 001201              | invalid_request           |
| 400             | The response_type parameter must be “code”.                  | 001202              | unsupported_response_type |
| 400             | The `client_id` must refer to a Service in Cabital for which OpenID has been configured. | 001203              | invalid_client_id         |
| 400             | The scope must contain “openid”                              | 001204              | invalid_scope             |

**Example：**

```shell
https://{Partner Authorization URL}?client_id=demo-client&redirect_uri=http%3A%2F%2Flocalhost%3A3000%2Fcallback&response_type=code&scope=basic+email&state=pnIj1g3GMsX0Rj6FDbVoe3rYbLJzdfejT0EfusiEbis%3D
```

**Callback Redirect should include following Parameters：**

| **Parameter name** | **Explanation**                                              |
| :----------------- | :----------------------------------------------------------- |
| code               | A code generated for the authentication request by Cabital. The Relying party must treat this code as an opaque value |
| state              | The value of the state parameter                             |

```shell
https://{Partner Callback URL}/callback?state=57a30714-91e5-4fd7-a917-a4c5c8563e24&code=642de989-2564-405a-a807-5a8d20952e09.57a30714-91e5-4fd7-a917-a4c5c8563e24.d3e0888f-61b1-4d42-b971-156397ba24b5
```

### 2.2 Access token

Partner send authentication code to cabital to exchange auth token.

**Request URL**

```shell
https://{Token URL}
```

**Basic authentication**

The Relying party must authenticate itself using Basic authentication.  Reference: OAuth2 appendix A.2(https://tools.ietf.org/html/rfc6749#appendix-A.2).

**HTTP Method**

*  POST: the parameters are sent in the body of a request with `Content-Type: application/x-www-form-urlencoded`. 

**Primary Parameters：**

| **Parameter name** | **Explanation**                                              |
| :----------------- | :----------------------------------------------------------- |
| grant_type         | The expected grant type. Must be the fixed value: authorization_code |
| code               | The code as received from Cabital in the Authentication response. |
| redirect_uri       | URI to send the authorization response back to. Must be the one that is registered in Cabital with the Relying Party |

**Error Code:** 

| HTTP Satus Code | Check                                                        | Reponse        Code | Reponse Message        |
| --------------- | :----------------------------------------------------------- | ------------------- | :--------------------- |
| 400             | The basic authentication must be valid                       | 001201              | unauthorized_client    |
| 400             | The response_type parameter must be “authorization_code”.    | 001201              | unsupported_grant_type |
| 400             | The code must be known to Cabital and must have been generated for the Service identified by the basic authentication username. | 001201              | invalid_request        |
| 400             | The redirect_uri must be known to Cabital for the Service.   | 001201              | invalid_request        |


If the request was processed successfully, it will return a JSON object.

**Json Object Fields：**

| **Attribute name** | **Explanation**                                              |
| :----------------- | :----------------------------------------------------------- |
| access_token       | Token needed for sending a UserInfo request. The token is generated by Cabital. |
| token_type         | The authorization type the token has been generated for, must be the fixed value: Bearer |
| expires_in         | Validity period of the `access_token`, in seconds            |
| id_token           | JSON Web Token with information about the authenticated end user and the request. |
| refresh_token      | Refresh token.                                               |

id_token includes header, signature and payload which includes following important field:

| **Attribute name** | **Explanation**                               |
| :----------------- | :-------------------------------------------- |
| account_id         | The user identifier within Cabital,  user ID. |
| mail               | Email address of the user.                    |
| kyc_token          | KYC token.                                    |

Example：

```shell
{
    "account_id":"2cdcae60-a52c-40cd-9489-0c7b4771cc1a",
    "kyc_token":"xxxxxxx",
    "email":"karay.hua@cabital.com"
}
```



### 2.3 UserInfo (Optional)

Partner token api return the unique identification of user by `sub` field. Otherwise, Cabital will use the token to fetch userinfo, which will provide user basic information.

**HTTP Method**

*  GET: in that case, the parameters are URL encoded in the query string.

**Request URL**

```shell
https://{User Info URL}
```

**Parameters：**

The request will come with an `Authorization` header with the value `Bearer` and `access_token` is taken from the Access Token response.



**Common Error**

| HTTP Satus Code | Check                                                | Reponse        Code | Reponse Message |
| --------------- | :--------------------------------------------------- | ------------------- | :-------------- |
| 403             | The authentication must be valid, unauthorized token |                     |                 |

**JSON fields**

| **Attribute name** | **Explanation**                               |
| :----------------- | :-------------------------------------------- |
| account_id         | The user identifier within Cabital,  user ID. |
| mail               | Email address of the user.                    |
| kyc_token          | KYC token.                                    |



### 2.4 Refresh token (Optional)

Cabital will refresh token in case token expired.

**HTTP Method**

*  POST: the parameters are sent in the body of a request with `Content-Type: application/x-www-form-urlencoded`.

**Request URL**

```shell
https://{Token URL}
```

**Parameters：**

| **Parameter** | **Description**                                              |
| :------------ | :----------------------------------------------------------- |
| grant_type    | The expected grant type. Must be the fixed value: `refresh_token` |
| refresh_token | The refresh token as received from Cabital.                  |

Response please same as **Access token** interface.

**Token Life Suggestion：**

|         | **Access Token**               | **Refresh Token**                         |
| :------ | :----------------------------- | :---------------------------------------- |
| Expires | Default 3600 seconds (1 hour). | Default 86400 seconds (24 hours / 1 day). |

If user changed password or status abnormal，refresh token will be revoked immediately, have to authenticate again.



## 3. Resources

* [OpenID Connect](https://openid.net/connect/)

- [OpenID Connect Core 1.0 incorporating errata set 1](https://openid.net/specs/openid-connect-core-1_0.html)
- [Proof Key for Code Exchange by OAuth Public Clients](https://tools.ietf.org/html/rfc7636)
- [OAuth 2.0 Token Exchange](https://tools.ietf.org/html/draft-ietf-oauth-token-exchange-19)
- [OAuth 2.0 Mutual-TLS Client Authentication and Certificate-Bound Access Tokens](https://tools.ietf.org/html/draft-ietf-oauth-mtls-17)
- [JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants](https://tools.ietf.org/html/rfc7523)

