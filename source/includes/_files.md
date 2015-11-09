# 文档

## 获取文档列表

```http
GET /files?folder=jUiAcImUVWuhIbcW HTTP/1.1
Host: api.shimo.im
```

```javascript
shimo.get('files', { qs: { folder: 'jUiAcImUVWuhIbcW' } });
```

> 请求成功后返回样例：

```json
[{
  "guid": "7WSjLS29I2Vqaa2X",
  "title": "文档一"
}, {
  "guid": "N25Dhp2vTT4uHoKt",
  "title": "文件夹一"
}]
```

获取指定目录下所有文档列表。

### HTTP 请求

`GET https://api.shimo.im/files`

### 请求 Query

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
folder | 否 | 无 | string | 如提供，则只获取指定文件夹下的文件列表，否则返回桌面文件

## 创建文档

```http
POST /files?folder=32 HTTP/1.1
Content-Type: application/json
Host: api.shimo.im
Content-Length: 15

{"folder":false}
```

```javascript
shimo.post('files', {
  qs: { folder: 32 },
  body: { folder: false }
});
```

> 请求成功后返回样例：

```json
{
  "guid": "68",
  "type": "file",
  "title": "新建文档",
  "content": "<div></div>",
  "url": "https://shimo.im/docs/2da92f"
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
isFolder | 否 | `false` | boolean | 创建文件还是文件夹
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
  "guid": "N25Dhp2vTT4uHoKt",
  "title": "文档一",
  "html": "<div>content goes here</div>"
}
```

获取指定文档的内容，包括标题、内容 HTML 等。

### HTTP 请求

`GET https://api.shimo.im/files/:fileId`

## 修改文档

```http
PATCH /files/17 HTTP/1.1
Content-Type: application/json
Host: api.shimo.im
Content-Length: 16

{"creator":"79"}
```

```javascript
shimo.patch('files/17', { body: { creator: 79 } });
```

修改文档，目前可以修改文档的管理员（默认情况下管理员即为文档的创建者）。

### HTTP 请求

`PATCH https://api.shimo.im/files/:fileId`

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
creator | 否 | 无 | string | 文档管理员的 id
