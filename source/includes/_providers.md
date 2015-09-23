# 登录合作方

石墨为有限的登录合作方提供专用 API 以实现文件夹权限同步绑定等功能。

## 创建文档

根据合作方提供的实体 ID 来建立文档，并跳转到文档编写页面。

### HTTP 请求

`POST https://api.shimo.im/provider/:provider/files`

### 请求 Param

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
provider | 是 | 无 | string | 合作方名称

## 获取文档列表

返回合作方提供的实体 ID 对应的文件列表

### HTTP 请求

`GET https://api.shimo.im/provider/:provider/files`

### 请求 Param

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
provider | 是 | 无 | string | 合作方名称

### 请求 Query

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
providerEntity | 是 | 无 | string | 要获取的所属实体 ID

## 获取文档内容

返回合作方提供的实体 ID 对应的文件列表

### HTTP 请求

`GET https://api.shimo.im/provider/:provider/files/:guid`

### 请求 Param

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
provider | 是 | 无 | string | 合作方名称
guil | 是 | 无 | string | 文件的 GUID

### 请求 Query

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
providerEntity | 是 | 无 | string | 要获取的所属实体 ID

## 请求用户绑定合作方账号

### HTTP 请求

`POST https://shimo.im/sso/login/:provider`

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
callback | 是 | 无 | string | 登录成功后的回调地址
