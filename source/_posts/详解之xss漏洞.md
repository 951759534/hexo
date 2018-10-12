---
title: 详解之xss漏洞
date: 2017-05-25 09:22:50
tags: 前端安全
---
	
  今天突然又翻一下关于xss漏洞的资料  xss又称css（Cross Site Scripting）跨站脚本攻击  
之前只是以为xss只是用户输入过滤不当而造成 然而发现原来里面的学问这么深   
    xss分为三种类型：存储型xss 反射性xss  dom-xss  
  存储型的xss  数据库中存有存在的xss攻击的数据，返回给客户端。若数据未被转义被浏览器渲染 有可能导致xss攻击  
  反射性xss 将用户输入存在的XSS攻击数据发送给后台 后台并未对数据进行存储 也未经过任何过滤 就有可能导致xss攻击  
  DOM-XSS
	用户点击链接参数带有js语言  后台未对url参数进行过滤 返回给客户端 前端直接从url中获得参数  
	例如利用BOM对象location中的href进行正则匹配获得带有xss漏洞的脚本参数 攻击者利用这个漏洞做出无法想象的事情 xss攻击无处在 如何进行防御 
###  xss漏洞的防御  
	 
对输入端和输出端统一进行过滤  
   		对于前端 有一个xss过滤很好的类库 毕竟术业有专攻 [传送门](https://github.com/leizongmin/js-xss)  
   		后端 有一个不同的使用方式，编码方式不同，java现成的工具可以用——ESAPI，不同位置如何转义可参照ESAPI文档，[传送门](https://github.com/ESAPI/esapi-java-legacy/tree/master/configuration/esapi)    比如属性值转义：

>String safe = ESAPI.encoder().encodeForHTMLAttribute( 
request.getParameter( "input" ) );   
   
php简单过滤:
> $safe = htmlspecialchars($this->input->get/post("input"))

前端要做好的工作 要有良好的css设计  html标签a的href进行规范化 避免用JavaScript打开一个新的窗口 不要使用全局变量 将不可信的数据例如白名单 提交数据时进行校验       --完