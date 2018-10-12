---
title: requireJS源码解析
date: 2017-06-01 19:27:01
tags: 前端 js库
---
浅谈requireJS源码   版本 2.3.3
开始是这样的   
作者定义一个立即执行函数 有两个参数 global  和 setTimeOut  
匿名函数开始是作者自己结合原生js定义的一些方法  
例如:
>  function isFunction(it) {
        return ostring.call(it) === '[object Function]';  
    }  
 ostring是什么?  原来是 Object.prototype.toString     

大致流程是  
newContext函数开始
设置handlers对象     
设置Module类  
设置context对象 
newContext函数完  返回值context对象
设置req函数
执行req({})
设置define函数
最后再次执行req({cfg})函数
>   ->   newContext 中的部分代码 代码太多 不罗列
  		开始定义一堆属性  然后定义一下内部要用到的函数     设置handlers对象   

	 有三个属性值 require  exports  module  

>   ->  设置Module类   模块类

>   ->  设置context对象  


		关键函数  configure 主要设置cfg的属性   makeShimExports函数  对shime进行特殊处理(即加载特殊要提前依赖的模块)
        makerequir函数 定义加载的依赖模块函数   
		其中调用mix混合对象属性  
		nameToUrl函数 将模块名转换为路径 
		load函数  调用req的load
 		completeLoad函数 完成加载函数  
		onScriptLoad函数  检查加载状态 如果加载完成 
        interactiveScript的值置为null   继续调用completeLoad函数  completeLoad继续调用takeGlobalQueue 
		onScriptError 检查script标签错误
		好像	少了点什么  诶   checkLoaded  检查加载的 
		在completeLoad函数中的最后调用checkLoaded
		看一下核心函数 
		 completeLoad: function (moduleName) {  //接受参数模块名称
                var found, args, mod,
                    shim = getOwn(config.shim, moduleName) || {},
                    shExports = shim.exports;
                takeGlobalQueue();    //推入队列
                while (defQueue.length) {
                    args = defQueue.shift();
                    if (args[0] === null) {
                        args[0] = moduleName;
                        if (found) {
                            break;
                        }
                        found = true;
                    } else if (args[0] === moduleName) {
                        found = true;
                    }

                    callGetModule(args);
                }
                context.defQueueMap = {};
                mod = getOwn(registry, moduleName);

                if (!found && !hasProp(defined, moduleName) && mod && !mod.inited) {
                    if (config.enforceDefine && (!shExports || !getGlobal(shExports))) {
                        if (hasPathFallback(moduleName)) {
                            return;
                        } else {
                            return onError(makeError('nodefine',
                                             'No define call for ' + moduleName,
                                             null,
                                             [moduleName]));
                        }
                    } else {
                        callGetModule([moduleName, (shim.deps || []), shim.exportsFn]);
                    }
                }

                checkLoaded();
            }
		

   
>   -> 核心函数req  接受四个参数  模块名数组  执行函数  错误处理 选项
>       
       req = requirejs = function(deps,callback,errback,optional){...}
       if (!isArray(deps) && typeof deps !== 'string') {  //如果第一个参数不是数组 是一个定义对象 
            config = deps;       
            if (isArray(callback)) {
                deps = callback;
                callback = errback;
                errback = optional;
            } else {
                deps = [];
            }
        }
        if (config && config.context) {    
            contextName = config.context;
        }
        context = getOwn(contexts, contextName);
        if (!context) {
            context = contexts[contextName] = req.s.newContext(contextName);
        }
        if (config) {
            context.configure(config);
        }
        return context.require(deps, callback, errback);  //调用context的require  间接调用context 的makeRequire  返回闭包函数 localRequire 然后调用  zzzzzzzzz  执行 intakeDefines（）函数  intakeDefines执行 takeGlobalQueue 将globalDefQueue push进context的队列 然后置为空数组
				/* 检测 入口 data-main */  
      if (isBrowser && !cfg.skipDataMain) {  //判断是否是浏览器环境          isBrowser = !!(typeof window !== 'undefined' && typeof navigator !== 'undefined' && window.document),
        eachReverse(scripts(), function (script) {
            if (!head) {
                head = script.parentNode;
            } dataMain = script.getAttribute('data-main');
            if (dataMain) {
			if (!cfg.baseUrl && mainScript.indexOf('!') === -1) {
                    src = mainScript.split('/');
                    mainScript = src.pop();
                    subPath = src.length ? src.join('/')  + '/' : './';
                    cfg.baseUrl = subPath;
                }
                mainScript = mainScript.replace(jsSuffixRegExp, '');
                if (req.jsExtRegExp.test(mainScript)) {
                    mainScript = dataMain;
                }    
                cfg.deps = cfg.deps ? cfg.deps.concat(mainScript) : [mainScript];   //将data-main的属性值推入dep中
                return true;
            }
        });
    
define函数   

requireJS作者是这样定义的   
> define = function(name,deps,callback){...}
查阅官方api  define有三种写法   
第一种  包含三个参数  第一个参数是 模块的名称 第二个是引入的模块   
第三个是 执行的函数 
第二种  包含两个参数  第一个参数 是要引入的模块  第二个是要执行的函数
第三种  包含一个参数  就是执行的函数   
源码是这样写的 外加我的注释 
>
 
      /*  参数逻辑判断  */
    if (typeof name !== 'string') {   //判断第一个参数是否是字符串 如果不是
            callback = deps;   //后面的参数向前移一位
            deps = name;
            name = null;
        }
        if (!isArray(deps)) {    //deps要引入模块是否是数组  如果不是  
            callback = deps;      //后面的参数前移
            deps = null;
        }
		/*如果传入参数中没有数组*/
        if (!deps && isFunction(callback)) {   
            deps = [];
            if (callback.length) {  //  判断callback实际接收的参数
                callback
                    .toString()  // 将可执行函数转换为字符串
                    .replace(commentRegExp, commentReplace) /* 这里是个正则匹配 commentRegExp = /\/\*[\s\S]*?\*\/|([^:"'=]|^)\/\/.*$/mg 用来过滤注释用的*/
                    .replace(cjsRequireRegExp, function (match, dep) {
                        deps.push(dep);  //将require字段引入的push到dep中   
                    });
                deps = (callback.length === 1 ? ['require'] : ['require', 'exports', 'module']).concat(deps);
            }
        }
	 if (useInteractive) { //useInteractive在文初被设为false  这个应该是判断是IE(6-8)script标签做的特殊处理  (getInteractiveScript函数)ie6-8不支持script标签 支持onreadystatechange属性 有“uninitialized” – 原始状态 “loading” – 下载数据中..“loaded” – 下载完成 “interactive” – 还未执行完毕. “complete” – 脚本执行完毕.
            node = currentlyAddingScript || getInteractiveScript();
            if (node) {
                if (!name) {
                    name = node.getAttribute('data-requiremodule'); //获得引入模块的名字
                }
                context = contexts[node.getAttribute('data-requirecontext')]; //找出模块中的名字
            }
        }  
 			if (context) {             //将此模块加入队列中 
            context.defQueue.push([name, deps, callback]);    
            context.defQueueMap[name] = true;
        } else {
            globalDefQueue.push([name, deps, callback]);
        }

看一下执行代码  
>  
    req.exec = function (text) {     /*将string文本eval  zzzzzzzzzzz*/
        return eval(text);
    };
触发是 req.load这个函数开始执行 第二个参数是主要参数 即moduleName   context.completeLoad(moduleName)
模块加载完成执行 onScriptLoad   然后 再次执行completeLoad然后一层层加载                      -end  
                       
 --转载声明出处    


 