# `addProperty`

在 Widget 中增加一个或多个属性。

## 属性

| 名称                          | 类型     | 描述              | 必填 |
| ----------------------------- | -------- | ----------------- | ---- |
| widgetName                    | `string` | Widget 名称       | 是   |
| [properties](./properties.md) | `Array`  | Widget 的属性列表 | 是   |

注意：

1. `widgetName` 不区分大小写

## 示例

```json
{
    "id": "add-property-example",
    "author": "jinzw",
    "changes": [{
        "addProperty": {
            "widgetName": "Button",
            "properties": [{
                "name": "title",
                "label": "标题",
                "defaultValue": "",
                "valueType": "string"
            }]
        }
    }]
}
```