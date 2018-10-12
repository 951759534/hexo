---
title: Velocity.js介绍及使用
date: 2017-05-19 22:45:35
tags: js库
---
在Web上使用CSS实现动画并不是唯一的方式，我们也可以使用JS来实现，并且JS还有一些CSS无法替代的优势。  
Velocity模仿了jQuery的语法，可以完美地同jQuery协作  
用法  
>$element.velocity("scroll", 1000); 
其中$element是dom结点  
>$element.velocity({ left: "50px" }, 500, "ease-in-out",callback);  
>这个用过jQuery的很好懂了吧  

>$element
  .velocity("scroll", { duration: 1000 })
  .velocity({ opacity: 1 });
//默认的滚动沿y轴方向，想改到x轴方向的话可以使用axis选项：
$element.velocity("scroll", { axis: "x" });  
Transforms

想要结合CSS和JS设计动画？ 设置一些CSS变换规则，允许你做一些2D或3D的动画，比如平移，扩大，旋转。注意这些变换不会影响元素在网页中的位置，也不会影响该元素周围 的元素在页面中的位置。 Velocity支持下面的变换：
>translateX: 从左向右沿x轴移动元素  
translateY: 从上到下沿y轴移动元素  
rotateZ: 关于z轴旋转元素    
rotateX: 关于x轴旋转元素（看起来由里向外）
rotateY: 关于y轴旋转元素（由左到右）  
scaleX: 成倍数改变元素宽度  
scaleY: 成倍数改变元素高度    
  
tip:要用velocity要先引入哦！			 	
			                 --end  
参考文章 [Intro to JavaScript Animation](https://davidwalsh.name/intro-javascript-animation "Intro to JavaScript Animation")