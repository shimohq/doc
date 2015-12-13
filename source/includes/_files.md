# 文档

## 获取文档列表

```http
GET /files?folder=jUiAcImUVWuhIbcW&level=2 HTTP/1.1
Host: api.shimo.im
```

```javascript
shimo.get('files', { qs: { folder: 'jUiAcImUVWuhIbcW', level: 2 } });
```

> 请求成功后返回样例：

```json
[{
  "id":2,
  "guid":"kefUe93v77M5p8Mm",
  "name":"文件夹",
  "type":"folder",
  "updatedAt":"2015-12-13T14:08:54.000Z",
  "createdAt":"2015-12-13T14:08:54.000Z",
  "creator":1,
  "role":"owner",
  "children":[{
    "id":3,
    "guid":"GMu2Vzt3h5wei1rl",
    "name":"文档一",
    "type":"document",
    "updatedAt":"2015-12-13T14:08:54.000Z",
    "createdAt":"2015-12-13T14:08:54.000Z",
    "creator":1,
    "parentId":2,
    "role":"owner"
  }, {
    "id":4,
    "guid":"pTdupi2DGvkp6W5E",
    "name":"文档二",
    "type":"document",
    "updatedAt":"2015-12-13T14:08:54.000Z",
    "createdAt":"2015-12-13T14:08:54.000Z",
    "creator":1,
    "parentId":2,
    "role":"owner"
  }]
}]
```

获取指定目录下所有文档列表。其中 `type` 的取值如下：

取值 | 描述
--------- | -----------
folder | 文件夹
document | 文档
spreadsheet | 表格

### HTTP 请求

`GET https://api.shimo.im/files`

### 请求 Query

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
folder | 否 | 无 | string | 如提供，则只获取指定文件夹下的文件列表，否则返回桌面文件
level | 否 | 1 | number | 需要获取的文件夹层级，子文件夹可以通过 `children` 访问
excerpt | 否 | false | boolean | 是否需要获取文档的摘要

## 创建文档

```http
POST /files?folder=32 HTTP/1.1
Content-Type: application/json
Host: api.shimo.im
Content-Length: 15

{"type":"document"}
```

```javascript
shimo.post('files', {
  qs: { folder: 32 },
  body: { type: 'document' }
});
```

> 请求成功后返回样例：

```json
{
  "id":5,
  "guid":"lJdGNS5XCOsRNCma",
  "name":"新建文档",
  "html":"<div style=\"\"><br></div>\n\n",
  "type":"document",
  "creator":1,
  "role":"owner",
  "updatedAt":"2015-12-13T14:17:47.000Z",
  "createdAt":"2015-12-13T14:17:47.000Z"
}
```

创建一篇文档，并返回文件的信息。

### HTTP 请求

`POST https://api.shimo.im/files`

### 请求 Query

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
folder | 否 | 无 | string | 如提供，则文件会创建于指定的文件夹中

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
type | 否 | `document` | string | 文档类型，见“获取文档列表” API 的介绍
name | 否 | 新建文档 | string | 文件（夹）标题

## 获取文档内容

```http
GET /files/17 HTTP/1.1
Host: api.shimo.im
```

```javascript
shimo.get('files/N25Dhp2vTT4uHoKt');
```

> 请求成功后返回样例：

```json
{
  "id":3,
  "guid":"neFFrL5xmakcgPGS",
  "name":"我的文档",
  "html":"<div style=\"\"><br></div>\n\n",
  "type":"document",
  "updatedAt":"2015-12-13T14:20:22.000Z",
  "createdAt":"2015-12-13T14:20:22.000Z",
  "creator":1,
  "parentId":2,
  "role":"owner"
}
```

获取指定文档的内容，包括标题、内容 HTML 等。

### HTTP 请求

`GET https://api.shimo.im/files/:fileId`
