# API 仓库

## 创建 git 仓库

在 [github](https://github.com) 或 [码云](https://gitee.com) 等基于 [git](https://git-scm.com/) 的源码托管平台上创建一个 git 仓库。可参考示例仓库 [blocklang/api-web-api](https://github.com/blocklang/api-web-api)。

注意：当前仅支持公开的仓库。

## 项目结构

API 仓库以增量的方式存储 API 变更记录。约定使用如下项目结构：

```text
项目根目录
    api.json                         - 存储 API 仓库的基本信息
    components                       - 存储所有 API 的变更记录
        {component}                  - 组件名称，要遵循变量名的命名规范
            changelog                - 存储变更文件
                {change_log}.json    - 描述 API 的变更记录，如新增一个 API
                {change_log}.json
```

可在以上目录结构的基础上增加目录或文件，如在 `{component}` 目录下增加描述组件 `README.md` 文件。

示例

如定义一个 [`console`](https://developer.mozilla.org/en-US/docs/Web/API/Console) 对象的项目结构：

```text
项目根目录
    api.json
    components
        console
            changelog
                202004091038_new_console.json
                202004101010_add_console_log.json
```

### api.json

`api.json` 用于描述 API 仓库的基本信息，包括如下属性：

| 名称        | 类型       | 描述                        | 必填 |
| ----------- | ---------- | --------------------------- | ---- |
| name        | `string`   | 组件库名称                  | 是   |
| displayName | `string`   | 组件库的显示名              | 否   |
| description | `string`   | 组件库的详细介绍            | 否   |
| category    | `string`   | 组件库的种类，值为 `WebAPI` | 是   |
| components  | `string[]` | 存储组件的相对路径          | 是   |

`components` 中存储的值为从项目根目录到 `{component}` 的相对路径，不包括 `changelog` 文件夹。

示例

```json
{
  "name": "api-web-func",
  "displayName": "",
  "description": "",
  "category": "WebAPI",
  "components": ["components/console"]
}
```

### {change_log}.json

该文件用于增量描述 API 的变更记录，以方便跟踪变化。

`{change_log}` 可任意命名，但建议在文件名中包括时间戳和操作概述，如 `202004291112_create_console.json` 中 `202004291112` 表示2020年4月29日11点12分，`create_console` 表示创建一个 `Console` 对象的 API。

支持的变更操作有：

1. [`createObject`](./create_object.md) - 创建一个 JavaScript 对象
2. [`addFunction`](./add_function.md) - 在 JavaScript 对象中增加一个函数

注意：

1. 因为此文件是 **增量** 描述 API 变更记录的；
2. 如果该文件没有发布，则可以随意调整；
3. 如果该文件已发布，则不能修改和删除。

这里有一个很关键的时间节点，即发布。一个文件已发布，是指文件中的内容稳定后，在 git 仓库上[标注了 tag](https://git-scm.com/docs/git-tag)。
