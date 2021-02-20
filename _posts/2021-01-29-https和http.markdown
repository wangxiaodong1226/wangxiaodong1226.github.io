---
layout:     post
title:      "https和http的区别"
subtitle:   " \"https我是加密的\""
date:       2021-01-29 12:00:00
author:     "Yandong"
header-img: "img/post-bg-2021.jpg"
tags:
    - 计算机网络
---

## 说一下http和https的区别
* `http协议需要申请CA证书，费用较高`。
* `http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议`
* `http协议的端口是80，https的端口是443`
* `http的连接是无状态的(每次的请求都是独立的)，https协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全,有效的防止运营商劫持`
提示：https的ssl加密是在传输层实现的
![java-javascript](/img/网络/14.png)