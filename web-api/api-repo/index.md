# API 仓库

## 创建 git 仓库

在 [github](https://github.com) 或 [码云](https://gitee.com) 等基于 [git](https://git-scm.com/) 的源码托管平台上创建一个 git 仓库。可参考示例仓库 [blocklang/api-web-api](https://github.com/blocklang/api-web-api)。

注意：当前仅支持公开的仓库。

## 项目结构

API 仓库以增量的方式存储 API 变更记录。约定使用如下项目结构：

```text
项目根目录
    api.json                         - 存储 API 仓库的基本信息
    changelog                        - 存储所有 API 的变更记录
        {component}                  - 组件名称
            {change-set}.json    - 描述 API 的变更记录，如新增一个 API
            {change-set}.json
```

`{component}` 和 `{change-set}` 是变量，参考 [API 仓库中目录与文件名的命名规范](../../api-repo.md)。

可在以上目录结构的基础上增加目录或文件，如在 `{component}` 目录下增加描述组件的 `README.md` 文件。

示例

如定义一个 [`console`](https://developer.mozilla.org/en-US/docs/Web/API/Console) 对象的项目结构：

```text
项目根目录
    api.json
    changelog
        202004091028__console
                202004091038__new_console.json
                202004101010__add_console_log.json
```

### api.json

`api.json` 用于描述 API 仓库的基本信息，包括如下属性：

| 名称        | 类型       | 描述                        | 必填 |
| ----------- | ---------- | --------------------------- | ---- |
| name        | `string`   | 组件库名称                  | 是   |
| displayName | `string`   | 组件库的显示名              | 否   |
| description | `string`   | 组件库的详细介绍            | 否   |
| category    | `string`   | 组件库的种类，值为 `WebAPI` | 是   |

示例

```json
{
  "name": "api-web-func",
  "displayName": "",
  "description": "",
  "category": "WebAPI"
}
```

### {change-set}.json

该文件用于增量描述 API 的变更记录，以方便跟踪变化。

`{change-set}` 名称遵循 [API 仓库中目录与文件名的命名规范](../../api-repo.md)，如 `202004291112__create_console.json` 中 `202004291112` 表示2020年4月29日11点12分，`create_console` 表示创建一个 `Console` 对象的 API。

支持的变更操作有：

1. [`createObject`](./create-object.md) - 创建一个 JavaScript 对象
2. [`addFunction`](./add-function.md) - 在 JavaScript 对象中增加一个或多个函数
