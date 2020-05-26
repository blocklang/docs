# `addFunction`

在所选的 JavaScript 对象中添加一个或多个函数。

## 属性

| 名称                     | 类型     | 描述             | 必填 |
| ------------------------ | -------- | ---------------- | ---- |
| [functions](function.md) | `Array`  | 对象中的函数列表 | 是   |

注意：

1.  并不需要 `objectName` 来指定 Object，因为一个 Object 的所有操作都是放在一个文件夹下的

## 示例

```json
{
    "id": "add-function-example",
    "author": "jinzw",
    "changes": [{
        "addFunction": {
            "functions": [{
                "name": "info",
                "description": "",
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