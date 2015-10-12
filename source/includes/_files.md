# 文档

## 获取文档列表

```http
GET /files?folderId=jUiAcImUVWuhIbcW&subFolders%20=true HTTP/1.1
Host: api.shimo.im
```

```shell
curl -X "GET" "https://api.shimo.im/files?folderId=jUiAcImUVWuhIbcW
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
folderId | 否 | 无 | string | 如提供，则只获取指定文件夹下的文件列表，否则返回桌面文件

## 创建文档

```http
POST /files?parentId=32 HTTP/1.1
Content-Type: application/json
Host: api.shimo.im
Content-Length: 15

{"type":"file"}
```

```shell
curl -X "POST" "https://api.shimo.im/files?parentId=32" \
	-H "Content-Type: application/json" \
	-d "{\"type\":\"file\"}"
```

> 请求成功后返回样例：

```json
{
  "id": "68",
  "type": "file",
  "title": "无标题",
  "content": "<div></div>",
  "url": "https://shimo.im/docs/2da92f"
}
```

创建一篇文档，如果成功则返回文档的 URL。

### HTTP 请求

`POST https://api.shimo.im/files`

### 请求 Query

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
parentId | 否 | 无 | string | 如提供，则文件会创建于指定的文件夹中

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
type | 否 | `file` | string | 要创建的实体类型，`file` 或 `'folder'`

<aside class="notice">
成功后文档在石墨的 URL 可以从返回对象的 `url` 属性中获取，之后可以跳转到该 URL 以使用户继续编辑。
</aside>

## 获取文档内容

```http
GET /files/17 HTTP/1.1
Host: api.shimo.im
```

```shell
curl -X "GET" "https://api.shimo.im/files/N25Dhp2vTT4uHoKt"
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

```shell
curl -X "PATCH" "https://api.shimo.im/files/17" \
	-H "Content-Type: application/json" \
	-d "{\"creator\":\"79\"}"
```

修改文档，目前可以修改文档的管理员（默认情况下管理员即为文档的创建者）。

### HTTP 请求

`PATCH https://api.shimo.im/files/:fileId`

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
creator | 否 | 无 | string | 文档管理员的 id
