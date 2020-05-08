# `responses`

描述 http 响应信息。

## 属性

| 名称                  | 类型     | 描述             | 必填 |
| --------------------- | -------- | ---------------- | ---- |
| statusCode            | `string` | http 状态码      | 是   |
| contentType           | `string` | 内容类型         | 是   |
| description           | `string` | 参数的详细说明   | 否   |
| [schema](./schema.md) | `Object` | 参数值的数据类型 | 是   |

注意：

1. `statusCode` 的[常用值(以下文字摘自阮一峰的日志)](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)为：
   * 200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
   * 201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
   * 202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
   * 204 NO CONTENT - [DELETE]：用户删除数据成功。
   * 400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
   * 401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
   * 403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
   * 404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
   * 406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
   * 410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
   * 422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
   * 500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。
2. `contentType` 的值为：`application/json`、`application/octet-stream`、`application/x-www-form-urlencoded`、`text/plain` 和 `application/xml`。

## 示例

```json
{
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
```