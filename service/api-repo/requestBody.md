# `requestBody`

描述 http 的请求体。

## 属性

| 名称                  | 类型     | 描述             | 必填 |
| --------------------- | -------- | ---------------- | ---- |
| contentType           | `string` | 内容类型         | 是   |
| description           | `string` | 请求体的详细说明 | 否   |
| [schema](./schema.md) | `Object` | 请求体的结构     | 否   |

注意：

1. `contentType` 的值为：`application/json`、`application/octet-stream`、`application/x-www-form-urlencoded`、`text/plain` 和 `application/xml`。

## 示例

```json
{
    "requestBody": [{
        "contentType": "application/json",
        "description": "",
        "schema": {
            "type": "object",
            "properties": [{
                "name": "userId",
                "type": "string"
            }]
        }
    }]
}
```