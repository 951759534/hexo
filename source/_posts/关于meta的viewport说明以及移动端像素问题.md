---
title: 关于meta的viewport说明以及移动端像素问题
date: 2017-06-05 11:11:44
tags: 移动端 前端
---
在html的结构中  head中有meta标签中的 其中meta标签有viewport属性  如下
> `<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no" />`  
>  width：控制 viewport 的大小，可以指定的一个值，如果 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。  
height：和 width 相对应，指定高度。  
initial-scale：初始缩放比例，也即是当页面第一次 load 的时候缩放比例。  
maximum-scale：允许用户缩放到的最大比例。  
minimum-scale：允许用户缩放到的最小比例。  
user-scalable：用户是否可以手动缩放    
总的来说viewport就是控制用户是否能在移动端放大或者缩小的一个meta属性 有一些允许最大缩放比例和最小缩放比例   

接下来就要说说移动端手机像素的问题  
1.设备像素  指的是设备中使用的物理像素(Physic pixel)。这个单位用px表示，它是一个[相对绝对单位]:
>即在同样一个设备上，每1个设备像素所代表的物理长度(如英寸)是固定不变的(即设备像素的绝对性);   
但在不同的设备之间，每1个设备像素所代表的物理长度(如英寸)是可以变化的(即设备像素的相对性);

2.what?说的是在不同设备之间 设备像素是变化的？那么 这有给前端开发工程师带来了难题  zzzzzzz  接下来说明一下大家买手机 介绍总有什么什么的分辨率 譬如  1920*1080p的 那么 根据公式 ppi = 1920的平方乘以1080的平方 除以 屏幕尺寸   屏幕尺寸就是手机的尺寸 买手机的参数会有说明 举个栗子就明白了  5英寸 5.5英寸 5.7英寸 这些数字应该很熟悉吧 zzzzzzzzz   
>讲了半天 ppi有什么用呢 专业的话说 用于度量计算机显示屏上像素的密度  即每英寸屏幕所拥有的像素数 例如魅族手机 有500dpi（瞎写的）  
>就是一英尺能有500个像素 dpi越大 一英尺所容纳的像素越多 则字越小 zzzzzzzzzzzz 但是又想在不同手机中展示相同的尺寸 怎么办呢   

3.我能想到的就是在移动端用rem  可能有更好地方法 但是我感觉我这方法简单定义好body字体 然后呢？ 我使用css预编译语言stylus   
[带个张鑫旭老师stylus文档传送门](http://www.zhangxinxu.com/jq/stylus/)  
定义一个mixin  
>     rem($val) 
	  $val/$default rem
说了这么多 解决方法就是这么简单zzzzzzzzzz 哈哈 



                                                          --end