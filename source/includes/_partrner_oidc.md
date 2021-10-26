

# 合作方接入准备

本文主要描述合作方Partner使用OpenID Connect (OIDC)协议接入Cabital.

## 1. 准备信息

**合作方Partner需提供给Cabital信息：**

| 名称                     | 配置值                        |      |
| ------------------------ | ----------------------------- | ---- |
| Authorized origins       | https://partner.com                |      |
| Authorized redirect URL | https://partner.com/callback | Partner Callback URL |



**Cabital提供给合作方Partner的资料信息：**

| 名称                 | 配置值                               | 备注                  |
| -------------------- | ------------------------------------ | --------------------- |
| Client ID            | `Client_ID`                          | OAuth 2 Client ID     |
| Client Secret        | `Client_SECRET`                      | OAuth 2 Client Secret |
| Partner Base URL | [https://domain/{partner-id}](https://domain/{partner-id}) | Domain URL            |
| OIDC Configuration | [https://domain/{partner-id}/oidc-configuration](https://domain/{partner-id}/oidc-configuration) | openid-configuration信息 |




## 2. API资源

### 2.1 Auth request接口

合作方Partner使用Cabital用户Login时重定向到Cabital调用此接口.

**重定向URL**

```shell
https://{Partner Base URL}/oidc/auth
```

**主要支持参数：**

| 参数          | **描述**                                                     |
| :------------ | :----------------------------------------------------------- |
| response_type | REQUIRED.  Value is  ‘code’                                  |
| client_id     | REQUIRED. OAuth2 Client identifier created by Cabital        |
| redirect_url  | REQUIRED. Redirection URI to which the response will be sent. This URI MUST exactly match one of the Redirection URI values for the Client pre-registered at Cabital |
| scope         | REQUIRED. See the scope options in the table below           |
| state         | OPTIONAL. Opaque value used to maintain state between the request and the callback. Returned as a URL parameter on the authentication response. |

**主要支持scopes：**

| **Scope** | **描述**                                                     |
| :-------- | :----------------------------------------------------------- |
| basic     | REQUIRED. Makes the Openid Connect authentication flow possible and includes the `id_token` in the response |
| email     | OPTIONAL. To include the users email address in the `id_token` |

**使用HTTP 方法**

*  GET:  Parameters are URL encoded in the query string.

**错误处理说明信息:**

| HTTP Satus Code | Check                                                        | Reponse        Code | Reponse Message           |
| --------------- | :----------------------------------------------------------- | ------------------- | :------------------------ |
| 400             | The `redirect_uri` must be known to Cabital for the Service. | 001201              | invalid_request           |
| 400             | The response_type parameter must be “code”.                  | 001202              | unsupported_response_type |
| 400             | The `client_id` must refer to a Service in Cabital for which OpenID has been configured. | 001203              | invalid_client_id         |
| 400             | The scope must contain “openid”                              | 001204              | invalid_scope             |

**例子：**

```shell
https://{Partner Base URL}/oidc/auth?client_id=demo-client&redirect_uri=http%3A%2F%2Flocalhost%3A3000%2Fcallback&response_type=code&scope=basic+email&state=pnIj1g3GMsX0Rj6FDbVoe3rYbLJzdfejT0EfusiEbis%3D
```

**Callback重定向将带有下列参数：**

| **Parameter name** | **Explanation**                                              |
| :----------------- | :----------------------------------------------------------- |
| code               | A code generated for the authentication request by Cabital. The Relying party must treat this code as an opaque value |
| state              | The value of the state parameter                             |

```shell
https://{Partner Callback URL}/callback?state=57a30714-91e5-4fd7-a917-a4c5c8563e24&code=642de989-2564-405a-a807-5a8d20952e09.57a30714-91e5-4fd7-a917-a4c5c8563e24.d3e0888f-61b1-4d42-b971-156397ba24b5
```

### 2.2 Access token

合作方Partner发送授权code到Cabital获取token信息.

**请求URL**

```shell
https://{Partner Base URL}/oidc/token
```

**使用Basic authentication**

The Relying party must authenticate itself using Basic authentication.  参考OAuth2 appendix A.2(https://tools.ietf.org/html/rfc6749#appendix-A.2).

**使用HTTP 方法**

*  POST: the parameters are sent in the body of a request with `Content-Type: application/x-www-form-urlencoded`.

**请求使用参数**

| **Parameter name** | **Explanation**                                              |
| :----------------- | :----------------------------------------------------------- |
| grant_type         | The expected grant type. Must be the fixed value: authorization_code |
| code               | The code as received from Cabital in the Authentication response. |
| redirect_uri       | URI to send the authorization response back to. Must be the one that is registered in Cabital with the Relying Party |

**处理错误信息说明:** 

如果处理失败，Cabital会发送错误响应信息。

| HTTP Satus Code | Check                                                        | Reponse        Code | Reponse Message        |
| --------------- | :----------------------------------------------------------- | ------------------- | :--------------------- |
| 400             | The basic authentication must be valid                       | 001201              | unauthorized_client    |
| 400             | The response_type parameter must be “authorization_code”.    | 001201              | unsupported_grant_type |
| 400             | The code must be known to Cabital and must have been generated for the Service identified by the basic authentication username. | 001201              | invalid_request        |
| 400             | The redirect_uri must be known to Cabital for the Service.   | 001201              | invalid_request        |


如果处理成功，则返回json报文，此时HTTP header设置”Content-Type: application/json“。

**json对象字段如下：**

| **Attribute name** | **Explanation**                                              |
| :----------------- | :----------------------------------------------------------- |
| access_token       | Token needed for sending a UserInfo request. The token is generated by Cabital. |
| token_type         | The authorization type the token has been generated for, must be the fixed value: Bearer |
| expires_in         | Validity period of the `access_token`, in seconds            |
| id_token           | JSON Web Token with information about the authenticated end user and the request. |
| refresh_token      | Refresh token.                                               |

id_token对象通常包含头，签名和内容体，其中payload作为json对象主要有如下信息：

| **Attribute name** | **Explanation**                               |
| :----------------- | :-------------------------------------------- |
| account_id         | The user identifier within Cabital,  user ID. |
| mail               | Email address of the user.                    |
| kyc_token          | KYC token.                                    |

payload内容如下所示：

```shell
{
    "account_id":"2cdcae60-a52c-40cd-9489-0c7b4771cc1a",
    "kyc_token":"xxxxxxx",
    "email":"karay.hua@cabital.com"
}
```



### 2.3 UserInfo

合作方Partner可以使用返回字段`sub`识别唯一账号，如果没有需要的账户信息，需要调用UserInfo请求。

**使用HTTP 方法**

*  GET: in that case, the parameters are URL encoded in the query string.

**使用URL**

```shell
https://{Partner Base URL}/oidc/userinfo
```

**重定向请求带有如下参数：**

HTTP header中设置: Authorization: Bearer The `access_token` is taken from the Access Token response.

如果处理失败，Cabital会发送错误响应信息。

**处理错误信息说明**

| HTTP Satus Code | Check                                                | Reponse        Code | Reponse Message |
| --------------- | :--------------------------------------------------- | ------------------- | :-------------- |
| 403             | The authentication must be valid, unauthorized token |                     |                 |


如果处理成功，则返回json报文，此时HTTP header设置”Content-Type: application/json“。

**json对象字段如下：**

| **Attribute name** | **Explanation**                               |
| :----------------- | :-------------------------------------------- |
| account_id         | The user identifier within Cabital,  user ID. |
| mail               | Email address of the user.                    |
| kyc_token          | KYC token.                                    |



### 2.4 Refresh token

刷新token接口

**使用HTTP 方法**

*  POST: the parameters are sent in the body of a request with `Content-Type: application/x-www-form-urlencoded`.

**使用URL**

```shell
https://Base-url/{partner-id}/oidc/token
```

**重定向请求带有如下参数**

HTTP header中设置: Authorization: Bearer The `access_token` is taken from the Access Token response.

| **Parameter** | **Description**                                              |
| :------------ | :----------------------------------------------------------- |
| grant_type    | The expected grant type. Must be the fixed value: `refresh_token` |
| refresh_token | The refresh token as received from Cabital.                  |

Response信息参考**Access token**接口

**Token生命周期有如下规则：**

|         | **Access Token**               | **Refresh Token**                         |
| :------ | :----------------------------- | :---------------------------------------- |
| Expires | Default 3600 seconds (1 hour). | Default 86400 seconds (24 hours / 1 day). |

当用户更改密码和用户不正常时候，refresh token会失效，需要用户再次验证身份。

## 3. Resources

* [OpenID Connect](https://openid.net/connect/)

- [OpenID Connect Core 1.0 incorporating errata set 1](https://openid.net/specs/openid-connect-core-1_0.html)
- [Proof Key for Code Exchange by OAuth Public Clients](https://tools.ietf.org/html/rfc7636)
- [OAuth 2.0 Token Exchange](https://tools.ietf.org/html/draft-ietf-oauth-token-exchange-19)
- [OAuth 2.0 Mutual-TLS Client Authentication and Certificate-Bound Access Tokens](https://tools.ietf.org/html/draft-ietf-oauth-mtls-17)
- [JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants](https://tools.ietf.org/html/rfc7523)

