# `createWidget`

创建一个 Widget。

## 属性

| 名称                        | 类型      | 描述             | 必填 |
| --------------------------- | --------- | ---------------- | ---- |
| name                        | `string`  | Widget 名称      | 是   |
| label                       | `string`  | Widget 显示名    | 否   |
| description                 | `string`  | Widget 详细说明  | 否   |
| canHasChildren              | `boolean` | 是否包含子节点   | 否   |
| [properties](./properties.md) | `Array`   | 部件中的属性列表 | 是   |
| [events](./events.md)         | `Array`   | 部件中的事件列表 | 是   |

## 示例

```json
{
    "id": "create-widget-example",
    "author": "jinzw",
    "changes": [{
        "createWidget": {
            "name": "Button",
            "label": "按钮",
            "description": "",
            "canHasChildren": true,
            "properties": [],
            "events": []
        }
    }]
}
```

TODO: 删除 canHasChildren 属性，改为支持添加多个子节点。