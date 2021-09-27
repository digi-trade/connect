# Cabital Faas API Docuement

# 账户操作相关

### GET : /faas/partners/{partner_id}/limit

拿到该用户提现限额

### GET : /faas/partners/{partner_id}/balances

账户可用余额

### GET : /faas/partners/{partner_id}/balances/{SYMBOL}

账户单一币种余额

### GET : /faas/partners/{partner_id}/balances/{SYMBOL}/deposit/{method}

账户币种入账的信息



### POST : /faas/partners/{partner_id}/conversions

换币

```json
{
    "quote_id": "20210416225049:BTC-USDT:Customer",
    "quote": "61426.365",
    "pair": "BTC-USDT",
    "buy_amount": "0.001",
    "sell_amount": "61.426365",
    "major_ccy": "BTC"
}
```

### GET : /faas/partners/{partner_id}/debits

获取历史交易记录

### POST : /faas/partners/{partner_id}/debits

以客户的名义，从Cabital转出到Partner，收款方为Partner的关联账户

```json
{}
```

direct debit

transfer credit

### GET : /faas/partners/{partner_id}/credits

获取历史交易记录

### POST ：/faas/partners/{partner_id}/credits

以客户的名义，从Partner转账到Cabital，收款方为Cabital的关联账户

### GET : /faas/partners/{partner_id}/config

获取该Partner的配置，包含：

- 支持的币种（list）
  - 可否转账
  - 支持C+T
  - 支付方式（product management）
  - 整体limit
- 费率？

### GET : /faas/partners/{partner_id}/qutoes/{SYMBOL-SYMBOL}

获取最新的报价，Symbol为大写字母，用“-”连接，例如BTC-USD，其包含买入价，卖出价，中间价

现在按每个Partner一个报价，但现在实际共

**WS 报价？**

# 账户关联相关

### POST : /faas/partners/{partner_id}/account/match

提交Partner方KYC信息，返回Match的状态

### GET : /faas/partners/{partner_id}/account/match

返回当前连接状态

### GET : /faas/partners/{partner_id}/account/OTP

返回当前OTP状态

### Webhook : -> configurable

Account Connection 的状态

事件分为：

- Linked
- Created
- ReadyForMatching
- Matched
