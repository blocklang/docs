# 帮助文档

[blocklang.com](https://blocklang.com) 平台的帮助文档，部署在 [https://blocklang.com/docs](https://blocklang.com/docs)。


## 业务配置人员

<!-- // TODO: -->

## 组件开发人员

Block Lang 平台是由基础平台和扩展库组成。

Block Lang 的扩展库存储在 git 仓库中，一套组件需要以下三类仓库：

* API 仓库 - 存储接口描述信息 
* PROD 仓库 - 在生产环境中运行的实现
* IDE 仓库 - 在页面设计器中运行的实现

PROD 和 IDE 仓库都要实现 API 仓库中的接口。
Block Lang 平台仅依赖于 API 仓库，不依赖于 PROD 仓库和 IDE 仓库。

而 BlockLang 组件库包括：

* [Widget 组件库](widget/index.md) - 存储 UI widget（包含 API、PROD 和 IDE 三个仓库）
* [Service 组件库](service/index.md) - 存储 RESTful API 描述（包含 API 仓库）
* [Web Api 组件库](web-api/index.md) - 存储 JavaScript 函数（包含 API、PROD 和 IDE 三个仓库）