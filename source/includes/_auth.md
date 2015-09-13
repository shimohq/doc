# 鉴权

石墨鉴权接口遵循 OAuth 2.0 协议。OAuth 是一个能使得第三方应用在无需知道用户的密码的前提下读写用户私有资源的协议，第三方应用通过 OAuth 协议向用户请求授权后，可以获得一个 Access Token，此后即可使用该 Token 来代替用户密码来通过石墨开放 API 请求用户的私有资源。

石墨支持如下几种 OAuth 授权方式：

## Authorization Code 方式

Authorization Code 方式是最通用的授权方式，该方式尤其适用于网站使用。

### 将用户重定向至授权页面

`GET https://api.shimo.im/oauth/authorization`

### 请求 Query

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
client_id | 是 | 无 | string | 应用的 ID
redirect_uri | 是 | 无 | string | 验证结果的回调网址
scope | 否 | 无 | 需要的权限列表，以空格分隔
state | 否 | 无 | 推荐传入一个无法猜测的随机字符串，用来防范 CSRF 攻击

### 石墨跳转回第三方应用

如果用户同意授权，则石墨会通过 `code` 字段传回一个临时口令以及在上一步传来的 `state` 参数值（如果提供）。

### 通过 `code` 请求 Token

`POST https://api.shimo.im/oauth/token`

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
grant_type | 是 | 无 | string | 指定为 `"authorization_code"`
code | 是 | 无 | string | 上一步传回来的 `code` 参数
redirect_uri | 是 | 无 | string | 和第一步传的 `redirect_uri` 参数一致

## Provider Exchange 方式

```http
POST /oauth/access_token HTTP/1.1
Authorization: Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=
Content-Type: application/x-www-form-urlencoded
Host: api.shimo.im
Content-Length: 64

grant_type=provider_exchange&access_token=your+access+token+here
```

```shell
# Authorization 的值遵循 HTTP Basic Auth，由 client_id:client_secret 构成
curl -X "POST" "https://api.shimo.im/oauth/access_token" \
	-H "Authorization: Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=" \
	--data-urlencode "grant_type=provider_exchange" \
	--data-urlencode "access_token=your access token here"
```

对于限定的第三方登录提供商，石墨提供 Provider Exchange 方式来允许第三方直接获取其平台用户在石墨的 Access Token。
例如某个用户 A 是通过 OAuth 服务提供商 B 登录的石墨，此时 B 想请求 A 在石墨的 Access Token，则只需要提供 A 通过 B 登录
石墨时 B 颁发给石墨的 Access Token 即可。

### HTTP 请求

`POST https://api.shimo.im/oauth/access_token`

### 请求 Body

参数 | 必选 | 默认值 | 类型 | 描述
--------- | ------- | ------- | ------- | -----------
grant_type | 是 | 无 | string | 指定为 `provider_exchange`
access_token | 是 | 无 | string | 目标用户在 provider 的 Access Token
