# `addProperty`

在 Widget 中增加一个或多个属性。

## 属性

| 名称                          | 类型    | 描述              | 必填 |
| ----------------------------- | ------- | ----------------- | ---- |
| [properties](./properties.md) | `Array` | Widget 的属性列表 | 是   |

注意：

1. 并不需要 `widgetName` 来指定 Widget，因为一个 Widget 的所有操作都是放在一个文件夹下的

## 示例

```json
{
    "id": "add-property-example",
    "author": "jinzw",
    "changes": [{
        "addProperty": {
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