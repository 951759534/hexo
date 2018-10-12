---
title: 来谈谈AMD-CMD-commonJS
date: 2017-06-01 18:10:41
tags: 前端
---
> CommonJS规范加载模块是同步的,只有加载完成才能执行后面的操作  而      AMD规范是非同步加载模块  允许指定回调函数 由于nodeJS主要用于服务        端的编程,模块文件一般都存在本地硬盘 所以加载起来比较快 不用考虑非同步  的加载方式所以commonJS规范比较实用 但是如果是浏览器环境  要从服务器  加载模块 就必须采用非同步的方式 因此浏览器一般采用AMD规范     
AMD规范：

	define(['package/lib'], function(lib){
	  function foo(){
	    lib.log('hello world!');
	  }
	
	  return {
	    foo: foo
	  };
	});  


>  AMD("Asynchronous Module Definition")规范允许输出的模块兼容CommonJS规范，这时define方法需要写成下面  这样：

	define(function (require, exports, module){  
    var someModule = require("someModule");  
    var anotherModule = require("anotherModule");  
    someModule.doTehAwesome();  
    anotherModule.doMoarAwesome();  
    exports.asplode = function (){      
    someModule.doTehAwesome();    
    anotherModule.doMoarAwesome();  
      };  
    });    

CMD("Common Module Definition")是 SeaJS 在推广过程中对模块定义的规范化产出   
CMD和AMD的区别有以下几点： 

>1.对于依赖的模块AMD是提前执行，CMD是延迟执行。不过RequireJS从2.0开始，也改成可以延迟执行（根据写法不同，处理方式不通过）。   
>2.CMD推崇依赖就近 用到才去require，AMD推崇依赖前置。  
 虽然 AMD也支持CMD写法，但依赖前置是官方文档的默认模块定义写法。  
