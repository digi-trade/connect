# API 认证

API Key是客户访问FaaS认证的唯一方法，所有的API都需要签名并包含以下HTTP Header:

- ACCESS-KEY 客户的Access Key，请联系服务顾问获取
- ACCESS-SIGN 在客户系统中根据下面的规则生成的消息签名
- ACCESS-TIMESTAMP API访问的时间戳
- ACCESS-NONCE 请求发送的唯一的数字或者字符串，每个API请求必须选择不同的Nonce，接收系统会以NONCE作为调用消息的幂等处理

所有的访问必须设置 `Content-Type` 为 `application/json`，并且内容为正规的的JSON。

ACCESS-SIGN Header是通过sha256 HMAC的算法以Secret Key计算`时间戳 + HTTP方法 + NONCE + 访问路径 + 请求体`。

- 时间戳应该与ACCESS-TIMESTAMP相同
- 请求体为String形式，或者视为空尤其在通过GET方式访问API时
- HTTP方法必须为大写英文

ACCESS-TIMESTAMP 以秒为单位从Unix Epoch到当前的数字，客户请求的时间戳必须与API服务的时间差别30秒以内，否则将被认为是过期或者直接拒绝。

<aside class="warning">请不要分享你的API Key与Secrets，否则会造成信息泄漏以及不必要的交易费损失。如Secret出现异常，请立刻与你的服务顾问联系并立刻更新。</aside>