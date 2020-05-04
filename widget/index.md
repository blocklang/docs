# Widget

分别使用三类 git 仓库管理 Widget：

1. [API 仓库](api-repo/index.md) - 用于描述 Widget 基本信息、属性信息和事件信息
2. [PROD 仓库](prod-repo/index.md) - 在生产环境使用，存储 Widget 实现
3. [IDE 仓库](ide-repo/index.md) - 在[设计器](https://github.com/blocklang/page-designer/)中使用，提供在设计器中使用的 Widget，并支持通过插件的方式加载 Widget

IDE 仓库中的 Widget 要在 PROD 仓库中的 Widget 之上增加设计器交互功能，所以 IDE 仓库中也需要实现一套 IDE 版的 Widget。