# OAuth 授权

石墨鉴权接口遵循 OAuth 2.0 协议。OAuth 是一个能使得第三方应用在无需知道用户的密码的前提下读写用户私有资源的协议，第三方应用通过 OAuth 协议向用户请求授权后，可以获得一个 Access Token，此后即可使用该 Token 来代替用户密码来通过石墨开放 API 请求用户的私有资源。

石墨支持 Authorization Code, Password 和 Provider Exchange 三种 OAuth 授权方式。

## Authorization Code 方式

Authorization Code 方式是最通用的授权方式，该方式尤其适用于网站使用。

首先第三方应用将用户重定向至授权页面 `https://api.shimo.im/oauth/authorization`。

### 请求 Query

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
client_id | 是 | 无 | string | 应用的 ID
redirect_uri | 是 | 无 | string | 验证结果的回调网址
scope | 否 | 无 | string | 需要的权限列表，以空格分隔
state | 否 | 无 | string | 推荐传入一个无法猜测的随机字符串，用来防范 CSRF 攻击

在该页面中用户可以选择是否向第三方应用授权，而后石墨会跳转回第三方应用。如果用户同意授权，则石墨会传回一个临时口令（`code` 字段）以及在上一步传来的 `state` 参数值（如果提供）。

最后第三方应用在后台通过 `code` 的值向石墨 API 请求 Token，API endpoint 为 `POST https://api.shimo.im/oauth/token`。

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
grant\_type | 是 | 无 | string | 指定为 `"authorization_code"`
code | 是 | 无 | string | 上一步传回来的 `code` 参数
redirect\_uri | 是 | 无 | string | 和第一步传的 `redirect_uri` 参数一致

该 API 会返回 Access Token 和 Refresh Token：

## Provider Exchange 方式

```http
POST /oauth/token HTTP/1.1
Authorization: Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=
Content-Type: application/x-www-form-urlencoded
Host: api.shimo.im
Content-Length: 64

grant_type=provider_exchange&provider_user=user_id_here
```

```shell
# Authorization 的值遵循 HTTP Basic Auth，由 client_id:client_secret 构成
curl -X "POST" "https://api.shimo.im/oauth/token" \
	-H "Authorization: Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=" \
	--data-urlencode "grant_type=provider_exchange" \
	--data-urlencode "provider_user=user_id_here"
```

对于限定的第三方登录提供商，石墨提供 Provider Exchange 方式来允许第三方直接获取其平台用户在石墨的 Access Token。
例如某个用户 A 是通过 OAuth 服务提供商 B 登录的石墨，此时 B 想请求 A 在石墨的 Access Token，则只需要提供 A 在 B 的用户 ID 即可。

### HTTP 请求

`POST https://api.shimo.im/oauth/token`

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
grant\_type | 是 | 无 | string | 指定为 `provider_exchange`
provider_user | 是 | 无 | string | 目标用户在 provider 的用户 ID
