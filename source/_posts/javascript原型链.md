---
title: javascript原型链
date: 2017-08-01 20:42:31
tags: 前端
---
在javascript中  用__proto__属性表示一个对象的原型链  当查找一个对象的属性时,javascript会向上遍历原型链 知道找到指定名称的属性为止   
实现扩展方法
>实现一个对象ObjectExtend的扩展 
>     Object.prototype.Extend = function(objectExtend,object){
>     
>     for(var key in object){
        if (object.hasOwnProperty(key) && objectExtend[key] === undefined) {
             objDestination[key] = objSource[key];
         }
>     }
>     return objectExtend;
>     }
上述例子实现将object属性 给objectExtend中   
Object类是所有类的父类 父类中的内置方法和定义方法子类会继承   
