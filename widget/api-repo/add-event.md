# `addEvent`

在 Widget 中增加一个或多个事件。

## 属性

| 名称                     | 类型     | 描述             | 必填 |
| ------------------------ | -------- | ---------------- | ---- |
| widgetName               | `string` | Widget 名称       | 是   |
| [events](./events.md) | `Array`  | Widget 的事件列表 | 是   |

注意：

1. `widgetName` 不区分大小写

## 示例

```json
{
    "id": "add-event-example",
    "author": "jinzw",
    "changes": [{
        "addEvent": {
            "widgetName": "TextInput",
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