---
layout: single
title:  "接口"
date:   2019-07-26 17:53:16 +0800
last_modified_at: 2019-07-26 17:53:52 +08:00
permalink: /docs/interface/
toc: true
---

业务接口需要使用认证接口中返回的`access_token`。
按照`OAuth2.0`的使用标准，`access_token`可以直接使用在 `http(s)://<domain>/<path>?access_tken=<access_token>` 

但是，**这里不推荐这么做**。因为认证采用的JWT方法，内容过长，建议直接存放到`Header`里:
> HTTP Header: `Authorization:Bearer <access_token>`

或者在使用postman调试时，选用下面的配置:
![截图2](/img/2019-07-22-对接文档/postman-screenshot-2.jpg)
找到Authorization选项卡 -> Type=Breaer Token -> Token 填入获得的<access_token>

上面这个操作等效于:
![截图3](/img/2019-07-22-对接文档/postman-screenshot-3.jpg)