# IDE 仓库

支持以插件的方式加载 PROD 版的函数实现。

## 创建 git 仓库

在 [github](https://github.com) 或 [码云](https://gitee.com) 等基于 [git](https://git-scm.com/) 的源码托管平台上创建一个 git 仓库。可参考示例仓库 [blocklang/ide-widgets-bootstrap](https://github.com/blocklang/ide-widgets-bootstrap)。

注意：当前仅支持公开的仓库。

## 项目结构

本项目是一个用 Dojo 开发 TypeScript 项目，使用 [@dojo/cli-build-app](https://github.com/dojo/cli-build-app) 构建工具。

约定使用如下项目结构：

```text
项目根目录
    component.json                   - 存储 IDE 仓库的基本信息
    package.json                     - 项目配置文件
    tsconfig.json                    - TypeScript 配置文件
    .dojorc                          - Dojo 配置文件
    .commitlintrc.json               - git commit lint 配置文件
    src                              - 存放 Widget 源文件
        main.ts                      - 项目主函数，用于往设计器中注册 Widget
        index.html                   - web 容器页面
        icons.svg                    - Widget 图标
        property                     - 设计器属性面板中使用的属性 Widget
            {Prop.tsx}               - 属性 Widget
            {Prop.m.css}             - 属性 Widget 的模块化样式文件
            {Prop.m.css.d.ts}        - 样式文件的类型声明文件
        {widget-1}                   - widget 名称，使用中划线分隔的小写字母
            edit                     - 存放编辑模式下使用的 Widget
                index.tsx            - Widget 源码
                index.m.css          - Widget 模块化样式文件
                index.m.css.d.ts     - 样式文件的类型声明文件
                index.spec.tsx       - Widget 的测试用例
                propertiesLayout.ts  - Widget 属性在属性面板中的布局，会引用 property 目录中的 Widget
            preview                  - 存放预览模式下使用的 Widget
                index.tsx            - Widget 源码
                index.m.css          - Widget 模块化样式文件
                index.m.css.d.ts     - 样式文件的类型声明文件
                index.spec.tsx       - Widget 的测试用例
        {widget-2}                   - widget 名称，使用中划线分隔的小写字母
                index.tsx            - Widget 源码，默认为编辑模式下使用的 Widget
                index.m.css          - Widget 模块化样式文件
                index.m.css.d.ts     - 样式文件的类型声明文件
                index.spec.tsx       - Widget 的测试用例
                propertiesLayout.ts  - Widget 属性在属性面板中的布局，会引用 property 目录中的 Widget
```

本项目中主要存储以下几种资源：

1. Widget 源码，放在 `{widget-1}` 或 `{widget-2}` 目录下，又分为
   1. 编辑模式下使用的 Widget，放在 `edit` 目录下
   2. 预览模式下使用的 Widget，放在 `preview` 目录下
   3. 如果只需定义编辑模式下使用的 Widget，则直接放在 `{widget}` 目录下
2. 属性的 Widget 源码，放在 `property` 目录下，一个属性定义一个属性 Widget
3. 每个 Widget 对应一个属性布局定义文件，放在 `propertiesLayout.ts` 文件中
4. Widget 的图标，放在 `icons.svg` 文件中
5. 往设计器中注册本仓库中的 Widget，放在 `main.ts` 文件中
6. 构建和发布 Widget 的命令脚本，放在 `package.json` 和 `.dojorc` 文件中
7. 仓库基本信息，放在 `component.json` 文件中

通过 `widget-1` 和 `widget-2` 两个 Widget，分别演示了编辑模式和预览模式两种情况下的项目结构。

注意：可在以上目录结构的基础上增加目录或文件。如为每个 Widget 增加 `README.md` 文件等。

### component.json

component.json 用于描述 IDE 仓库的基本信息，包括如下属性：

| 名称        | 类型      | 描述                                    | 必填 |
| ----------- | --------- | --------------------------------------- | ---- |
| name        | `string`  | 组件库名称                              | 是   |
| displayName | `string`  | 组件库的显示名                          | 否   |
| description | `string`  | 组件库的详细介绍                        | 否   |
| category    | `string`  | 组件库的种类，值为 `Widget`             | 是   |
| language    | `string`  | 组件库使用的编程语言，值为 `TypeScript` | 是   |
| std         | `boolean` | 是否标准库，默认为 `false`              | 否   |
| dev         | `boolean` | 是否用于开发模式，值为 `true`           | 是   |
| appType     | `string`  | app 类型，值为 `web`                    | 是   |
| api         | `object`  | 实现的 api 仓库信息                     | 是   |

IDE 仓库的 `dev` 值为 `true`。

api 属性：

| 名称    | 类型     | 描述                          | 必填 |
| ------- | -------- | ----------------------------- | ---- |
| git     | `string` | git 地址                      | 是   |
| version | `string` | 版本号，对应 git tag 的版本号 | 是   |

示例

```json
{
  "name": "ide-widgets",
  "displayName": "",
  "description": "",
  "category": "Widget",
  "language": "TypeScript",
  "std": false,
  "dev": true,
  "appType": "web",
  "api": {
    "git": "https://github.com/blocklang/api-widgets.git",
    "version": "0.0.1"
  },
  "components": ["src/button"]
}
```

如果存放源码的文件名为 `index.ts` 或 `index.tsx`，则 `components` 路径中不需要包含 `index.ts` 或 `index.tsx`。

注意：PROD 项目和 IDE 项目都要实现 API 项目，使用 `api.git` 和 `api.version` 指向 API 项目地址和版本号。

### package.json

在项目配置文件中配置依赖和命令脚本，默认使用如下配置：

```json
{
    "name": "ide-widgets",
    "version": "0.0.1",
    "displayName": "",
    "repository": {
        "type": "git",
        "url": "https://github.com/your-repo/ide-widgets.git"
    },
    "scripts": {
        "build": "dojo build --mode dist --single-bundle --omit-hash && shx cp src/icons.svg output/dist",
        "test": "dojo test",
        "test:unit": "dojo build --mode unit && dojo test --unit --config local",
        "test:functional": "dojo build --mode functional && dojo test --functional --config local",
        "test:all": "dojo build --mode unit && dojo build --mode functional && dojo test --all --config local",
        "prettier": "prettier --write \"{src,tests}/**/*.{ts,tsx,css}\""
    },
    "dependencies": {
        "@dojo/framework": "7.0.0-rc.2",
        "@blocklang/designer-core": "~0.0.1-alpha.87",
        "widgets": "0.0.1",
        "tslib": "~1.11.1"
    },
    "devDependencies": {
        "@commitlint/cli": "^8.3.5",
        "@commitlint/config-conventional": "^8.3.4",
        "@dojo/cli": "7.0.0-rc.1",
        "@dojo/cli-build-app": "7.0.0-rc.1",
        "@dojo/cli-build-widget": "7.0.0-rc.1",
        "@dojo/cli-test-intern": "7.0.0-rc.1",
        "@types/node": "^13.13.4",
        "@types/sinon": "^9.0.0",
        "husky": "^4.2.5",
        "lint-staged": "^10.2.0",
        "prettier": "^2.0.5",
        "shx": "^0.3.2",
        "sinon": "^9.0.2",
        "typescript": "~3.8.3"
    },
    "husky": {
        "hooks": {
            "commit-msg": "commitlint -E HUSKY_GIT_PARAMS",
            "pre-commit": "lint-staged"
        }
    },
    "lint-staged": {
        "*.{ts,tsx,css}": [
            "prettier --write",
            "git add"
        ]
    },
    "prettier": {
        "singleQuote": false,
        "tabWidth": 4,
        "useTabs": true,
        "printWidth": 120,
        "arrowParens": "always"
    }
}
```

其中 `dependencies` 中的 `widgets` 引用的是 PROD 版的 Widget 库。

注意：

1. 在项目根目录下执行 `npm run build` 命令，会构建项目并将结果保存到根目录下的 `output/dist/` 目录中；
2. **不**需要将 `output/dist/` 文件中的内容发布到 [npmjs](https://www.npmjs.com/)；
3. 在提交代码时，git commit message 要遵循 [commit](https://www.conventionalcommits.org/en/v1.0.0/) 规范；
4. 按需将依赖调整为最新版本。

### tsconfig.json

TypeScript 配置文件，默认使用如下配置，可按需调整：

```json
{
	"compilerOptions": {
		"declaration": false,
		"experimentalDecorators": true,
		"lib": [
			"dom",
			"es5",
			"es2015.promise",
			"es2015.iterable",
			"es2015.symbol",
			"es2015.symbol.wellknown"
		],
		"module": "umd",
		"moduleResolution": "node",
		"noUnusedLocals": true,
		"outDir": "_build/",
		"removeComments": false,
		"importHelpers": true,
		"downlevelIteration": true,
		"sourceMap": true,
		"strict": true,
		"target": "es5",
		"types": [ "intern" ]
	},
	"include": [
        "./src/**/*.ts",
        "./src/**/*.tsx"
	]
}
```

注意：`"types": [ "intern" ]` 指出使用 [intern](https://theintern.io/) 编写测试用例。 

### .commitlintrc.json

用于规范 git commit 信息的格式，使用默认配置。

```json
{
    "extends": ["@commitlint/config-conventional"]
}
```

### property 目录

Block Lang 平台引入组件市场概念，拼装业务软件的原材料统统通过插件的方式集成进来。这些原材料包括，组成可视化页面的 Widget，加载远程数据的 Service，丰富页面逻辑的 Web API，甚至包括 Page Designer 中的属性面板，也是允许用户按需定制的。

property 目录中存储的是可视化的属性编辑面板。以某一个 Widget 的 `value` 属性为例：

> property/Value.tsx

```tsx
import { create, tsx } from "@dojo/framework/core/vdom";
import { SingleProperty } from "@blocklang/designer-core/interfaces";

// SingleProperty 是 blocklang 提供的与 Page Designer 集成的接口
const factory = create().properties<SingleProperty>();

export default factory(function Value({ properties }) {
	const { 
		index, // value 属性在属性列表中的索引
		value = "", // value 属性的值
		onPropertyChanged // 如果在此部件中修改了 value 值，则向外部发布通知
	} = properties();

	return (
		<div>
			<input
				key="input"
				value={value}
				classes={[c.form_control]}
				oninput={(event: KeyboardEvent<HTMLInputElement>) => {
					const value = event.target.value;
					onPropertyChanged({ index, newValue: value, isChanging: false, isExpr: false });
				}}
			/>
		</div>
	);
});
```

1. 属性 Widget 的 properties 统一使用 `SingleProperty` 接口；
2. 属性 Widget 要显示属性值，如 `value={value}`；
3. 当属性值修改后，要向外部发布通知，如 `oninput` 事件处理函数中调用 `onPropertyChanged` 函数。

一个 Widget（指 Button 等 UI Widget） 通常包含多个属性，此时就需要使用属性布局定义文件 `propertiesLayout.ts` 来组合所有属性 Widget。

### propertiesLayout.ts

我们将 `propertiesLayout.ts` 称为**属性布局定义文件**。在属性布局定义文件中，定义了属性面板内所有属性 Widget 的排列方式。定义文件会默认导出 JSON 对象，结构如下：

> propertiesLayout.ts

```ts
// 导入 value 属性的 Widget
import Value from "../../property/Value";
import { PropertyLayout } from '@blocklang/designer-core/interfaces';

const propertiesLayout: PropertyLayout[] = [
	{
		propertyName: "value",  // 属性名，通常与 API 中的名称保持一致（注意：也会有例外）
		propertyLabel: "值",    // 在属性面板中的显示文本
		propertyWidget: Value   // 对应的 Widget
	}
];

export default propertiesLayout;
```

`propertiesLayout.ts` 支持以下属性：

<!-- 
当修复完 [#4](https://github.com/blocklang/designer-core/issues/4) 之后，

1. 在属性名后加上类型
2. 标明属性是否必填
-->

3. `propertyName`   - 属性名称，通常取 API 仓库中定义的属性名，但也可能是属性分组名
4. `propertyWidget` - 属性 Widget，在此处与属性 Widget 关联
5. `propertyLabel`  - 属性显示名
6. `propertyGroup`  - 属性组，适用于多个属性在一个区域内显示
7. `target`         - 如果 `propertyName` 没有指向属性时，使用 `target` 来指向对应的属性
8. `indent`         - 空格，将属性组内部的各个属性隔开
9.  `newline`        - 换行，当一个属性组内的属性较多，无法在一行内显示时，进行换行显示
10. `divider`        - 分割线，用于在属性组内分隔原生属性与复合属性，以及为区间部件添加连接符等，支持三个值
   1. `vertical`        - 垂直分割线
   2. `horizontal`      - 水平分割线
   3. `segement`        - 中划线
11. `if`             - 按条件匹配，如果条件表达式解析为真，则显示该 Widget，否则不显示
12. `widget`        - 目标 Widget 表达式
13. `propertyValue` - 属性值数组

#### 用法示例

1. 一个属性对应一个 Widget

   ```ts
   {
       "propertyName": "maxWidth",
       "propertyLabel": "最大宽度",
       "propertyWidget": PropertyWidget1
   }
   ```

2. 属性 Widget 中组合多个有逻辑关联的属性，将这些属性放在 `propertyGroup` 中

   ```ts
   {
       "propertyLabel": "边框",
       "propertyGroup": [
           {
               "propertyName": "borderLeft"
           },
           {
               "propertyName": "borderTop"
           },
           {
               "propertyName": "borderRight"
           }
        ]
   }
   ```

3. 在属性组内组合使用 `indent`，`newline` 和 `divider` 调整布局

   分别使用空格和换行分开三个字体相关的属性：

   ```ts
   {
       "propertyLabel": "字体",
       "propertyGroup": [
           {
               "propertyName": "fontWeight"
           },
           {
               "indent": true
           },
           {
               "propertyName": "fontItalic"
           },
           {
               "newline": true
           },
           {
               "propertyName": "textColor"
           }
       ]
   }
   ```

   以下示例中演示了 `target` 属性，对 `margin` 的设置会同时应用到 `marginLeft`、`marginTop`、`marginRight` 和 `marginBottom` 四个属性上，而此处的 `propertyName` 属性的值，也就是 `margin` 在 API 规范中并没有定义，只是一个临时表意的复合属性:

   ```ts
   {
       "propertyLabel": "外边距",
       "propertyGroup": [
           {
               "propertyName": "marginLeft"
           },
           {
               "divider": "horizontal"
           },
           {
               "propertyName": "margin",
               "target": [
                   "marginLeft",
                   "marginTop",
                   "marginRight",
                   "marginBottom"
               ]
           }
       ]
   }
   ```

4. 当属性 Widget 中组合展示多个属性时，需要使用 `target` 标识出对应哪个 Widget 中的哪个属性。`target` 值可以是字符串或者 json 对象，字符串直接写属性名，解析时取其对应的属性 Widget；使用 json 对象时，`widget` 表示目标 Widget，当前仅支持 `{self}` 和 `{parent}`，`propertyName` 表示属性名称

   ```ts
   {
       "propertyLabel": "弹性容器子部件",
       "propertyName": "flexItem",
       "target": [
           {"widget": "{parent}", "propertyName": "flexDirection"},
           "alignSelf"
       ]
   }
   ```

5. 当满足某个前置条件才显示属性部件时，需要使用 `if` 做条件判断，`widget` 用于表示判断的目标（同上），`propertyName` 表示目标的属性名称，`propertyValue` 用于表示符合条件的值数组

   如果 `display` Widget 的值为 `flex` 或 `inlineFlex` 时，显示 `flexContainer`

   ```ts
   {
       "if": {"widget": "{self}", "propertyName": "display", "propertyValue": ["flex", "inlineFlex"]},
       "propertyLabel": "弹性容器",
       "propertyName": "flexContainer"
   }
   ```

<!-- 
第 4、5 点解释的还不够清楚，优化完接口后完善。
-->

### widget 源码

开发在 Page Designer 中使用的 Widget。Page Designer 有两种模式：

1. 预览模式 - 预览界面的真实效果
2. 编辑模式 - 所见即所得的设计界面

这两种模式使用的 Widget 稍有不同，编辑模式的 Widget，是需要在预览模式 Widget 的基础上增加设计交互功能。

通常预览模式下的 Widget 是直接使用 PROD 仓库中对应的组件；但如果 PROD 中的组件不能满足需求时，也可以在此仓库（IDE）中实现一个预览模式下使用的 Widget。如何使用 Dojo 开发基于函数的 Widget，详见[Dojo Widget Authoring system](https://github.com/dojo/framework/blob/master/src/core/README.md)。

`@blocklang/designer-core/middleware/ide` 模块提供的 `ide` 中间件，囊括设计交互功能。在开发 IDE 版 Widget 时，直接调用 `ide` 中提供的相关方法即可。

<!-- 支持跳转到介绍 ide 中间件的 API 文档 -->

以下示例，演示如何定义编辑模式下使用的 Widget。

<!-- 当移除掉 alwaysRenderActiveWidget 后，示例中的代码改为 tsx 格式 -->

> button/index.tsx

```tsx
import { create, v, w } from "@dojo/framework/core/vdom";
// 使用在 PROD 仓库中定义的属性接口
import { TextInputProperties } from "widgets-bootstrap/text-input";
// 使用 ide 中间件
import ide from "@blocklang/designer-core/middleware/ide";
// 需要屏蔽 dom 元素的事件时，在 dom 元素上放一个遮盖层
import Overlay from "@blocklang/designer-core/widgets/overlay";
import * as css from "./index.m.css";

const factory = create({ ide }).properties<TextInputProperties>();

export default factory(function TextInput({ properties, middleware: { ide } }) {

    // 在 root 节点上增加 ide 扩展功能
	ide.config("root");

	const { value = "", onValue } = properties();
	return [
		v("input", {
			key: "root",
			classes: [css.root, "form_control"],
			value,
			oninput: (event: KeyboardEvent<HTMLInputElement>) => {
				event.stopPropagation();
				const value = event.target.value;
				onValue && onValue(value);
			},
        }),
        // 增加遮盖层
        w(Overlay, { ...ide.getFocusNodeOffset(), ...ide.activeWidgetEvents() }),
        // 如果获取焦点，则界面有变动时就重新渲染该部件
		ide.alwaysRenderActiveWidget()
	];
});
```

### widget 测试用例

测试 `TextInput` 部件的测试用例。本示例中使用 [intern](https://theintern.io/) 和 [@dojo/framework 中的测试 API](https://github.com/dojo/framework/tree/master/src/testing) 编写测试用例。

示例略。

### src/main.ts

往 [@blocklang/page-designer](https://github.com/blocklang/page-designer/) 中注册三类资源：

1. 设计模式下使用的 Widget
2. 设计模式下使用的属性布局定义文件
3. 预览模式下使用的 Widget

```ts
import * as blocklang from "@blocklang/designer-core/blocklang";
import TextInputPreview from "widgets-bootstrap/text-input";
import TextInputIde from "./text-input";
import TextInputPropertiesLayout from "./text-input/propertiesLayout";
import { ExtensionWidgetMap, GitUrlSegment } from "@blocklang/designer-core/interfaces";

// ide 仓库地址
const gitUrlSegment: GitUrlSegment = { 
    website: "github.com", 
    owner: "blocklang", 
    repoName: "ide-widgets" 
};
const widgets: ExtensionWidgetMap = {
    // 文本输入框
	TextInput: { 
        widget: TextInputPreview, // 预览模式下使用
        ideWidget: TextInputIde, // 编辑模式下使用
        propertiesLayout: TextInputPropertiesLayout // 属性布局定义
    }
};
// 往设计器中注册 Widget，设计器就可以使用这三类 Widget
blocklang.registerWidgets(gitUrlSegment, widgets);
```

### src/icons.svg

`icons.svg` 中存储所有 Widget 图标。会显示在 [Page Designer](https://github.com/blocklang/page-designer) 的 Widget 面板中。

1. 文件名必须是 `icons.svg`，且放在 `src/` 目录下
2. 一个部件对应一个 `symbol` 节点，且 `symbol` 节点的 `id` 必须与 Widget 名保持一致，如 `TextInput`；
3. 图标库使用的是 svg sprites，因此必须放在 `symbol` 节点内；
4. 所有的 `symbol` 节点必须放在 `svg` 节点中，并且添加 `display: none` 样式。

示例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" style="display: none;">
    <symbol id="TextInput" viewBox="0 0 1024 1024">
        <title>TextInput</title>
        <g style="pointer-events:all">
            <g stroke-opacity="1">
                <rect stroke="#808080" height="468.80658" width="1156.54317" y="358.18931" x="-69.53086707336425" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="32" fill="#ffffff" fill-opacity="1" stroke-opacity="1"></rect>

                ...

            </g>
        </g>
    </symbol>
</svg>
```

### src/index.html

在项目中直接使用以下 html 模板即可，无需修改。因为在执行 `npm run build` 时，需要将生成的 `main.js` 放在 `index.html` 中，但是插件加载机制会直接加载 `main.js` 文件，不会使用此页面。

```html
<!DOCTYPE html>
<html lang="en-us" dir="ltr">

<head>
	<meta charset="utf-8">
	<title>web-func</title>
	<meta name="theme-color" content="#222127">
	<meta name="viewport" content="width=device-width, initial-scale=1">
</head>

<body>
</body>

</html>
```

## 编译

在项目根目录下，先执行 `npm install` 安装依赖，然后执行 `npm run build`，以确认是否能编译通过。构建完成后，输出的内容保存在 `output/dist` 目录下。
