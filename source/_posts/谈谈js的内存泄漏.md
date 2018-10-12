---
title: 谈谈js的内存泄漏
date: 2017-05-30 13:06:00
tags: 前端
---
  javascript具有自动垃圾回收机制  执行环境会管理代码执行过程中的内存   
  原理: 垃圾收集器会定期中出不在使用的变量  然后释放其内存   
 > 1. 标记清除  
		js中最为常见的回收方式就是标记清除算法     
		当变量进入环境时  在函数声明一个变量将这个变量标记为'进入  环境' 从逻辑上讲 永远不能释放进入环境的变量所占用的内存       
		因为只要执行留进入相应环境  就可能用到它们 当变量离开环境  时  将其标记为离开环境    

	function test(){  
			let  aa = 'a';  //被标记 进入环境
		}
     test()  //执行完毕  aa被标记为离开环境被回收   

>	垃圾回收器会在运行时候给存储在内存中的所有变量加上标记   
	会去除环境中变量以及环境中变量引用的变量标记(闭包)  在此之后加上标记变量被视为准备删除的变量  最后 垃圾回收器完成清楚工作   
	到目前为止 IE,Firefox,Opera,Chrome,Safari实现的都是标记清除垃圾回收机制只不过垃圾收集时间不相同罢了 
 
> 2.引用计数   
	function test(){  
	let a = {};  //a的引用计数为0
    let b = a;	 // a的引用计数为1
	let c = a ;  //a的引用计数为2  
	let b = {} ; // a的引用计数减1  


  内存泄漏:  
   		内存泄漏是指一块被分配的内存既不能使用,又不能回收,直到浏览器进程自动结束   
	在C++中 因为是手动管理内存,内存泄漏是经常出现的事情    
	在C#和Java等语言采用自动垃圾回收方法管理内存 正常使用时不会发生泄漏 浏览器也是采用自动垃圾回收方法管理内存.但由于浏览器垃圾回收方法由bug 会产生内存泄漏   
  > 内存泄漏的几种情况:   
	1. 当页面中元素被移除或者替换时，若元素绑定事件仍没有被移除,在IE中会做出恰当的处理  要手动移除事件不然会产生内存泄漏   
	2.循环引用 （IE7/8） 
 
	let a = document.querySelector(".aa")
	let b = document.querySelector(".bb")
	a.f = b
	b.f = a   
	 
>    上方代码a b互相引用 虽然会在垃圾收集系统识别处理  但是在IE下如果 循环引用的是DOM或者是ActiveX对象 垃圾回收系统不会发现它们之间的循环关系与其他对象是隔离并释放它们  最终它们会被保存在内存中   解决办法手动解除循环引用   a.f = null
>     我们知道，IE中有一部分对象并不是原生js对象。例如，其内存泄露 DOM和BOM中的对  象就是使用C++以COM对象的形式实现的，而COM对象  的垃圾回收机制采用的就是引用计数   策略。因此，即使IE的js引擎采用标记清除策略来实现，但js访问的COM对象依然是基于引用计  数策略的。换句话说，只要在IE中涉及COM对象，就会存在循环引用的问题。
>           
    3. 闭包    
	  闭包会维持函数内局部变量 使其得不到释放    
	4. 自动类型装箱转换  
   
	 let a = 'aaaa'
	 console.log(a.length)   

> 	s本是string类型而非object 没有length属性  所以访问length  会自动创建一个临时的String 对象 封装s 这个对象一定会泄漏 解决方法  


	 console.log(new String(a).length)  
 
  	5. 使用全局变量  也会导致内存 泄漏    
  	6.  未清除的定时器  若定时器中的的元素被dom移除  定时器依然存在 同时定时器中的变量依然存在 不会被释放 
  >     let someResource = getData();
		setInterval(function() {
   		 let node = document.getElementById('Node');
    	if(node) {
        node.innerHTML = JSON.stringify(someResource));
   		 }
		}, 1000);    
            
                      
参考  [ 4 Types of Memory Leaks in JavaScript and How to Get Rid Of Them](https://auth0.com/blog/four-types-of-leaks-in-your-javascript-code-and-how-to-get-rid-of-them/)