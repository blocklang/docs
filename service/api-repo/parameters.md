# `parameters`

描述 http 请求的输入参数。

## 属性

| 名称                  | 类型      | 描述                               | 必填 |
| --------------------- | --------- | ---------------------------------- | ---- |
| name                  | `string`  | 参数名                             | 是   |
| in                    | `string`  | 参数种类                           | 是   |
| [schema](./schema.md) | `Object`  | 参数值的数据类型                   | 是   |
| description           | `string`  | 参数的详细说明                     | 否   |
| required              | `boolean` | 是否必填，默认为 `true`            | 否   |
| allowEmptyValue       | `boolean` | 参数值是否允许为空，默认为 `false` | 否   |

注意：

1. `in` 的值为：`path`、`query`、`header` 和 `cookie`

## 示例

```json
{
    "parameters": [{
        "name": "userId",
        "in": "path",
        "schema": {
            "type": "string"
        }
    }]
}
```