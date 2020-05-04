# `property`

定义 Widget 的属性

## 属性

| 名称         | 类型      | 描述                     | 必填 |
| ------------ | --------- | ------------------------ | ---- |
| name         | `string`  | 属性名                   | 是   |
| label        | `string`  | 属性显示名               | 否   |
| defaultValue | `string`  | 属性默认值               | 否   |
| valueType    | `string`  | 属性值的类型             | 是   |
| description  | `string`  | 属性详细说明             | 否   |
| required     | `boolean` | 是否必填，默认为 `false` | 否   |
| options      | `Array`   | 输入参数列表             | 是   |

注意：

1. `valueType` 的值为：`string`、`number`、`boolean`

## `options` 属性

| 名称             | 类型     | 描述             | 必填 |
| ---------------- | -------- | ---------------- | ---- |
| value            | `string` | 选项值           | 是   |
| label            | `string` | 选项的显示名     | 否   |
| description      | `string` | 选项的详细说明   | 否   |
| valueDescription | `string` | 选项值的详细说明 | 否   |

## 示例

```json
{
    "properties": [{
        "name": "name",
        "label": "名称",
        "defaultValue": "",
        "valueType": "string",
        "options": []
    }]
}
```