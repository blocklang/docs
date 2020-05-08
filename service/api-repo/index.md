# API 仓库

## 创建 git 仓库

在 [github](https://github.com) 或 [码云](https://gitee.com) 等基于 [git](https://git-scm.com/) 的源码托管平台上创建一个 git 仓库。可参考示例仓库 [blocklang/service-demo](https://github.com/blocklang/service-demo)。

注意：当前仅支持公开的仓库。

## 项目结构

API 仓库以增量的方式存储 RESTful API 变更记录。约定使用如下项目结构：

```text
项目根目录
    api.json                         - 存储 API 仓库的基本信息
    changelog                        - 存储所有 API 的变更记录
        {resources}                  - 资源对象，尽量使用复数
            {url}                    - 资源 url + http method
                {change-set}.json    - 描述 API 的变更记录
```

RESTful API 使用三层结构来组织：

* 第一层：资源 `{resources}` - 如 `users` 表示用户资源
* 第二层：路径 `{paths}` - 如 `users/{userId}` 表示单个用户
* 第三层：操作 `{operations}` - 对应 HTTP method，值为 `GET`、`POST`、`PUT` 和 `DELETE` 等

`{url}` 中包含 `{paths}` 和 `{operations}` 两层信息，如 `users_get`。

要妥善解决的问题：

1. 因为 API 仓库详细记录变更历史，所以当某项 changelog 文件发布后，就不能再修改文件路径名（包括目录名）和文件内容；
2. 但是 `{resources}`、 `{paths}` 作为目录名以及 API 属性，也会出现重命名的情况，所以不能因为修改 API 属性，而打乱了变更的执行顺序。

约定目录名和文件名中包含两部分信息，遵循此模式 `{order}__{description}`。以下命名规范适用于 `{resources}`、`{urls}` 和 `{change-set}`：

1. `{order}` 是目录和文件的创建顺序，标识 changelog 的变更路径，**一旦发布后，就不允许修改**；
2. `{description}` 是用户友好的描述信息，任何时间皆可修改；
3. `__` 是两条下划线，用于分割 `{order}` 和 `{description}`，所以 `{order}` 中不能包含两条下划线；
4. `{order}` 是必填的，`{description}` 是可选的，如果没有 `{description}`，则可以删除分隔符 `__`。

其中 `{order}` 是时间戳，值为创建时间，遵循 `yyyyMMddHHmm` 格式，精确到分即可，且同一层目录下的 `{order}` 值必须唯一，如下

```text
202005081600__users
202005081601__roles
```

注意：changelog 中记录的是过程数据，而不是最终结果，即使要删除某一个 RESTful API，也不能删除目录和 changelog 文件，但可以在目录和文件名中增加特殊标识，如 `202005081600__users_d` 中的 `_d` 表示已删除该资源（名称仅仅起标识作用，要真正删除，还需要编写删除用的 changelog）。

以下示例描述了 `users` 和 `roles` 资源的目录结构：

```text
项目根目录
    api.json                                        - 存储 API 仓库的基本信息
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

### api.json

`api.json` 用于描述 API 仓库的基本信息，包括如下属性：

| 名称        | 类型       | 描述                        | 必填 |
| ----------- | ---------- | --------------------------- | ---- |
| name        | `string`   | 组件库名称                  | 是   |
| displayName | `string`   | 组件库的显示名              | 否   |
| description | `string`   | 组件库的详细介绍            | 否   |
| category    | `string`   | 组件库的种类，值为 `Service` | 是   |

示例

```json
{
  "name": "api-xx-service",
  "displayName": "",
  "description": "",
  "category": "Service"
}
```

### {change-set}.json

该文件用于增量描述 REST ful API 的变更记录。

`{change_log}` 遵循 `{order}__{description}` 命名规范，如 `202004291112__create_api.json` 中 `202004291112` 表示2020年4月29日11点12分，`create_api` 表示创建一个 RESTful API。

支持的变更操作有：

1. [`createApi`](./create-api.md) - 创建一个 RESTful API

注意：

1. 因为此文件是 **增量** 描述 API 变更记录的；
2. 如果该文件没有发布，则可以随意调整；
3. 如果该文件已发布，则不能修改和删除。

这里有一个很关键的时间节点，即发布。一个文件已发布，是指文件中的内容稳定后，在 git 仓库上[标注了 tag](https://git-scm.com/docs/git-tag)。