# `createObject`

创建一个 JavaScript 对象。

## 属性

| 名称                     | 类型     | 描述             | 必填 |
| ------------------------ | -------- | ---------------- | ---- |
| name                     | `string` | 对象的名称       | 是   |
| description              | `string` | 对象的描述       | 否   |
| [functions](function.md) | `Array`  | 对象中的函数列表 | 是   |

## 示例

```json
{
    "id": "create-object-example",
    "author": "jinzw",
    "changes": [{
        "createObject": {
            "name": "console",
            "description": "",
            "functions": []
        }
    }]
}
```