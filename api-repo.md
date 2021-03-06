# API 仓库

API 仓库以增量方式描述 API 的变更，是由多组 `change-set` 按固定的顺序组成的。所以 API 仓库中必须包含两种信息，一是 `change-set`，另一个是 `change-set` 的**生成时间**。同时为了方便管理，我们对 API 进行分组管理。

注意：`change-set` 中记录的是变更数据，不是最终结果数据，所以一旦发布后，就不能再修改或删除。

## 目录名和文件名的命名规范

往组件市场发布 API 仓库时，每个分组会以**生成时间**为序逐项解析 `change-set`。而生成时间是体现在目录名和文件名中的。

约定目录名和文件名中包含两部分信息，模式为 `{order}__{description}`。主要处理好两个问题：

1. `{order}` 不能变 - 用于定义生成时间，发布后不能再次改动，但可读性不高；
2. `{description}` 可能变 - 为了提高可读性，可包含API 属性信息，可重命名此节内容。

注意：

1. `{order}` 描述目录和文件的创建顺序，标识 changelog 的变更路径，**一旦发布后，就不允许修改**；
2. `{description}` 是用户友好的描述信息，任何时间皆可修改；
3. `__` 是两条下划线，用于分割 `{order}` 和 `{description}`，所以 `{order}` 中不能包含两条下划线；
4. `{order}` 是必填的，`{description}` 是可选的，如果没有 `{description}`，则可以删除分隔符 `__`。

规定 `{order}` 是时间戳，取创建时间，遵循 `yyyyMMddHHmm` 格式（精确到分），且同一层目录下的 `{order}` 值必须唯一，示例如下

```text
202005081600__users
202005081601__roles
```

注意：changelog 中记录的是过程数据，而不是最终结果，即使要删除某一个 RESTful API，也不能删除目录和 changelog 文件，但可以在目录和文件名中增加特殊标识，如 `202005081600__users_d` 中的 `_d` 表示已删除该资源（名称仅仅起标识作用，要真正删除，还需要编写删除用的 changelog）。

## 发布

这里有一个很关键的时间节点，即发布。一个文件已发布，是指文件中的内容稳定后，在 git 仓库上[标注了 tag](https://git-scm.com/docs/git-tag)。

注意：

1. 因为此文件是 **增量** 描述 API 变更记录的，`change-set` 文件没有发布，则可以随意调整；一旦发布，则不能修改和删除；
2. 往组件市场注册时，只解析发布版和 master 分支，不解析预发布版。

## Schemas

通常情况下 Widget 属性和事件的输入参数等数据类型为 `string`、`number` 等**基本**数据类型。但在某些情况下也可能为 `object`，这时我们在 Schema 文件中定义 `Object` 对象的结构。

存储 Schema 的目录结构如下

```text
项目根目录
    blocklang.json
    changelog/
        202004091028__console
            202004091038__new_console.json
            202004101010__add_console_log.json
        schemas/
            202007020853__User
                202007020854__new_schema.json
```

所有的 schema 定义存放在 `schemas` 目录下，一个 `schema` 对应一个目录，在目录中存放 schema 的变更记录。

### 使用 `createSchema`自定义数据类型

使用 `createSchema` 操作自定义数据类型。`schema` 属性定义参考 [schema](./service/api-repo/schema.md)。

#### 示例

```json
{
    "id": "create-schema-example",
    "author": "jinzw",
    "changes": [{
        "createSchema": {
            "name": "User",
            "type": "object",
            "properties": [{
                "name": "id",
                "type": "string"
            }, {
                "name": "name",
                "type": "string"
            }]
        }
    }]
}
```

### 使用自定数据类型

以下示例是在 `createWidget` 操作中使用自定义的 `User` 类型。

```json
{
    "id": "create-widget-example",
    "author": "jinzw",
    "changes": [{
        "createWidget": {
            "name": "TextInput",
            "label": "文本输入框",
            "description": "",
            "canHasChildren": true,
            "properties": [{
                "name": "loginUser",
                "label": "登录用户",
                "defaultValue": "",
                "valueType": "User"
            }],
            "events": [{
                "name": "onValue",
                "label": "输入值",
                "valueType": "function",
                "arguments": [
                    {
                        "name": "loginUser",
                        "label": "登录用户",
                        "defaultValue": "",
                        "valueType": "User"
                    }
                ]
            }]
        }
    }]
}
```

分别在 `properties` 和事件的 `arguments` 中的 `valueType` 中引用 `User` 类型。

### 命名空间

为了避免类型名重复的情况，可在 `name` 中包含命名空间，如 `name` 的值为 `a.User`，其中 `a` 为命名空间，用 `.` 分割。
在引用时要包含命名空间。