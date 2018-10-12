---
title: 关于meta的referer属性
date: 2017-06-06 23:00:34
tags: 前端
---
>
1. Http协议头中的Referer主要用来让服务器判断来源页面, 即用户是从哪个页面来的,通常被网站用来统计用户来源,是从搜索页面来的,还是从其他网站链接过来,或是从书签等访问,以便网站合理定位.  
2. Referer有时也被用作防盗链, 即下载时判断来源地址是不是在网站域名之内, 否则就不能下载或显示,很多网站,如天涯就是通过Referer页面来判断用户是否能够下载图片.  

在某些情况下，出于一些原因,想要控制页面发送给 server 的 referer 信息的情况下，可以使用这 referer metadata 参数
	(也可以用h5属性a标签rel="noreferer" 去掉refer)。 
>referer 的 metedata 参数可以设置为以下几种类型的值：  
>
				never  
				always   
				origin  
				default  
如果在文档中插入 meta 标签，并且 name 属性的值为 referer，浏览器客户端将按照如下步骤处理这个标签：

> 
 	1. 如果 meta 标签中没有 content 属性，则终止下面所有操作
	2. 将 content 的值复制给 referrer-policy，并转换为小写
	3. 检查 content 的值是否为上面 list 中的一个，如果不是，则将值置为 default

上述步骤之后，浏览器后续发起 http 请求的时候，会按照 content 的值，做出如下反应(下面 referer-policy 的值即 meta 标签中 content 的值)：

		1.如果 referer-policy 的值为never：删除 http head 中的 referer 
		例:
	 	<meta name="referrer" content="never">
		2.如果 referer-policy 的值为default：如果当前页面使用的是 https 协议，而正要加载的资源使用的是普通的 http 协议，则将 http header 中的 referer 置为空
		例:
	 	<meta name="referrer" content="default">
		3.如果 referer-policy 的值为 origin：只发送 origin 部分
		例:
		<meta name="referrer" content="origin">
		4.如果 referer-policy 的值为 always：不改变http header 中的 referer 的值，注意：这种情况下，如果当前页面使用了 https 协议，而要加载的资源使用的是 http 协议，加载资源的请求头中也会携带 referer
		例:
		<meta name="referrer" content="always">   
		
>不过https跳转http会有refer头部丢失问题 由于 http规定  确实请求头没有Referer头部，原来是因为HTTP协议规定：
>
		Clients SHOULD NOT include a Referer header field in a (non-secure) HTTP request if the referring page was transferred with a secure protocol.
手动输入网址也会丢失refer  
>  不要把Rerferer用在身份验证或者其他非常重要的检查上，因为Rerferer非常容易在客户端被改变。   


参考 [Meta referrer in wiki](https://wiki.whatwg.org/wiki/Meta_referrer)  
参考 [译文](http://www.freebuf.com/news/57497.html)