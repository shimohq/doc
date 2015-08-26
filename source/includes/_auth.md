# 鉴权

石墨鉴权接口遵循 OAuth 2.0 协议。

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
