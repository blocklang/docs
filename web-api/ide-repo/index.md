# IDE 仓库

支持以插件的方式加载 PROD 版的函数实现。

## 创建 git 仓库

在 [github](https://github.com) 或 [码云](https://gitee.com) 等基于 [git](https://git-scm.com/) 的源码托管平台上创建一个 git 仓库。可参考示例仓库 [blocklang/ide-web-api](https://github.com/blocklang/ide-web-api)。

注意：当前仅支持公开的仓库。

## 项目结构

本项目是一个 TypeScript 项目，使用 [@dojo/cli-build-app](https://github.com/dojo/cli-build-app) 构建工具。

约定使用如下项目结构：

```text
项目根目录
    blocklang.json        - 存储 IDE 仓库的基本信息
    package.json          - 项目配置文件
    tsconfig.json         - TypeScript 配置文件
    .commitlintrc.json    - commit lint 配置文件
    src                   - 存放源文件
        main.ts           - 项目主函数，用于往设计器中注册组件
        index.html        - web 容器页面
```

注意：可在以上目录结构的基础上增加目录或文件。因为本仓库只是提供通过插件的方式加载 web api 功能，直接使用 PROD 仓库中的实现，所以不需要 `{component}` 目录。

### blocklang.json

blocklang.json 用于描述 IDE 仓库的基本信息，包括如下属性：

| 名称        | 类型      | 描述                                    | 必填 |
| ----------- | --------- | --------------------------------------- | ---- |
| repo        | `string`  | 组件库类型，值为 `IDE`                  | 是   |
| name        | `string`  | 组件库名称                              | 是   |
| displayName | `string`  | 组件库的显示名                          | 否   |
| description | `string`  | 组件库的详细介绍                        | 否   |
| category    | `string`  | 组件库的种类，值为 `WebAPI`             | 是   |
| language    | `string`  | 组件库使用的编程语言，值为 `TypeScript` | 是   |
| std         | `boolean` | 是否标准库，默认为 `false`              | 否   |
| appType     | `string`  | app 类型，值为 `web`                    | 是   |
| api         | `object`  | 实现的 api 仓库信息                     | 是   |

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
  "repo": "IDE",
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
  }
}
```

注意：PROD 项目和 IDE 项目都要实现 API 项目，使用 `api.git` 和 `api.version` 指向 API 项目地址和版本号。

### package.json

在项目配置文件中配置依赖和命令脚本，默认使用如下配置：

```json
{
    "name": "ide-web-func",
    "version": "0.0.1",
    "displayName": "",
    "repository": {
        "type": "git",
        "url": "https://github.com/xxx/ide-web-func.git"
    },
    "scripts": {
        "build": "dojo build --mode dist --single-bundle --omit-hash",
        "test": "dojo test",
        "test:unit": "dojo build --mode unit && dojo test --unit --config local",
        "test:functional": "dojo build --mode functional && dojo test --functional --config local",
        "test:all": "dojo build --mode unit && dojo build --mode functional && dojo test --all --config local",
        "prettier": "prettier --write \"{src}/**/*.{ts}\""
    },
    "dependencies": {
        "@dojo/framework": "7.0.0-rc.2",
        "designer-core": "~0.0.1-alpha.83",
        "web-func": "0.0.1",
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

1. 在项目根目录下执行 `npm run build` 命令，会构建项目并将结果保存到根目录下的 `output/dist/` 文件夹中；
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
		"./src/**/*.ts"
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

### src/main.ts

往 [@blocklang/page-designer](https://github.com/blocklang/page-designer/) 中注册 JavaScript 对象。

```ts
import * as blocklang from "designer-core/blocklang";

import { GitUrlSegment, ExtensionJsObjectMap } from "designer-core/interfaces";
// 因为 console 关键字，所以重命名为 consoleWrapper
import * as consoleWrapper from "@blocklang/web-api/console";

const gitUrlSegment: GitUrlSegment = { website: "github.com", owner: "blocklang", repoName: "ide-web-api" };
const jsObjects: ExtensionJsObjectMap = {console/*此处的 key 值必须对应 api 中的 js 对象名*/: consoleWrapper};
blocklang.registerJsObjects(gitUrlSegment, jsObjects);
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
