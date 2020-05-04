# `event`

定义部件的事件。

## 属性

| 名称         | 类型      | 描述                     | 必填 |
| ------------ | --------- | ------------------------ | ---- |
| name         | `string`  | 属性名                   | 是   |
| label        | `string`  | 属性显示名               | 否   |
| defaultValue | `string`  | 属性默认值               | 否   |
| valueType    | `string`  | 属性值的类型             | 是   |
| description  | `string`  | 属性详细说明             | 否   |
| required     | `boolean` | 是否必填，默认为 `false` | 否   |
| arguments    | `Array`   | 输入参数列表             | 是   |

注意：

1. `valueType` 的值为：`function`

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