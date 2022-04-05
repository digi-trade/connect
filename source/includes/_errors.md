# 错误处理 Code

系统通常错误处理

错误代码 | 含义
---------- | -------
400 | Bad Request -- 访问参数有错误
401 | Unauthorized -- API Secret 错误，或者 Request 签名错误。
403 | Forbidden -- 通常为客户已经取消与合作方的账户关联，需要合作方发起重新关联。（Unlink状态）
404 | Not Found -- 特定的资源不存在
405 | Method Not Allowed -- 访问的方法不正确
406 | Not Acceptable -- 格式强制要求是JSON，不允许
409 | Conflict -- 消息违反幂等性（NONCE）
429 | Too Many Requests -- 访问频率过高
500 | Internal Server Error -- 本方出现服务器错误，请重试.
503 | Service Unavailable -- 本方服务不可用，请稍后重试.
