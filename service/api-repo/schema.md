# `schema`

数据结构描述，适用于 `requestBody` 和 `responses`。

## 属性

| 名称        | 类型     | 描述     | 必填 |
| ----------- | -------- | -------- | ---- |
| type        | `string` | 数据类型 | 是   |
| properties  | `schema` | 数据结构 | 是   |
| name        | `string` | 属性名   | 否   |
| description | `string` | 详细说明 | 否   |

注意：

1. `type` 的值为：`string`、`number`、`boolean`、`object` 和 `array`
2. `properties` 的类型为 `schema`，属于嵌套类型

<!-- 当后台功能稳定后，补充全属性 -->

## 示例

```json
{
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
}
```