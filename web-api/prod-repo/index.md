# PROD 仓库

存储生产环境使用的函数实现。

## 创建 git 仓库

在 [github](https://github.com) 或 [码云](https://gitee.com) 等基于 [git](https://git-scm.com/) 的源码托管平台上创建一个 git 仓库。可参考示例仓库 [blocklang/web-api](https://github.com/blocklang/web-api)。

注意：当前仅支持公开的仓库。

## 项目结构

本项目是一个 TypeScript 项目，使用 [@dojo/cli-build-widget](https://github.com/dojo/cli-build-widget) 构建工具。

约定使用如下项目结构：

```text
项目根目录
    blocklang.json           - 存储 PROD 仓库的基本信息
    package.json             - 项目配置文件
    tsconfig.json            - TypeScript 配置文件
    .dojorc                  - dojo 配置文件
    .commitlintrc.json       - commit lint 配置文件
    src                      - 存放源文件
        {component}          - 组件名称
            index.ts         - 组件实现
            index.spec.ts    - 测试用例
```

可在以上目录结构的基础上增加目录或文件，如在 `{component}` 目录下增加描述组件 `README.md` 文件。

示例

如定义一个 [`console`](https://developer.mozilla.org/en-US/docs/Web/API/Console) 对象的项目结构：

```text
项目根目录
    blocklang.json
    package.json
    tsconfig.json
    .dojorc
    .commitlintrc.json
    src
        console
            index.ts
            index.spec.ts
```

### blocklang.json

blocklang.json 用于描述 PROD 仓库的基本信息，包括如下属性：

| 名称        | 类型       | 描述                                    | 必填 |
| ----------- | ---------- | --------------------------------------- | ---- |
| repo        | `string`   | 组件库类型，值为 `PROD`                 | 是   |
| name        | `string`   | 组件库名称                              | 是   |
| displayName | `string`   | 组件库的显示名                          | 否   |
| description | `string`   | 组件库的详细介绍                        | 否   |
| category    | `string`   | 组件库的种类，值为 `WebAPI`             | 是   |
| language    | `string`   | 组件库使用的编程语言，值为 `TypeScript` | 是   |
| std         | `boolean`  | 是否标准库，默认为 `false`              | 否   |
| appType     | `string`   | app 类型，值为 `web`                    | 是   |
| api         | `object`   | 实现的 api 仓库信息                     | 是   |
| components  | `string[]` | 存储组件的相对路径                      | 是   |

api 属性：

| 名称    | 类型     | 描述                          | 必填 |
| ------- | -------- | ----------------------------- | ---- |
| git     | `string` | git 地址                      | 是   |
| version | `string` | 版本号，对应 git tag 的版本号 | 是   |

注意：

1. 只要发布后，`repo` 和 `category` 的值就不允许修改

示例

```json
{
  "repo": "PROD",
  "name": "web-func",
  "displayName": "",
  "description": "",
  "category": "WebAPI",
  "language": "TypeScript",
  "std": false,
  "appType": "web",
  "api": {
    "git": "https://github.com/blocklang/api-web-func.git",
    "version": "0.0.1"
  },
  "components": ["src/console"]
}
```

注意：PROD 项目和 IDE 项目都要实现 API 项目，使用 `api.git` 和 `api.version` 指向 API 项目地址和版本号。

### package.json

在项目配置文件中配置依赖和命令脚本，默认使用如下配置：

```json
{
    "name": "web-func",
    "version": "0.0.1",
    "displayName": "",
    "repository": {
        "type": "git",
        "url": "https://github.com/xxx/web-func.git"
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
        "prettier": "prettier --write \"{src}/**/*.{ts}\""
    },
    "dependencies": {
        "@dojo/framework": "7.0.0-rc.2",
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

在 `widgets` 中列出 js 模块的相对路径，如配置 `src/console/index.ts` 时，不需要包含 `/index.ts`。

```json
{
	"build-widget": {
		"widgets": [
			"src/console"
		]
	}
}
```

### .commitlintrc.json

用于规范 git commit 信息的格式，使用默认配置。

```json
{
    "extends": ["@commitlint/config-conventional"]
}
```

### index.ts

函数签名要遵循 API 仓库中指定的规范。

```ts
export function myFunc() {
    // do somthing
}
```

### index.spec.ts

测试 `index.ts` 的测试用例。本示例中使用 [intern](https://theintern.io/) 编写测试用例。示例代码如下：

```ts
const { describe, it } = intern.getInterface("bdd");
const { assert } = intern.getPlugin("chai");

describe("console", () => {
	it("assert something", () => {
		assert.isTrue(true);
	});
});
```
如果需要 mock 对象，请使用 [sinon](https://sinonjs.org/)。执行 `npm run test` 命令运行测试用例。

## 编译

在项目根目录下，先执行 `npm install` 安装依赖，然后执行 `npm run build`，以确认是否能编译通过。构建完成后，输出的内容保存在 `dist` 目录下。
