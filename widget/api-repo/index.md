# API 仓库

## 创建 git 仓库

在 [github](https://github.com) 或 [码云](https://gitee.com) 等基于 [git](https://git-scm.com/) 的源码托管平台上创建一个 git 仓库。可参考示例仓库 [blocklang/api-widgets-bootstrap](https://github.com/blocklang/api-widgets-bootstrap)。

注意：当前仅支持公开的仓库。

## 项目结构

API 仓库以增量的方式存储 Widget 以及 Widget 属性和事件的变更记录。约定使用如下项目结构：

```text
项目根目录
    api.json                     - 存储 API 仓库的基本信息
    changelog                    - 存储所有 Widget 的变更记录
        {component}              - Widget 名称，Widget 名称使用中划线分割的小写字母，如部件名为 `TextInput` 时应写为 `text-input
            {change-set}.json    - 描述 Widget 以及 Widget 属性和事件的变更记录
            {change-set}.json
```

`{component}` 和 `{change-set}` 是变量，参考 [API 仓库中目录与文件名的命名规范](../../api-repo.md)。

可在以上目录结构的基础上增加目录或文件，如在 `{component}` 目录下增加描述组件的 `README.md` 文件。

示例，如定义一个 `Button` 部件的项目结构：

```text
项目根目录
    blocklang.json
    components
        202004091030__button
            202004091038__create_widget.json
            202004101010__add_property_value.json
```

### blocklang.json

`blocklang.json` 用于描述 API 仓库的基本信息，包括如下属性：

| 名称        | 类型     | 描述                        | 必填 |
| ----------- | -------- | --------------------------- | ---- |
| repo        | `string` | 组件库类型，值为 `API`      | 是   |
| name        | `string` | 组件库名称                  | 是   |
| displayName | `string` | 组件库的显示名              | 否   |
| description | `string` | 组件库的详细介绍            | 否   |
| category    | `string` | 组件库的种类，值为 `Widget` | 是   |

示例

```json
{
  "repo": "API",
  "name": "api-widget",
  "displayName": "",
  "description": "",
  "category": "Widget"
}
```

### {change-set}.json

该文件用于增量描述 Widget 以及 Widget 属性和事件的变更记录。

`{change-set}` 名称遵循 [API 仓库中目录与文件名的命名规范](../../api-repo.md)，如 `202004291112__create_widget.json` 中 `202004291112` 表示2020年4月29日11点12分，`create_widget` 表示创建一个 Widget。

支持的变更操作有：

1. [`createWidget`](./create-widget.md) - 创建一个 Widget
2. [`addProperty`](./add-property.md) - 在 Widget 中增加一个或多个属性
3. [`addEvent`](./add-event.md) - 在 Widget 中增加一个或多个事件

<!-- 
// TODO: 支持添加多个子部件，如 vue 的 slot 功能。
-->
