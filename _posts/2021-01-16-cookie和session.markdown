---
layout:     post
title:      "cookie和session的异同"
subtitle:   " \"cookie vs session\""
date:       2021-01-16 12:00:00
author:     "Yandong"
header-img: "img/post-bg-2021.jpg"
tags:
    - 计算机网络
---

## cookie和session的异同
1.cookie数据存放在客户的浏览器上，session数据放在服务器上。
2.cookie不是很安全，别人可以分析存放在本地的cookie并进行cookie欺骗，考虑安全应当使用session
3.session会在一定时间内保存在服务器上，当访问增多，会比较占用你服务器的性能，考虑到减轻服务器性能方面，应当使用cookie
4.单个cookie保存的数据不能超过4k,很多浏览器都限制一个站点最多保存20个cookie
5.可以考虑将登陆信息等重要信息存放在session，其他信息如果需要保留，可以放在cookie中

详情[这里](https://www.cnblogs.com/endlessdream/p/4699273.html)
