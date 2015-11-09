# OAuth 授权

石墨鉴权接口遵循 [OAuth 2.0 协议](https://tools.ietf.org/html/rfc6749)。OAuth 是一个使得第三方应用在无需知道用户的密码的前提下读写用户私有资源的标准协议，第三方应用通过 OAuth 协议向用户请求授权后，可以获得一个 Access Token，此后即可使用该 Token 来代替用户密码来通过石墨开放 API 请求用户的私有资源。

石墨支持 Authorization Code, Password 和 Refresh Token 三种 OAuth 授权方式，其中每种授权方式都是通过调用 `/oauth/token` 接口来最终获取 Access Token，该接口需要传送 "Authorization" 请求头，内容遵循 HTTP Basic 标准，格式为 `client_id:client_secret`。另外为了严格遵照 OAuth 2.0 规范，OAuth 相关的 API 均支持使用 "application/x-www-form-urlencoded" 格式传送数据。

## Authorization Code 方式

```http
POST /oauth/token HTTP/1.1
Authorization: Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=
Content-Type: application/x-www-form-urlencoded
Host: api.shimo.im
Content-Length: 64

grant_type=authorization&code=yourcode
```

```javascript
// 首先将用户重定向到授权页面：
res.redirect(shimo.oauth.authorization({
  redirect_uri: 'https://yourapp.tld/oauth/callback'
}));

// 然后在回调页面用返回的 code 请求 token：
shimo.oauth.token('authorization', {
  code: req.query.code
});
```

Authorization Code 方式是最通用的授权方式，该方式尤其适用于网站使用。

首先第三方应用将用户重定向至授权页面 `https://api.shimo.im/oauth/authorization`，需要提供的参数如下：

### 请求 Query

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
scope | 否 | 无 | string | 需要的权限列表，以空格分隔
client_id | 是 | 无 | string | 应用的 ID
redirect_uri | 是 | 无 | string | 验证结果的回调网址
response_type | 是 | 无 | string | 必须为 `"code"`
state | 否 | 无 | string | 推荐传入一个无法猜测的随机字符串，用来防范 CSRF 攻击

在该页面中用户可以选择是否向第三方应用授权，而后石墨会跳转回第三方应用。如果用户同意授权，则石墨会传回一个临时口令（`code` 字段）以及在上一步传来的 `state` 参数值（如果提供）。

最后第三方应用在后台通过 `code` 的值向石墨 API 请求 Token，API endpoint 为 `POST https://api.shimo.im/oauth/token`。

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
grant\_type | 是 | 无 | string | 指定为 `"authorization_code"`
code | 是 | 无 | string | 上一步传回来的 `code` 参数
redirect\_uri | 是 | 无 | string | 和第一步传的 `redirect_uri` 参数一致

该 API 会返回 Access Token 和 Refresh Token。

<aside class="notice">
scope 的有效值分别为 `public`, `read` 和 `write`，分别对应“获取用户公开资料”、“读取用户的私有文档”和“读写用户的私有文档”。
</aside>

## Password 方式

```http
POST /oauth/token HTTP/1.1
Authorization: Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=
Content-Type: application/x-www-form-urlencoded
Host: api.shimo.im
Content-Length: 64

grant_type=password&username=user@shimo.im@password=strongtoken
```

```javascript
shimo.oauth.token('password', {
  username: 'user',
  password: 'strongtoken'
});
```

Password 方式允许第三方应用通过用户的邮箱和密码获取 Access Token。目前此方式只开放给指定的第三方应用。

### HTTP 请求

`POST https://api.shimo.im/oauth/token`

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
grant\_type | 是 | 无 | string | 指定为 `password`
scope | 否 | 无 | string | 需要的权限列表，以空格分隔
username | 是 | 无 | string | 用户的邮箱
password | 是 | 无 | string | 用户的密码

## Refresh Token 方式

```http
POST /oauth/token HTTP/1.1
Authorization: Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=
Content-Type: application/x-www-form-urlencoded
Host: api.shimo.im
Content-Length: 64

grant_type=refresh_token&refresh_token=tokenhere
```

```javascript
shimo.oauth.token('refresh_token', {
  refresh_token: 'tokenhere',
  password: 'strongtoken'
});
```

Access Token 的有效期为一个小时，过期后需要使用 Refresh Token 刷新。Refresh Token 除非用户主动撤销，否则永久有效。

### HTTP 请求

`POST https://api.shimo.im/oauth/token`

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
grant\_type | 是 | 无 | string | 指定为 `refresh_token`
scope | 否 | 无 | string | 需要的权限列表，以空格分隔
refresh_token | 是 | 无 | string | Refresh Token
