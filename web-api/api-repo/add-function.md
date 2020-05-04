# `addFunction`

在所选的 JavaScript 对象中添加一个或多个函数。

## 属性

| 名称                     | 类型     | 描述             | 必填 |
| ------------------------ | -------- | ---------------- | ---- |
| objectName               | `string` | 对象的名称       | 是   |
| [functions](function.md) | `Array`  | 对象中的函数列表 | 是   |

## 示例

```json
{
    "id": "add-function-example",
    "author": "jinzw",
    "changes": [{
        "addFunction": {
            "objectName": "console",
            "description": "",
            "functions": [{
                "name": "info",
                "parameters": [{
                    "name": "message",
                    "type": "any",
                    "optional": false,
                    "variable": false
                }],
                "returnType": "void"
            }]
        }
    }]
}
```