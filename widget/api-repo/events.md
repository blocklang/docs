# `event`

描述 Widget 的事件。

## 属性

| 名称        | 类型      | 描述                     | 必填 |
| ----------- | --------- | ------------------------ | ---- |
| name        | `string`  | 事件名                   | 是   |
| label       | `string`  | 事件显示名               | 否   |
| valueType   | `string`  | 属性值的类型             | 否   |
| description | `string`  | 事件详细说明             | 否   |
| arguments   | `Array`   | 输入参数列表             | 是   |

注意：

1. `valueType` 的值只能为：`function`

## `arguments` 属性

| 名称         | 类型     | 描述         | 必填 |
| ------------ | -------- | ------------ | ---- |
| name         | `string` | 参数名       | 是   |
| label        | `string` | 参数显示名   | 否   |
| defaultValue | `string` | 参数默认值   | 否   |
| valueType    | `string` | 参数的类型   | 是   |
| description  | `string` | 参数详细说明 | 否   |

注意：

1. `valueType` 的值为：`string`、`number`、`boolean`

## 示例

```json
{
    "events": [{
        "name": "onValue",
        "label": "输入值",
        "valueType": "function",
        "arguments": [
            {
                "name": "value",
                "label": "值",
                "defaultValue": "",
                "valueType": "string"
            }
        ]
    }]
}
```