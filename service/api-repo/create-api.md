# `createApi`

创建一个 RESTful API

## 属性

| 名称                            | 类型     | 描述                                                | 必填 |
| ------------------------------- | -------- | --------------------------------------------------- | ---- |
| url                             | `string` | 资源 url                                            | 是   |
| httpMethod                      | `string` | http method，值为 `GET`、`POST`、`PUT`、`DELETE` 等 | 是   |
| description                     | `string` | 详细说明                                            | 否   |
| [parameters](./parameters.md)   | `Array`  | 输入参数，包括 path、query、header 和 cookie        | 否   |
| [requestBody](./requestBody.md) | `Object` | 请求体                                              | 否   |
| [responses](./responses.md)     | `Array`  | 响应                                                | 否   |

## 示例

```json
{
    "id": "create-api-example",
    "author": "jinzw",
    "changes": [{
        "createApi": {
            "url": "users/{userId}",
            "httpMethod": "GET",
            "description": "根据用户标识获取详细信息",
            "parameters": [{
                "name": "userId",
                "in": "path",
                "schema": { "type": "string" }
            }],
            "responses": [{
                "statusCode": "200",
                "contentType": "application/json",
                "schema": {
                    "type": "object",
                    "properties": [
                        {
                            "name": "userId",
                            "type": "string"
                        }, {
                            "name": "username",
                            "type": "string"
                        }
                    ]
                }
            }]
        }
    }]
}
```
