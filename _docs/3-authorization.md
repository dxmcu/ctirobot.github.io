---
layout: single
title:  "认证"
date:   2019-07-26 17:53:16 +0800
last_modified_at: 2019-07-26 17:53:52 +08:00
permalink: /docs/authorization/
toc: true
---

## 2. 认证与授权

第三方客户对接开放平台指令层时，采用的是`OAuth2.0` 中的 `client_credentials` 流程。如果有微信公众号开发经验的话，应该不会太陌生，这个流程有点类似于微信公众号的认证方式。

`client_credentials`中需要的客户端凭据(`client_id:client_secret`)，需要找服务方申请获得，待激活后方可使用。

认证接口，采用 HTTP Basic 认证。完成认证后，接口会返回JWT Token。

Basic认证时，需要将客户端凭据按照 `client_id:client_secret` 拼接后 `Base64Encode`处理。

---

### 2.1 通用返回结果

开放平台所有的认证接口都相同形式的内容，但有时会根据不同的认证模式而出现微小的差异。

OAuth认证通用返回值说明:

Response: `200`

```json
{
    "access_token": "<jwt token>",
    "token_type": "bearer",
    "expires_in": 35999,
    "scope": "*",
    "apiKey": "<uuid>",
    "jti": "<uuid>"
}
```

| 参数 | 类型 | 说明 |
| ---- | ---- | ---- |
| access_token | String | token，如有必要，可用Base64解出Payload |
| token_type | String | Http请求头Authorization的前缀修饰符，如：Basic，Bearer |
| expires_in | number | 过期时长，单位秒，`这不是一个时间戳` |
| scope | String | 当前Token的作用域 |
| apiKey | String | 对调用方无用 |
| jti | String | 对调用方无用 |

Response: `40x`

```json
{
    "error": "<String>",
    "error_description": "<String>"
}
```

| 参数 |类型| 说明 |
| ---- | ---- | ---- |
| error | String | 错误原因 |
| error_description | String | 错误原因描述 |

---

### 2.2 认证接口

|名称|值|
|---|---|
|Method|GET/POST|
|Path|`api/uaa/oauth/token?grant_type=client_credentials`
|参数|无|
|请求头|Authorization: Basic <client_id:client_secret>

Postman请求示例截图:

![截图1](/img/2019-07-22-对接文档/postman-screenshot-1.jpg)

由于`access_token`存在过期时间，因此建议再您自己的后台维护一个续签逻辑，保障每隔`expires_in`秒（因网络延迟，实际会比它小）重新获取一次`access_token`。
