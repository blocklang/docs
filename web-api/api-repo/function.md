# `function`

描述函数签名。

## 属性

| 名称       | 类型     | 描述       | 必填 |
| ---------- | -------- | ---------- | ---- |
| name       | `string` | 函数名     | 是   |
| parameters | `Array`  | 输入参数   | 否   |
| returnType | `string` | 返回值类型 | 是   |

1. returnType 的值为: `void` 表示不返回值

## parameters 属性

| 名称     | 类型      | 描述                     | 必填 |
| -------- | --------- | ------------------------ | ---- |
| name     | `string`  | 参数名                   | 是   |
| type     | `string`  | 参数类型                 | 是   |
| optional | `boolean` | 可选参数，默认为 `false` | 是   |
| variable | `boolean` | 可变参数，默认为 `false` | 是   |

1. type 的值为：`any` 表示任何类型
2. 最后一个参数，才能被设置为可变参数

## 示例

```json
{
    "functions": [{
        "name": "log",
        "parameters": [{
            "name": "message",
            "type": "any",
            "optional": false,
            "variable": false
        }],
        "returnType": "void"
    }]
}
```