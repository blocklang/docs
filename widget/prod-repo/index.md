# PROD 仓库

存储生产环境使用的 Widget。

## 创建 git 仓库

在 [github](https://github.com) 或 [码云](https://gitee.com) 等基于 [git](https://git-scm.com/) 的源码托管平台上创建一个 git 仓库。可参考示例仓库 [blocklang/widgets-bootstrap](https://github.com/blocklang/widgets-bootstrap)。

注意：当前仅支持公开的仓库。

## 项目结构

PROD 项目分两种，一种是在项目内实现 Widget；另一种是第三方仓库中已实现了 Widget，但根目录下并不存在 `component.json` 文件，因此需要新建一个 PROD 仓库来存放 `component.json` 文件，并引用第三方仓库中的 Widget。

在 `component.json` 文件也可以同时引用本项目和其他项目中的 Widget。

### 在本仓库中实现 Widget

本仓库中存储的是一个 TypeScript 项目，使用 [@dojo/cli-build-widget](https://github.com/dojo/cli-build-widget) 构建工具。

```text
项目根目录
    component.json            - 存储 PROD 仓库的基本信息
    package.json              - 项目配置文件
    tsconfig.json             - TypeScript 配置文件
    .dojorc                   - dojo 配置文件，配置需要 build 的 Widget
    .commitlintrc.json        - commit lint 配置文件
    src                       - 存放 Widget 源文件
        {component}           - Widget 名称，约定使用中划线分割的小写字母
            index.tsx         - Widget 实现
            index.spec.tsx    - Widget 测试用例
```

注意：**本项目必须要发布到 [npmjs.com](https://npmjs.com)**。

### 本仓库引用第三方仓库中的 Widget

仓库根目录中只包含一个 `component.json` 文件，在其中引用另一个仓库中的 Widget，项目结构如下：

```text
项目根目录
    component.json            - 存储 PROD 仓库的基本信息
```

第三方仓库必须是用 Dojo 开发的 Widget 项目，如 [@dojo/widgets](https://github.com/dojo/widgets)。

注意：

1. 引用的第三方 Widget 库必须要发布到 [npmjs.com](https://npmjs.com)；
2. 本仓库不需要也无法发布到 [npmjs.com](https://npmjs.com)。

### component.json

component.json 用于描述 PROD 仓库的基本信息，包括如下属性：

| 名称        | 类型       | 描述                                    | 必填 |
| ----------- | ---------- | --------------------------------------- | ---- |
| name        | `string`   | 组件库名称                              | 是   |
| displayName | `string`   | 组件库的显示名                          | 否   |
| description | `string`   | 组件库的详细介绍                        | 否   |
| category    | `string`   | 组件库的种类，值为 `Widget`             | 是   |
| language    | `string`   | 组件库使用的编程语言，值为 `TypeScript` | 是   |
| std         | `boolean`  | 是否标准库，默认为 `false`              | 否   |
| dev         | `boolean`  | 是否用于开发模式，值只能为 `false`      | 否   |
| appType     | `string`   | app 类型，默认为 `web`                  | 是   |
| api         | `object`   | 实现的 api 仓库信息                     | 是   |
| components  | `string[]` | 存储 Widget 的路径                      | 是   |

注意：

1. `appType` 的可选值为：`web` 表示 web 部件，`android` 表示原生的 android 部件，`iOS` 表示原生的 iOS 部件，`wechatApp` 表示微信小程序等，当前仅支持 `web`。

`api` 属性：

| 名称    | 类型     | 描述                          | 必填 |
| ------- | -------- | ----------------------------- | ---- |
| git     | `string` | git 地址                      | 是   |
| version | `string` | 版本号，对应 git tag 的版本号 | 是   |

`components` 属性中存储 Widget 的路径，如果引用本项目的 Widget，则直接使用相对路径，如 `src/button`；如果引用的是另一个仓库中的部件，则使用项目名加 Widget 路径格式 `{RepoName}:{WidgetPath}`，如引用 `@dojo/widgets` 中的 [Button](https://github.com/dojo/widgets/blob/master/src/button/index.tsx) 按钮时，值为 `@dojo/widgets:src/button`。

以下示例中演示了包含了上述两种情况的 `components` 值：

```json
{
  "name": "web-func",
  "displayName": "",
  "description": "",
  "category": "WebAPI",
  "language": "TypeScript",
  "std": false,
  "dev": false,
  "appType": "web",
  "api": {
    "git": "https://github.com/blocklang/api-widgets-bootstrap",
    "version": "0.0.1"
  },
  "components": ["src/button", "@dojo/widgets:src/button"]
}
```

注意：

1. PROD 仓库和 IDE 仓库都要实现 API 仓库中的 API，使用 `api.git` 和 `api.version` 指向 API 仓库地址和版本号；
2. 如果 `components` 中全都引用第三方仓库中的 Widget，则不需要 build 和发布到 [npmjs.com](https://npmjs.com)。

### package.json

如果项目中包含 Widget 实现，则需要 build 这些部件并发布到 [npmjs.com](https://npmjs.com)，默认使用如下配置：

```json
{
    "name": "your-widgets",
    "version": "0.1.0",
    "repository": {
        "type": "git",
        "url": "https://github.com/your-repo/your-widgets.git"
    },
    "scripts": {
        "build:lib": "dojo build widget --mode dist --target lib && shx cp -r output/dist/* dist/",
        "build:lib:legacy": "dojo build widget --mode dist --target lib -l && shx cp -r output/dist/* dist/",
        "clean": "shx rm -rf dist && shx mkdir dist",
        "build": "npm run clean && npm run build:lib && npm run build:lib:legacy && shx cp package.json dist/ && shx cp README.md dist/",
        "dev": "dojo build --mode dev --watch --serve",
        "test": "dojo test",
        "test:unit": "dojo build --mode unit && dojo test --unit --config local",
        "test:functional": "dojo build --mode functional && dojo test --functional --config local",
        "test:all": "dojo build --mode unit && dojo build --mode functional && dojo test --all --config local",
        "prettier": "prettier --write \"{src,tests}/**/*.{ts,tsx,css}\""
    },
    "dependencies": {
        "@dojo/framework": "^7.0.0",
        "tslib": "~1.11.1"
    },
    "devDependencies": {
        "@commitlint/cli": "^8.3.5",
        "@commitlint/config-conventional": "^8.3.4",
        "@dojo/cli": "^7.0.0",
        "@dojo/cli-build-app": "^7.0.0",
        "@dojo/cli-build-widget": "^7.0.0",
        "@dojo/cli-test-intern": "^7.0.0",
        "@types/node": "^9.6.55",
        "@types/sinon": "9.0.0",
        "husky": "^4.2.3",
        "lint-staged": "^10.1.1",
        "prettier": "^2.0.2",
        "shx": "^0.3.2",
        "sinon": "^9.0.1",
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

注意：

1. 在项目根目录下执行 `npm run build` 命令，会构建项目并将结果保存到根目录下的 `dist/` 文件夹中；
2. 在项目根目录下执行 `npm run test` 命令，会运行测试用例；
3. 在提交代码时，git commit message 要遵循 [commit](https://www.conventionalcommits.org/en/v1.0.0/) 规范；
4. 需要将 `dist/` 文件中的内容发布到 [npmjs](https://www.npmjs.com/)；
5. 按需将依赖调整为最新版本。

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
		"./src/**/*.ts"
	]
}
```

注意：`"types": [ "intern" ]` 指出使用 [intern](https://theintern.io/) 编写测试用例。 

### .dojorc

在 `widgets` 中列出 js 模块的相对路径，如配置 `src/button/index.ts` 时，不需要包含 `/index.ts`。

```json
{
	"build-widget": {
		"widgets": [
			"src/button"
		]
	}
}
```

执行 `npm run  build:lib` 和 `npm run build:lib:legacy` 两个命令时，会 build 此文件中指定的 widget。

### .commitlintrc.json

用于规范 git commit 信息的格式，使用默认配置。

```json
{
    "extends": ["@commitlint/config-conventional"]
}
```

### index.tsx

Widget 名称和属性名要遵循 API 仓库中指定的名称。推荐实现基于函数的 Widget（相当于 react 的 [hook](https://reactjs.org/docs/hooks-intro.html)）。

如在 `src/button/index.tsx` 中创建一个 `Button` 部件：

> src/button/index.tsx

```tsx
import { tsx, create } from "@dojo/framework/core/vdom";

// 定义部件的属性
export interface ButtonProperties { }

const factory = create().properties<ButtonProperties>();

// 定义部件
export default factory(function Button({ properties }) {
	const { } = properties();
    
	return <button>按钮</button>;
});
```

### index.spec.tsx

测试 `Button` 部件的测试用例。本示例中使用 [intern](https://theintern.io/) 和 [@dojo/framework 中的测试 API](https://github.com/dojo/framework/tree/master/src/testing) 编写测试用例。示例代码如下：

> src/button/index.spec.tsx

```tsx
const { describe, it } = intern.getInterface('bdd');
import { tsx } from '@dojo/framework/core/vdom';
import renderer, { assertion, wrap } from '@dojo/framework/testing/renderer';

import Button from '.';

const WrappedButton = wrap('button');
const baseAssertion = assertion(() => <WrappedButton>按钮</WrappedButton>);

describe('Button', () => {
	it('Should render using the default properties', () => {
		const r = renderer(() => <Button />);
		// 断言渲染的内容
		r.expect(baseAssertion);
	});
});
```
如果需要 mock 对象，请使用 [sinon](https://sinonjs.org/)。执行 `npm run test` 命令运行测试用例。

## 编译

在项目根目录下：

1. 先执行 `npm install` 安装依赖
2. 然后执行 `npm run build`，以确认是否能编译通过，构建完成后，输出的内容保存在 `dist` 目录下
3. 然后执行 `npm login` 和 `npm publish` 等命令发布到 [npmjs.com](https://npmjs.com)
