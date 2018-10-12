---
title: node干货之深入浅出1
date: 2017-06-03 20:00:50
tags: node
---
  之前写了关于requirejs源码分析  想必对浏览器模块化原理有一定理解 
那这次就对服务器端node的commonJS进行源码分析 先来讲讲关于require   
  服务器端的require是运行时加载 这个和es6的import不是一样的  es6的import是编译时加载  现在来分析require的字符串格式   
  require可以加载自己的核心模块 第三方模块 js文件  json文件 以及编译好的c++模块  (.node结尾的)  
加载顺序是 :  
>           1.先加载核心模块 比如 require("http");
>           2.试图在require的名称后面加上.js去搜索并加载
>           3. 试图在require的名称后面加上.json搜索并加载 列如 package.json  
>           4.试图在后面加上.node搜索编译好的c++模块   
>看一下require的查找   
        
		require(X) from module at path Y//请求模块x在路径y
		1. If X is a core module,   //如果x是核心模块
		   a. return the core module
		   b. STOP
		2. If X begins with '/'   如果x是开始/
		   a. set Y to be the filesystem root /在项目根路径找  可能会找到“package.json"
		3. If X begins with './' or '/' or '../'  如果X开始
			./或/或../
		   a. LOAD_AS_FILE(Y + X)  加载文件(x+y)后缀名依照上面顺序
		   b. LOAD_AS_DIRECTORY(Y + X) 或者找(x+y)的文件夹
		4. LOAD_NODE_MODULES(X, dirname(Y))  //找系统目录下的node_modules
		5. THROW "not found" //如果没有找到  抛出 not found
		
		LOAD_AS_FILE(X)
		1. If X is a file, load X as JavaScript text.  STOP
		2. If X.js is a file, load X.js as JavaScript text.  STOP
		3. If X.json is a file, parse X.json to a JavaScript Object.  STOP
		4. If X.node is a file, load X.node as binary addon.  STOP
		
		LOAD_INDEX(X)  //找文件夹的index
		1. If X/index.js is a file, load X/index.js as JavaScript text.  STOP
		2. If X/index.json is a file, parse X/index.json to a JavaScript object. STOP
		3. If X/index.node is a file, load X/index.node as binary addon.  STOP
		
		LOAD_AS_DIRECTORY(X)
		1. If X/package.json is a file,
		   a. Parse X/package.json, and look for "main" field.
		   b. let M = X + (json main field)
		   c. LOAD_AS_FILE(M)
		   d. LOAD_INDEX(M)
		2. LOAD_INDEX(X)
		
		LOAD_NODE_MODULES(X, START)
		1. let DIRS=NODE_MODULES_PATHS(START)  //找到项目路径的node_modules
		2. for each DIR in DIRS:
		   a. LOAD_AS_FILE(DIR/X)
		   b. LOAD_AS_DIRECTORY(DIR/X)
		
		NODE_MODULES_PATHS(START)
		1. let PARTS = path split(START)
		2. let I = count of PARTS - 1
		3. let DIRS = []
		4. while I >= 0,
		   a. if PARTS[I] = "node_modules" CONTINUE
		   b. DIR = path join(PARTS[0 .. I] + "node_modules")
		   c. DIRS = DIRS + DIR
		   d. let I = I - 1
		5. return DIRS  
官网 有句话这么在查找node_modules中说的 例require("express")
>If it is not found there, then it moves to the parent directory, and so on, until the root of the file system is reached.
>然后操作系统的node_modules中找   

参考[node的modules的api](https://nodejs.org/api/modules.html#modules_all_together)  
总结来说require的加载顺序 区分为带/和不带/的两种方式  先找缓存区
带/的就是找路径
如果不带/的 就先在核心模块找 如果是 stop 然后在项目的node_module
但是,只要首次加载成功后,node就会缓存起来,它缓存的是编译后的二进制模块,保证以后的加载速度  看一下源码注释  zzzzzzzzzz    
>					// Check the cache for the requested file.
					// 1. If a module already exists in the cache: return its exports object.
					// 2. If the module is native: call `NativeModule.require()` with the
					//    filename and return the result.
					// 3. Otherwise, create a new module for the file and save it to the cache.
					//    Then have it load  the file contents before returning its exports
					//    object. 
[module的源码地址](https://github.com/nodejs/node/blob/master/lib/module.js)