# 介绍

石墨开放 API 提供了对石墨的读写接口，使得用户可以将自己的应用和公司的产品和石墨进行集成。

API 遵循 RESTful 风格，使用 JSON 数据格式作为请求和返回的数据格式。

所有 API 均支持 `?filter` 参数，用来请求服务器只返回需要的实体属性。

## 版本控制

当前石墨开放 API 的版本为 v2，进行接口调用时客户端需要通过 `Accept` HTTP Header 明确告知服务器当前调用的 API 接口，格式为 `Accept: application/vnd.shimo.v2+json`。如果不传递，默认将请求 v1 接口。

## 客户端

* Node.js 官方客户端：[Shimo.js](https://github.com/shimohq/shimo.js).
