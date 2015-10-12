# 用户

## 获取当前用户资料

```http
GET /users/me HTTP/1.1
Host: api.shimo.im
```

```shell
curl -X "GET" "https://api.shimo.im/users/me"
```

> 请求成功后返回样例：

```json
{
  "id": "78",
  "name": "李明",
  "email": "liming@shimo.im",
  "avatar": "https://static.shimo.im/avatar/2e7a00413bc9.png"
}
```

获取当前登录用户的公开资料。

### HTTP 请求

`GET https://api.shimo.im/users/me`
