---
title: 谈谈cookie
date: 2017-05-29 12:08:53
tags: 前端
---
cookie为持久保存客户端数据提供了方便 分担了服务器存储的负担,但还是有很多的局限性   
第一: 每个特定的域名最懂生成20个cookie   
IE和Opera会清理近期最少使用的cookie Firefox会随机清理cookie   
cookie最大大约有4096个字节 为了兼容性 一般不能超过4095个字节   
IE提供了一种存储可以持久化用户数据叫做userData 从IE5.0开始  
每个数据最多128k 每个域名最多1M   
cookie的优点:
具有极高的可扩展型和可用性   
通过加密和安全传输技术(SSL)减少cookie被破解的可能性   
可以控制cookie的生命周期   
缺点   
Cookie数量和长度的限制 每个域下最多只能有20条cookie  每个cookie长度不能超过4KB   
具有安全性的问题  如果cookie被人拦截了  被转发 可以 获得session信息   
有的用户在浏览器的禁用cookie   
