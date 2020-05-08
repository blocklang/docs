# `responses`

描述 http 响应信息。

## 属性

| 名称                  | 类型     | 描述             | 必填 |
| --------------------- | -------- | ---------------- | ---- |
| statusCode            | `string` | http 状态码      | 是   |
| contentType           | `string` | 内容类型         | 是   |
| description           | `string` | 参数的详细说明   | 否   |
| [schema](./schema.md) | `Object` | 参数值的数据类型 | 是   |

## 示例

```json
{
    "responses": [{
        "statusCode": "200",
        "contentType": "application/json",
        "schema": {
            "type": "object",
            "properties": [
                {
                    "name": "userId",
                    "type": "string"
                }, {
                    "name": "username",
                    "type": "string"
                }
            ]
        }
    }]
}
```