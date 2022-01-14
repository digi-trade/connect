# 版本变化

## 0.2.5
- 更新了获取单币入账信息接口gbp币种时响应field去除iban

## 0.2.4

- 更新了错误码和错误信息

## 0.2.3

- 修改了获取合作方的配置接口的交易费用配置信息
- 增加了接口文档中金额格式规范
- 增加了match接口中国家码相关信息
- 增加账户单币入账信息支持GBP
- 修改了transfers相关接口
- 修改了quote接口字段
- Webhook event去除了MATCHING状态事件

## 0.2.2

- 修改了获取合作方的配置接口的conversions交易配置信息
- 增加了获取合作方的配置接口的交易费用配置信息

## 0.2.1

- 修改了获取合作方的配置接口的conversions交易配置信息
- 修改了URL和API不一致问题

## 0.2.0

- 规范了产品名称，从 `partner` 转换为 `cabital connect`
- 增加了合作方准备文档，为合作方接入本方系统做准备
- Connect 状态命名的细微调整，细化了`Rejected`的两种不同性质
- 增加了账户的错误处理，并调整了状态，转换为config里面

## 0.1.1

- 恢复了 API-Key，便于已经集成 CaaS 的合作方，集成 Connect
- 由于使用API-Key，移除访问 Path 中的 Partner Id
- 增加更多 API 细节，以及错误处理

## 0.1.0

- 提供最初的 Partner API 文档