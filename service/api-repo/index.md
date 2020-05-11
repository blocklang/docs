# API 仓库

## 创建 git 仓库

在 [github](https://github.com) 或 [码云](https://gitee.com) 等基于 [git](https://git-scm.com/) 的源码托管平台上创建一个 git 仓库。可参考示例仓库 [blocklang/service-demo](https://github.com/blocklang/service-demo)。

注意：当前仅支持公开的仓库。

## 项目结构

API 仓库以增量的方式存储 RESTful API 变更记录。约定使用如下项目结构：

```text
项目根目录
    blocklang.json                   - 存储 API 仓库的基本信息
    changelog                        - 存储所有 API 的变更记录
        {resources}                  - 资源对象，尽量使用复数
            {url}                    - 资源 url + http method
                {change-set}.json    - 描述 API 的变更记录
```

`resources`、`url` 和 `change-set` 是变量，参考 [API 仓库中目录与文件名的命名规范](../../api-repo.md)。

[OpenAPI 3.0](http://spec.openapis.org/oas/v3.0.3) 使用三层结构来组织 RESTful API：

* 第一层：资源 `{resources}` - 如 `users` 表示用户资源
* 第二层：路径 `{paths}` - 如 `users/{userId}` 表示单个用户
* 第三层：操作 `{operations}` - 对应 HTTP method，值为 `GET`、`POST`、`PUT` 和 `DELETE` 等

而我们将 `{paths}` 和 `{operations}` 两层合并为 `{url}`，如 `users_GET`。

以下示例描述了 `users` 和 `roles` 资源的目录结构：

```text
项目根目录
    blocklang.json                                  - 存储 API 仓库的基本信息
    changelog                                       - 存储所有 API 的变更记录
        202005081600__users                         - 用户资源，资源名推荐使用复数
            202005081601__users_GET                 - url，url 中有 / 时，可使用下划线代替，如 users/{userId} 应写为 users_{userId}
                    202005081602__create.json       - 描述 API 的变更记录
            202005081603__users_POST                - 新增一个用户信息
                    202005081604__create.json       - 创建一个 RESTful API
            202005081605__users_{userid}_PUT        - 更新一个用户信息
                    202005081606__create.json       - 创建一个 RESTful API
            202005081607__users_{userid}_DELETE     - 删除一个用户信息
                    202005081608__create.json       - 创建一个 RESTful API
        202005081608__roles                         - 角色资源
```

可在以上目录结构的基础上增加目录或文件，如目录下增加描述 API 的 `README.md` 文件。

### blocklang.json

`blocklang.json` 用于描述 API 仓库的基本信息，包括如下属性：

| 名称        | 类型     | 描述                         | 必填 |
| ----------- | -------- | ---------------------------- | ---- |
| repo        | `string` | 组件库类型，值为 `API`       | 是   |
| name        | `string` | 组件库名称                   | 是   |
| displayName | `string` | 组件库的显示名               | 否   |
| description | `string` | 组件库的详细介绍             | 否   |
| category    | `string` | 组件库的种类，值为 `Service` | 是   |

示例

```json
{
  "repo": "API",
  "name": "api-xx-service",
  "displayName": "",
  "description": "",
  "category": "Service"
}
```

### {change-set}.json

该文件用于增量描述 REST ful API 的变更记录。

`{change-set}` 遵循 [API 仓库中目录与文件名的命名规范](../../api-repo.md)，如 `202004291112__create_api.json` 中 `202004291112` 表示2020年4月29日11点12分，`create_api` 表示创建一个 RESTful API。

支持的变更操作有：

1. [`createApi`](./create-api.md) - 创建一个 RESTful API
