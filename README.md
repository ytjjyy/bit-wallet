# API 接口文档

## 签名规则
- 使用 **HMAC-SHA256** 进行签名 签名字符串统一为小写 
- 所有参与签名的参数按 **字母升序排序**（键名排序）
- 每个参数用 `key=value` 的形式，用 **&** 连接
- 在最后追加 `&secret_key=密钥`
- 对整个字符串进行 HMAC-SHA256计算，得到 `sign`
- 服务端用相同方式生成签名，与客户端传来的 `sign` 对比验证合法性 
- 示例：sign = HMAC_SHA256(“amount=100&app_id=123&channel_id=1&client_ip=127.0.0.1&notify_url=https://xxx.com/callback&out_trade_no=987&secret_key=YOUR_SECRET”)

## 创建订单接口
/api/order/post
### 请求参数

| 参数名        | 类型    | 必填 | 说明                                                                 |
|---------------|---------|------|----------------------------------------------------------------------|
| app_id        | int64   | 是   | 商户编号，不能为空                                                    |
| amount        | float64 | 是   | 订单金额，必须 ≥ 0.01                                                |
| channel_id    | int64   | 是   | 通道 ID，不能为空，可选值：<br>1 = TRC20<br>2 = ERC20<br>3 = BEP20     |
| out_trade_no  | string  | 是   | 商户订单号，不能为空                                                  |
| notify_url    | string  | 是   | 回调地址，必须是有效 URL                                              |
| client_ip     | string  | 是   | 客户端 IP，最长 45 位，格式合法                                       |
| sign          | string  | 是   | 签名字符串，不能为空                                                  |

### 返回示例

```json
{
    "code": 0,
    "message": "success",
    "data": {
        "address": "T1",
        "channel_id": 1,
        "out_trade_no": " da5b7e975dde61",
        "url": "https://127.0.0.1/wallet/show.html"
    }
}
```


## 回调通知
${notify_url}
### 请求参数

| 参数名        | 类型    | 必填 | 说明                                                                 |
|---------------|---------|------|----------------------------------------------------------------------|
| app_id        | int64   | 是   | 商户编号                                                  |
| amount        | float64 | 是   | 订单金额                                               |
| channel_id    | int64   | 是   | 通道编号，可选值：<br>1 = TRC20<br>2 = ERC20<br>3 = BEP20             |
| out_trade_no  | string  | 是   | 商户订单号                                           |
| status        | string  | 是   | 回调状态，可选值：<br>0 = 等待支付<br>1 = 支付成功<br>2 = 支付超时|
| sign          | string  | 是   | 签名字符串                                                |

### 返回示例

```json
SUCCESS
```