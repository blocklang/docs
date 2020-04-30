# Web 函数

分别使用三类 git 仓库管理 Web 函数

1. [API 版仓库](api-repo/index.md) - 用于描述 web 函数接口
2. [PROD 版仓库](prod-repo/index.md) - 在生产环境使用，存储 web 函数的实现
3. [IDE 版仓库](ide-repo/index.md) - 在[设计器](https://github.com/blocklang/page-designer/)中使用，支持通过插件的方式加载函数实现

因为 PROD 版和 IDE 版的函数实现是完全一致的，所以 IDE 版仓库会依赖 PROD 版仓库，并直接使用 PROD 仓库中的函数实现。