---
title: http2.0
date: 2017-07-07 20:16:15
tags: HTTP
---
 HTTP2.0是超文本传输协议2.0  下一代的HTTP协议 只适用于https://网址   
网页优化的一大关键局是要将传输的数据压缩为最小  而 HTTP2.0有很多关于这方面的优化   
接下来将HTTP1.1和HTTP2.0进行对比   
#http1.1
1. 持久连接  每个TCP连接开始都有三次握手  而开启持久化连接后不必要每次都握手   即request属性中的Connection 为keep-alive   
2. HTTP管道  持久HTTP多次请求满足先进先出  发送请求 等待响应完成 发送下一个请求   
单 HTTP1.1不允许多路复用  假如发送两个请求 记事客户端同时发送两个请求 先请求HTML 在请求CSS  而且CSS资源先准备就绪服务器一会先响应HTML资源 在响应 CSS  但是 HTTP2.0可以多路复用  按照优先级返回响应 
3. HTTP1.0增加请求和响应首部 方便能够交换有关请求和响应的元信息   
HTTP2.0新增首部压缩   
4. 连接和拼合   
	减少请求次数是性能优化的关键  HTTP1.X可以吧多个资源捆绑打包 通过一次网络请求获取   
	HTTP2.0可以多向请求和响应 消除了请求多个资源就是用多个TCP连接    
5. 嵌入资源   把资源嵌入文档中可以减少请求次数 
#http2.0
1. http2.0的性能增强  全在于二进制分帧层 作用是定义如何封装HTTP消息并在客户端和服务端之间传输    
2. 所用HTTP2.0通信都在一个连接上完成 连接承载任意数量的双向数据流 每个数据流以消息发送  消息由一个或者多个帧组成 HTTP2.0最小的通信单位是帧  帧可以乱序发送 根据帧首部的流标识重新组装 流可以并行的在同一个TCP连接上交换消息
3. 多向请求和响应  HTTP1.X中客户端想发送并行的请求和改进性能 必须使用多个TCP连接 HTTP2.0需要一个就行   
4. HTTP2.0新增请求优先级   根据浏览器请求优先的资源进行响应避免不必要的阻塞   
5. 服务器推送 HTTP2.0可以进行服务器的推送  
6. 首部压缩  
HTTP2.0连接的两端都知道已经发送那些首部  根据之前接受首部数据进行对比 只发送差异的部分  




参考[http2.0](http://www.cnblogs.com/strick/p/5658280.html)