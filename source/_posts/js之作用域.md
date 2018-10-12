---
title: js之作用域
date: 2017-06-03 21:33:32
tags: 前端 js
---
>下面一行代码

	var value = 1;
	function foo() {
	    console.log(value);
	}
	function bar() {
	    var value = 2;
	    foo();
	}
	bar();  //结果是?
因为 JavaScript 采用的是词法作用域，函数的作用域在函数定义的时候就决定了。
而与词法作用域相对的是动态作用域，函数的作用域是在函数调用的时候才决定的。  
如果是动态作用域 就会调用函数的作用域 则答案是 2
答案 是 1  
参考  [伢羽:JavaScript深入之词法作用域和动态作用域](https://github.com/mqyqingfeng/Blog/issues/3)
