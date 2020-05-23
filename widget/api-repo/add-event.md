# `addEvent`

在 Widget 中增加一个或多个事件。

## 属性

| 名称                  | 类型    | 描述              | 必填 |
| --------------------- | ------- | ----------------- | ---- |
| [events](./events.md) | `Array` | Widget 的事件列表 | 是   |

注意：

1.  并不需要 `widgetName` 来指定 Widget，因为一个 Widget 的所有操作都是放在一个文件夹下的

## 示例

```json
{
    "id": "add-event-example",
    "author": "jinzw",
    "changes": [{
        "addEvent": {
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
    }]
}
```