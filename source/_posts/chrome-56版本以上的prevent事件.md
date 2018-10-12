---
title: chrome-56版本以上的prevent事件
date: 2017-05-20 23:25:53
tags: 移动端
---
>Unable to preventDefault inside passive event listener due to target being treated as passive. See https://www.chromestatus.com/...
不能阻止浏览器默认行为 
当你触摸滑动页面时，页面应该跟随手指一起滚动。而此时你绑定了一个 touchstart 事件，你的事件大概执行 200 毫秒。这时浏览器就犯迷糊了：如果你在事件绑定函数中调用了 preventDefault，那么页面就不应该滚动，如果你没有调用 preventDefault，页面就需要滚动。但是你到底调用了还是没有调用，浏览器不知道。只能先执行你的函数，等 200 毫秒后，绑定事件执行完了，浏览器才知道，“哦，原来你没有阻止默认行为，好的，我马上滚”。此时，页面开始滚。  

oh no  所以为了让页面滚动变得更为流畅，从 chrome56 开始，在 window、document 和 body 上注册的 touchstart 和 touchmove 事件处理函数，会默认为是 passive: true。浏览器忽略默认事件
preventDefault() 可以第一时间滚动了。  
解决方案
> 注册事件是添加 { passive: false }  
> window.addListenEvent("touchstart",fn,{passive:  false})  
  可以阻止默认事件但是也同时降低了性能    

参考[Making touch scrolling fast by default](https://developers.google.com/web/updates/2017/01/scrolling-intervention)   需科学上网

尽管对于移动端的Safari来说这仍然很有必要,网站不应该依赖在touchstart和touchmove监听器上调用preventDefault()来阻止默认行为.因为在Chrome中,这样做已经不被支持和提倡了.开发者应该在那些需要禁用滚动和缩放的元素上增加touch-actionCSS属性,好在任何touch事件出现前通知到浏览器.为了阻止tap(就像是一个click事件的产生)的默认行为,在touchend监听器中调用preventDefault().  
这就是第二种方案  
应用 CSS 属性 touch-action: none; 这样任何触摸事件都不会产生默认行为，但是 touch 事件照样触发。  
参考 [css文档](https://w3c.github.io/pointerevents/#the-touch-action-css-property)  